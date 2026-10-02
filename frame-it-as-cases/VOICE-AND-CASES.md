# Frame It as Cases: Work That Speaks for Itself

**Track:** General AI Fluency · **Week:** 2 · **Estimated:** 3h

## Voice card

**Plain-spoken, evidence-first, quietly confident.**

(Everything below is written to sound like me saying it, not a template.)

---

## Case 1 — LLM Usage Metering & Billing Service

**Problem.** When you sell LLM API calls, you have to meter every token and
bill for it — and if a client's network hiccups and it retries, you must never
charge twice. Double-charging is the kind of bug that ends customer trust.

**What I did.** Built a FastAPI service (my FlyRank backend capstone) where
every billable request carries a client-supplied idempotency key: seen the key
before, return the stored result verbatim; new key, compute cost and save key
+ charge in one atomic commit. A `UNIQUE(tenant_id, idempotency_key)`
constraint is the backstop — even two retries landing in the same millisecond
resolve to exactly one charge (the loser rolls back and returns the winner's
result). 13 tests, 5 acceptance probes, all passing; default billing provider
is an honest $0 simulator with a real Stripe test-mode path implemented.

**Outcome.** Submitted for mentor review as my backend capstone. The piece I'd
defend in any interview: I can explain the failure mode, the fix, and the race
condition — all without looking at the code.
[Repo](https://github.com/Peoplerepabulic/flyrank-capstone-metering-billing)

## Case 2 — The Polite Scraper

**Problem.** I needed real data (60 books: title, price, rating) from a live
site without getting blocked or being a bad citizen of the web.

**What I did.** Wrote a scraper that checks robots.txt first, identifies
itself honestly, waits a full second between requests, retries with backoff,
and validates every record against a strict schema — bad records get skipped,
never crash the run. 60/60 books scraped and validated on the real run.

**Outcome.** A reusable pattern I now reach for whenever I need web data: be
polite first, validate everything, never let one bad page kill the pipeline.
[Repo](https://github.com/Peoplerepabulic/flyrank-polite-scraper)

## Case 3 — AI Decision Flow (React Flow + Inngest)

**Problem.** AI decisions buried in code are invisible: you can't see the
flow, test a branch, or explain to anyone what the system actually does.

**What I did.** Built a Next.js app where the whole decision workflow is a
visual graph — drag nodes, connect edges, edit each node's prompt in a side
panel, then run it through Inngest as background jobs with a timestamped
execution log. TypeScript build passes; traversal verified.

**Outcome.** Decisions you can point at. The demo I show when someone asks
whether I can make AI systems legible, not just functional.
[Repo](https://github.com/Peoplerepabulic/flyrank-ai-decision-flow)

---

## Bio

Qiwei Li — WashU senior, Computer Science + Mathematics (May 2027). I build
backend systems where correctness is the product: metering, scraping, and AI
workflows that survive retries, bad input, and real review. Kaggle silver
medal (42nd of 1,446). Fluent in English and Chinese.

## Contact / CTA

The full code is on GitHub — start with the case above that matches your
problem. Want the story behind a decision? Email me: lqiwei@wustl.edu.

---

## AI draft vs. revised (one example, honest)

**AI's first draft of Case 1's "What I did" beat:**

> "I developed a robust, cutting-edge billing microservice leveraging FastAPI
> and idempotency patterns to ensure data integrity at scale."

**What I changed and why:** "robust, cutting-edge, leveraging, at scale" —
every word is hype, zero information. I replaced it with the actual mechanism
(client-supplied key → verbatim replay → atomic commit → unique constraint as
backstop) because a hiring manager believes mechanisms, not adjectives. The
revision is longer, but every sentence is checkable against the repo. That's
the voice card working: evidence-first means deleting every word I can't
prove.
