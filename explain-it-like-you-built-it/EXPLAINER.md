# Explain It Like You Built It

**Track:** General AI Fluency · **Week:** 5

## The piece I picked

The **idempotency keys** in my FlyRank backend capstone — an LLM usage-metering
and billing service. Specifically: how the API guarantees that if a client
retries a request (because the network timed out, say), the customer is never
charged twice.

## The plain-words explanation

Imagine you tap "pay" on your phone, the screen freezes, and you tap it again.
Did the first tap go through? If the app charges you twice, that's a bug — and
in billing, it's the kind of bug that loses customers.

My API solves this with something called an **idempotency key**. Think of it
like a coat-check ticket:

1. Before the client sends a "record this usage" request, it makes up a random
   ticket number (a UUID) and staples it to the request in an `Idempotency-Key`
   header. The header is required — no ticket, no service (the API answers 400).
2. The server checks the ticket drawer first. Seen this ticket before? It
   doesn't redo anything — it just hands back the stored result from last
   time, word for word.
3. New ticket? The server computes the cost and files the ticket **together
   with** the charge in one single database save. There is no moment where the
   charge exists but the ticket doesn't, so a retry can never slip through the
   crack.

The part I found most interesting — the one I didn't understand until an AI
tutor walked me through it — is the **race**. What if two retries arrive at the
*exact same millisecond*? Both check the drawer, both see "new ticket", both
try to file. The safety net is a database rule: `UNIQUE(tenant_id,
idempotency_key)` — the drawer physically cannot hold two tickets with the
same number. Exactly one insert wins; the loser catches the collision, throws
away its half-written work, and returns the winner's result. Either way the
customer is charged exactly once, even when the timing is adversarial.

And one design rule matters more than the code: **the ticket must come from
the client, not the server.** If the server made up the ticket, a retried
request would get a *new* ticket and look like a brand-new charge. The whole
trick only works because the *client* reuses its ticket when it retries.

## Why this piece

It's the smallest part of the capstone that carries the biggest idea: in
billing, **correctness isn't a feature, it's the product**. And it's the part
I'd be proudest to defend in an interview — I can explain the failure mode
(the double-charge on retry), the fix (client-supplied ticket + unique
constraint), and the nasty edge case (the same-millisecond race), all without
looking at the code.
