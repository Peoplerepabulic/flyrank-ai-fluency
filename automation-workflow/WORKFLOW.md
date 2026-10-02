# Ship an Automation Workflow v2 (FL-04)

**Track:** General AI Fluency · **Week:** 4 · **Estimated:** 7h · **Actual:** ~4h

## Why this workflow

I'm running a high-volume Summer 2027 job search (dozens of applications).
Every application needs the same three things: the posting's facts, a fit
assessment against my resume, and tailored notes on what to emphasize. Doing
that from scratch each time is slow and inconsistent — so I built it as a
repeatable 3-step pipeline.

**Implementation note (honest):** this is defined as a structured,
tool-agnostic prompt pipeline — three steps with fixed prompts and defined
handoffs, executed the same way every time. I ran it manually five times on
real inputs (below); the prompts are portable as-is into a Claude Project,
a custom GPT, or an n8n chain. The value being demonstrated is the pipeline
design and its documented runs, not any particular platform.

## Step diagram

```
 ┌──────────┐    posting facts     ┌──────────────┐   fit assessment   ┌───────────┐
 │ 1. GATHER │ ──────────────────▶ │ 2. SYNTHESIZE │ ─────────────────▶ │ 3. DRAFT  │
 │ (facts)   │    role, pay, loc,   │ (judgment)    │  strengths/gaps/  │ (notes)   │
 └──────────┘    reqs, auth text    └──────────────┘  risks             └───────────┘
      │                                    │                                  │
      ▼                                    ▼                                  ▼
 structured fact sheet              fit score + risk flags            application notes
```

**Handoffs:** Step 1 outputs a fixed-schema fact sheet (role / company /
location / pay / requirements / work-authorization wording / deadline). Step 2
may only use that sheet + the resume — no new research. Step 3 turns Step 2's
gaps into concrete "emphasize / watch out" notes. A human must verify the
work-authorization wording before any submit (see Failure points).

## The prompts (exactly as used)

**Step 1 — GATHER:**
> Given this job posting text, extract a fact sheet with exactly these fields:
> role title, requisition/req ID, company, location(s), pay or range, employment
> type, requirements (bulleted, max 6), work-authorization wording (quote it
> verbatim), application deadline if any. Output the sheet only, no commentary.

**Step 2 — SYNTHESIZE:**
> You are given a job fact sheet and my resume (WashU senior, CS+Math, May
> 2027, 3.75 GPA; FlyRank Back-End AI Engineering Intern; Kaggle silver 42/1446;
> non-F-1 EAD through 2028-08-03, no sponsorship needed; not a US citizen).
> Produce: (a) fit score 1–10 with one-line justification; (b) top 3 strengths
> mapped to stated requirements; (c) top 2 gaps or risks, quoting the posting
> where relevant; (d) a go / no-go recommendation. Be skeptical — a 7 means
> "strong", not "applied".

**Step 3 — DRAFT:**
> Given the fact sheet and the fit assessment, write application notes: (1)
> the 2–3 resume bullets to emphasize for this posting; (2) any posting
> question that needs a careful truthful answer; (3) one risk to double-check
> before submitting. Keep it under 150 words.

---

## The five runs

### Run 1 — Mastercard · Software Engineer, Launch Program 2027 (R-288578)

**Step 1 (fact sheet):** O'Fallon MO / SLC UT / Arlington VA · $100k base (O'Fallon)
· new-grad program · auth wording: excludes sponsorship only · req R-288578.

**Step 2 (synthesis):** Fit **8/10** — backend + new-grad program is a direct
match; strengths: FastAPI billing service (payments-adjacent), evals/retries
discipline, Kaggle. Gaps: no fintech experience; Java-heavy stack vs my
Python/TS. **Go.**

**Step 3 (notes):** Emphasize: idempotent billing API (directly payments-relevant),
polite scraper (data pipelines). Careful answer: restrictive-covenant question →
No (confirmed). Risk: none on auth wording — verified clean.

**Result:** applied 2026-10-01, confirmation received.

### Run 2 — DoorDash · Software Engineer I, Entry-Level (Greenhouse 8163709)

**Step 1:** $107,400–$158,000 + equity · entry-level · location preference asked
on form (no remote option) · team-preference field required · auth: sponsorship
exclusion only.

**Step 2:** Fit **7/10** — strengths: backend AI + decision-flow project maps to
"Launchpad & AI Research"; internship count truthful = 1. Gaps: entry-level I
(competing with experienced new grads); no LA ties (picked Los Angeles).
**Go** — comp band justifies it.

