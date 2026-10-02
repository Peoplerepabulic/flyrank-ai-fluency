# Three Roads: Choose Your Stack with AI

**Track:** General AI Fluency · **Week:** 4 · **Estimated:** 2h

Constraints given to the AI: free only · honest about my skill level (strong
Python/TS, no design background) · must serve the sitemap + content map
(4 pages, case studies, images, repo/demo links, long-form text) · must be
something I can maintain alone.

## Road 1 — Hand-written HTML/CSS on GitHub Pages

- **Build:** plain HTML + CSS, no framework, no build step.
- **Free hosting:** GitHub Pages (already have the account).
- **Backend needed?** No.
- **Trade-offs:** Maximum control, zero dependencies, loads instantly. But
  every new case study is hand-edited HTML; no components, so repeated
  markup (nav, footer) drifts unless I'm disciplined. Honestly fine at
  4 pages.

## Road 2 — Next.js on Vercel (free tier)

- **Build:** React components, App Router, markdown-driven case studies.
- **Free hosting:** Vercel hobby tier.
- **Backend needed?** No (static export is enough).
- **Trade-offs:** Components kill the duplication problem; image optimization
  and routing come free. But it's a build pipeline for a 4-page site —
  node_modules, version churn, and cold comfort if something breaks the
  night before an interview. More power than this site needs.

## Road 3 — Astro on Netlify (free tier)

- **Build:** Astro with content collections (one markdown file per case
  study), components only where interactive.
- **Free hosting:** Netlify free tier.
- **Backend needed?** No.
- **Trade-offs:** The sweet spot on paper: markdown-in, site-out, almost no
  client JS, fast by default. But it's the stack I know least — learning
  curve lands exactly when I should be writing case studies, not configs.

## My rationale (my own words)

I'm choosing **Road 1: hand-written HTML/CSS on GitHub Pages**.

The two I ruled out: Next.js (Road 2) is the strongest technically and the
one I'd pick for a client — but for four pages it's machinery I don't need,
and machinery breaks. Astro (Road 3) is the elegant answer, but I'd be
learning it under deadline, which is how portfolios end up half-migrated.

Road 1 wins on the question that actually matters: **can I maintain this
alone, at 11pm, before an interview?** Yes — there's nothing to break. No
build step, no dependencies, no dashboard. A new case study is one HTML file
following the existing pattern, and the identity kit (two fonts, four
colors) keeps it coherent without a design system. The honest cost is
duplicated nav/footer markup, which I'll manage with a tiny build script if
it ever hurts — not before.

**Backend, answered honestly:** the portfolio needs no backend. The one
dynamic idea (a contact form) is worse than a `mailto:` link — forms need a
service, spam filtering, and monitoring; an email link needs nothing and
never breaks.
