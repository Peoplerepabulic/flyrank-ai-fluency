# Design Your Personal Agent (FL-06)

**Track:** General AI Fluency · **Week:** 5 · **Estimated:** 4h

## The job (one job, done well)

**Job-search inbox triager.** I get a high volume of recruiting email:
application confirmations, rejections, interview invites, online assessments,
deadlines. The agent's one job: every morning, read the last 24h of
job-search mail and produce a short digest — what needs action today, what
changed, what's noise. Nothing else. Not a chatbot, not a general assistant:
one job.

**User & frequency:** me, once daily (~8am CT). On demand if I ask.

## Tools & data sources (each with a realistic access plan)

1. **Gmail read** — I already have a working Gmail skill/CLI in my environment
   (`hatch_gws_cli gmail +triage`); the agent calls the same interface.
   Realistic: proven, no new auth needed.
2. **Local application tracker** — a simple JSON/CSV I maintain of every
   application (company, role, req ID, status, date). The agent reads it to
   match emails to applications ("this rejection is for req R-288578").
   Realistic: a file I own, no integration risk.
3. **Calendar (optional, v2)** — only for interview scheduling conflicts.
   Deferred: not needed for the MVP.

No tool is speculative; every one above already works in my setup today.

## Platform choice (argued against an alternative)

**Chosen: a scheduled Python script (cron) + Gmail CLI + file tracker.**

- Why: the job is batch, not conversational — it runs once a day, reads,
  classifies, writes a digest. A script is the honest shape for that. Zero
  new dependencies, full control over prompts, trivially debuggable, and the
  Gmail access path is already proven.
- **Alternative considered: n8n workflow.** n8n would give a visual builder
  and easy scheduling, and it's the "no-code agent" answer. I ruled it out
  because the classification step needs careful prompt iteration with real
  examples, which is faster to iterate in code than in n8n nodes — and
  self-hosting n8n is another service to babysit for a job that runs
  5 minutes a day.
- A Claude Project + connectors was also considered and rejected: great for
  interactive triage, wrong shape for an unattended morning digest.

## Eval cases (defined before building — 6)

1. Rejection email → classified "rejection", matched to the right application
   in the tracker, no action flagged.
2. Interview invite with scheduling link → "action: schedule", extracted date
   window, flagged as today's priority.
3. Online assessment invite with 72h deadline → "action: assessment", deadline
   extracted and highlighted.
4. "Application received" confirmation → "noise/log", status updated in
   tracker, not flagged.
5. Recruiter outreach for a role I never applied to → "review", surfaced
   separately (not auto-filed).
6. Ambiguous: email mentions two companies → agent must not guess; flags
   "needs human" rather than misfiling.

Pass bar for MVP: 6/6 correct classification on a labeled set of 20 real
emails before it touches the daily digest.

## Guardrails (high-risk / irreversible actions)

- **Read-only by default.** The agent may read mail and update the local
  tracker file. It may NOT send, reply, archive, or label anything in Gmail
  in v1 — every suggested action goes in the digest for me to do.
- **Never auto-respond to employers.** No draft replies, no "looks good,
  send it" buttons. The cost of a wrong auto-reply is unbounded; the digest
  is the interface.
- **Uncertain → human.** If classification confidence is low or an email
  matches no known application, it goes in a "needs your eyes" section —
  never silently filed.
- **No credential sprawl.** Gmail access reuses the existing authenticated
  CLI; no new tokens, no OAuth scopes beyond read.

## What v1 deliberately excludes

Sending mail, calendar writes, interview scheduling, cover-letter drafting.
Each is a real temptation and each multiplies the blast radius. The agent
earns those in v2 by being boringly correct at v1 for a month.
