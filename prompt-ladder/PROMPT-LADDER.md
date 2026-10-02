# The Prompt Ladder

**Track:** General AI Fluency · **Week:** 2 · **Estimated:** 2h · **Actual:** ~2h

## The task I chose

Writing a cover letter for a Summer 2027 software engineer internship — a real
task from my own track (I'm in the middle of a high-volume job search).

## Baseline (v0) — the weak prompt

**Prompt:** `Write a cover letter for a software engineer internship.`

**Output (excerpt):**

> Dear Hiring Manager,
>
> I am writing to apply for the software engineer internship position at your
> company. I am passionate about technology and cutting-edge software development.
> As a computer science student, I have experience with programming languages
> such as Python and Java, and I am a quick learner who works well in teams.
>
> I would love the opportunity to contribute to your team and learn from your
> experienced engineers. Thank you for your consideration.
>
> Sincerely,
> [Your Name]

Yes — it's a little embarrassing. Placeholder name, "passionate", "cutting-edge",
zero specifics. This is the "before" picture.

---

## v1 — one layer: a clearer goal

**Prompt:** v0 + `The goal is to get the hiring manager to invite me to an
interview by showing I can contribute from day one, not just listing coursework.`

**Output (excerpt):**

> Dear Hiring Manager,
>
> I am applying for the software engineer internship because I can contribute
> from day one. My coursework in data structures and algorithms has prepared me
> to take on real engineering tasks immediately, and I am eager to prove myself
> in a professional setting...

**Notes**

- **What changed:** added a goal sentence (interview via day-one contribution).
- **What improved:** the letter now argues something instead of just existing —
  paragraph 1 has a thesis.
- **What still failed:** every supporting detail is still generic ("coursework",
  "real engineering tasks"). The goal is clear but there's nothing to believe.
- **Try next:** give it someone to talk to — a defined audience.

## v2 — one layer: a defined audience

**Prompt:** v1 + `The audience is a hiring manager at a mid-size fintech company
hiring a Summer 2027 backend intern. They care about reliability, real project
experience, and clear communication.`

**Output (excerpt):**

> Dear Hiring Manager,
>
> As a backend-leaning engineer who values reliable systems, I am excited to
> apply for your Summer 2027 internship. Fintech systems demand correctness, and
> my academic training has prepared me to contribute to dependable software from
> day one...

**Notes**

- **What changed:** audience defined (fintech hiring manager, backend intern).
- **What improved:** tone shifted — "reliable systems", "correctness" match the
  audience's vocabulary. Slightly less like a form letter.
- **What still failed:** honestly? This barely moved the needle. The letter is
  more *formal* but just as *empty* — "my academic training" still proves
  nothing. **This was my "this didn't help much" moment:** tone without
  substance is decoration. The missing ingredient is clearly specifics, not
  audience.
- **Try next:** feed it real context — actual background it can cite.

## v3 — one layer: real context

**Prompt:** v2 + `About me: WashU senior (CS + Math, May 2027, GPA 3.75).
Back-End AI Engineering Intern at FlyRank (Jul–Aug 2026): built a usage-metering
& billing service with idempotent APIs (FastAPI), a polite web scraper, and an
LLM classification endpoint with evals. Kaggle silver medal (rank 42/1446).
Fluent in English and Chinese.`

**Output (excerpt):**

> Dear Hiring Manager,
>
> I'm a WashU senior in Computer Science + Mathematics (May 2027, 3.75 GPA)
> applying for your Summer 202... As a Back-End AI Engineering Intern at
> FlyRank, I built a usage-metering and billing service in FastAPI where
> retried requests could never double-charge a customer — exactly the kind of
> correctness fintech systems depend on. I also shipped a polite web scraper
> and an LLM classification endpoint backed by a real eval set, and earned a
> Kaggle silver medal (rank 42 of 1,446)...

**Notes**

- **What changed:** real, checkable facts added.
- **What improved:** everything. This is the single biggest jump in the ladder —
  the letter went from decoration to evidence. The fintech audience layer from
  v2 finally *pays off* because there's something to aim at the audience.
- **What still failed:** it's now too long and rambly (380+ words); the billing
  detail, while true, reads like a resume paragraph pasted in. No shape.
- **Try next:** constrain the shape — format.

## v4 — one layer: a specified output format

**Prompt:** v3 + `Format: under 250 words, 3 short paragraphs (hook + evidence +
close), no bullet points, professional tone, end with a specific call to action.`

**Output (excerpt):**

> Dear Hiring Manager,
>
> I'm a WashU senior (CS + Math, May 2027) who has already done the unglamorous
> work your backend team cares about: at FlyRank I built a metering and billing
> API where idempotency wasn't a buzzword but a correctness guarantee — a
> retried request could never double-charge a customer.
>
> That same instinct shows up across my work: a polite, schema-validated web
> scraper, an LLM endpoint guarded by timeouts, retries and a real eval set,
> and a Kaggle silver medal (42nd of 1,446). I communicate clearly in English
> and Chinese, and I hold a 3.75 GPA while shipping.
>
> I'd welcome the chance to discuss how I can contribute to your Summer 2027
> backend team. I'm available for an interview at your convenience — and happy
> to walk through any of these systems live.

**Notes**

- **What changed:** hard format constraints (250 words, 3 paragraphs, CTA).
- **What improved:** the ramble collapsed into a readable letter. The 3-beat
  shape forced prioritization — weakest details got cut automatically.
- **What still failed:** one cliché slipped back in ("un glamorous work your
  team cares about" is trying too hard); "happy to walk through any of these
  systems live" is fine but the close is still slightly stiff.
- **Try next:** ban the clichés explicitly and add a truthfulness guardrail.

## v5 — one layer: constraints + quality criteria

**Prompt:** v4 + `Constraints: do not invent any fact not given above; do not
use clichés like "passionate" or "cutting-edge"; every claim must tie to the
background above. Quality check before output: would a skeptical hiring manager
believe every sentence?`

**Output:**

> Dear Hiring Manager,
>
> I'm a WashU senior in Computer Science + Mathematics (May 2027, 3.75 GPA)
> applying for your Summer 2027 backend internship. At FlyRank, I built a
> usage-metering and billing API in FastAPI where a retried request could never
> double-charge a customer — idempotency as a correctness guarantee, not a
> feature bullet. I also shipped a polite web scraper with schema-validated
> output and an LLM classification endpoint guarded by timeouts, retries, and
> a measured eval set.
>
> Outside of work, I earned a Kaggle silver medal (rank 42 of 1,446) and I work
> fluently in English and Chinese. What I bring on day one is backend code that
> has already survived retries, bad input, and real review.
>
> I'd welcome an interview to discuss the fit — I'm glad to walk through any of
> these systems and answer hard questions about the trade-offs.

**Notes**

- **What changed:** anti-cliché + anti-invention constraints, skeptic test.
- **What improved:** the trying-too-hard line is gone; every sentence is
  checkable against the background. This is the first version I'd actually send.
- **What still failed:** minor — "Outside of work, I earned a Kaggle silver
  medal" slightly undersells it, and the letter is company-agnostic (no company
  name). Both are one-line fixes per application, not prompt flaws.
- **Try next:** per-company customization layer (company name + one specific
  reason) — but that's application work, not prompt work. The ladder is done.

---

## The final reusable prompt (cleaned up for a stranger)

```
Write a cover letter for a [ROLE] at [COMPANY TYPE].

Goal: earn an interview by showing I can contribute from day one.
Audience: a hiring manager at [COMPANY TYPE] hiring for [TERM]; they care about
[2-3 things they care about].

About me: [2-4 checkable facts: school, degree, dates, GPA] [1-2 work/project
bullets with concrete outcomes, not adjectives] [1 distinguishing fact].

Format: under 250 words, 3 short paragraphs (hook + evidence + close), no
bullet points, professional tone, end with a specific call to action.

Constraints: invent nothing not given above; no clichés ("passionate",
"cutting-edge", "dream"); every claim must tie to the background. Before
outputting, check: would a skeptical hiring manager believe every sentence?
```

## What the ladder taught me

Six runs, one layer at a time. The honest ranking of what mattered: **real
context (v3) > format (v4) > goal (v1) > constraints (v5) > audience (v2)**.
The surprise was v2: defining the audience *felt* like progress but changed
almost nothing until there were facts to aim. Specifics first, polish second —
that's the whole lesson in one line.