**Step 3:** Emphasize: React Flow + Inngest decision flow (closest to AI team
work), background-jobs project. Careful answers: internship count = 1 (truthful);
team preference = "Launchpad & AI Research". Risk: location preference is
binding-ish — picked LA deliberately.

**Result:** applied 2026-10-01, confirmation page shown.

### Run 3 — Rockwell Automation · Software Engineer - C++ (R26-7383)

**Step 1:** Milwaukee WI, hybrid · C++ role · auth wording: "Legal authorization
to work in the US is required" (no sponsorship carve-outs listed) · req R26-7383.

**Step 2:** Fit **6/10** — strengths: strong CS fundamentals, shipped systems;
gaps: role is C++-centric, my recent work is Python/TS; hybrid Milwaukee =
relocation. Auth wording is the generic kind my EAD satisfies. **Go** (weak go).

**Step 3:** Emphasize: systems thinking (correctness under retry), Kaggle (C++
adjacent? no — be honest: algorithms). Careful: don't oversell C++ — keep
claims to what's on the resume. Risk: relocation willingness — confirmed yes.

**Result:** applied 2026-10-01, Workday "Application Submitted".

### Run 4 — Emerson · Software Engineer (req 26010937)

**Step 1:** Austin TX, hybrid, entry-level · auth wording excludes sponsorship +
specific temporary statuses incl. F-1 OPT/CPT · req 26010937.

**Step 2:** Fit **7/10** — strengths: backend shipping experience, evals; gaps:
none major on skills. Auth: wording explicitly lists F-1 OPT/CPT as excluded —
my non-F-1 EAD is **not** in the excluded set → eligible, but this is exactly
the wording class a human must verify (see Failure points). **Go.**

**Step 3:** Emphasize: LLM API with timeout/retries/eval (production habits),
scraper (data ingestion). Careful: email-OTP identity check on the form —
completed via fresh code. Risk: none remaining after verification.

**Result:** applied 2026-10-01, status Under Consideration.

### Run 5 — Progressive · IT Software Developer Associate, Entry Level (261811)

**Step 1:** Remote · $77,000/yr + $6,000 starting bonus + Gainshare up to 24% ·
entry-level · auth excludes sponsorship-required statuses only · job 261811.

**Step 2:** Fit **8/10** — strengths: backend + AI + shipped projects; remote =
no relocation friction; comp transparent. Gaps: "IT" title suggests
enterprise-stack work (less greenfield AI). **Go** — strong.

**Step 3:** Emphasize: full project list (breadth fits "associate" scope),
reliability habits. Careful: Cloudflare challenge on the careers site — cleared
via user takeover, then completed. Risk: none on auth.

**Result:** applied 2026-10-01, thank-you page + confirmation email.

---

## Time accounting (honest, setup included)

| | Per run | × 5 runs |
|---|---|---|
| Manual (pre-pipeline): read posting, assess fit, write notes from scratch | ~20 min | ~100 min |
| Pipeline: run 3 fixed prompts, review outputs | ~6 min | ~30 min |
| One-time setup: designing steps, writing + testing prompts | — | ~25 min |
| **Total** | | **~55 min vs ~100 min → ~45 min saved** |

The setup cost is real and counted. The pipeline pays for itself after ~2 runs;
at job-search volume (30+ applications) the saving is on the order of **7 hours**.
The per-run number assumes the human still reads the final notes (~2 min of the
6) — that review is load-bearing, not optional.

## Where it breaks + what a human must still check

1. **Work-authorization wording is not machine-judgeable.** Step 2 flags the
   wording class, but only a human decides "my EAD qualifies here" — posting
   language varies ("no sponsorship" vs "indefinite authorization") and the
   cost of getting it wrong is an application built on a false premise. **Human
   check required before every submit.**
2. **Fit scores inherit posting-text quality.** Vague postings ("rockstar
   developer wanted") produce confident-sounding nonsense scores. If Step 1's
   fact sheet is thin, the run should be discarded, not trusted.
3. **Stale facts.** Pay bands and deadlines drift; the pipeline has no memory
   of when a fact was gathered. Re-run Step 1 if a posting is more than a few
   days old before acting on it.
4. **It can't judge the unjudgeable.** Team culture, hiring-manager taste, and
   whether a role is already effectively filled never appear in the fact sheet.
   The pipeline narrows the field; it doesn't pick winners.
5. **Prompt drift.** If I "improve" a step prompt mid-search, earlier runs stop
   being comparable. Prompts are versioned (v1 above) — change the version, not
   the prompt in place.
