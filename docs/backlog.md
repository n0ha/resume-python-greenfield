# Backlog — Minimal Product (v0)

This backlog captures the MVP tickets for the job‑seeker CV tailoring product. Use alongside `scripts/create_github_issues.sh` to seed GitHub issues.

---

## Epics
- Landing Page (LP)
- Paste & Review Flow (UI)
- LLM Orchestration (Core)
- PDF Rendering (CV)
- Payments (Stripe)
- Delivery (Email/Links)
- Abuse Controls (Caps/CAPTCHA)
- Infra & Analytics

---

## Issues

1) LP: Hero + CTA
- Labels: LP, frontend
- Acceptance: Hero with headline/subheadline; primary CTA routes to Paste page; loads fast on mobile.

2) LP: How It Works + Value Props
- Labels: LP, frontend
- Acceptance: 3-step explainer; value bullets for ATS, control, quality.

3) LP: Pricing + FAQ + Footer
- Labels: LP, frontend
- Acceptance: Pricing text (€/$0.99), FAQs, footer links (Privacy/Terms/Contact).

4) LP: SEO + OG
- Labels: LP, seo
- Acceptance: Title, meta description, OG tags, favicon.

5) UI: Paste Page
- Labels: UI, frontend
- Acceptance: Two plain-text fields (CV, Job Ad); submit starts session; basic validation.

6) UI: Review — Summary Section
- Labels: UI, frontend, review
- Acceptance: Show suggested Summary; approve/reject; rejection keeps original.

7) UI: Review — Skills Section
- Labels: UI, frontend, review
- Acceptance: Suggested skills (only from pasted CV) prioritized by job ad; approve/reject.

8) UI: Review — Experience per Entry
- Labels: UI, frontend, review
- Acceptance: For each job: suggested edits and possible reorder; approve/reject; allow hide/trim to fit 2 pages.

9) UI: Preview Page
- Labels: UI, frontend
- Acceptance: Show full cover letter text; show CV snippet (Summary + top Experience); CTA to pay.

10) Core: LLM — Job Ad Parsing
- Labels: core, llm
- Acceptance: Extract company/role when confident; otherwise null; no inference beyond text.

11) Core: LLM — Section Suggestions
- Labels: core, llm
- Acceptance: Generate Summary, Skills, Experience edits with “no new facts” constraint; output structured JSON.

12) Core: Apply Review Decisions
- Labels: core
- Acceptance: Merge approved suggestions; rejected sections keep original; produce final CV YAML.

13) CV: ATS Template + PDF Render
- Labels: cv, pdf
- Acceptance: Single-column, consistent headings/dates/bullets; up to 2 pages; render to PDF from YAML.

14) Payments: Stripe Checkout
- Labels: payments
- Acceptance: Create Checkout session for 0.99; success/cancel handling; webhook secret configured.

15) Payments: Webhook Fulfillment
- Labels: payments, backend
- Acceptance: Verify signature; mark session paid; authorize download.

16) Delivery: Download + 24h Link
- Labels: delivery, backend
- Acceptance: Success page immediate download; email a link valid 24h.

17) Abuse: Token Caps + CAPTCHA
- Labels: abuse, security
- Acceptance: Per-session/IP cap; challenge on suspicious volume.

18) Infra: Health + Observability
- Labels: infra
- Acceptance: /health endpoint; basic logs; error tracking hooks (stub ok).

19) Analytics: Events
- Labels: analytics
- Acceptance: Track `cta_click_preview`, `start_session`, `review_complete`, `checkout_start`, `checkout_success`.

20) Docs: Minimal Product + Landing
- Labels: docs
- Acceptance: Keep `minimal-product.md` and `landing-page.md` updated as implemented.

---

## Notes
- Language: English-only outputs in v0.
- No inline editing; no regeneration loop.
- Cover letter is free to copy; PDF behind paywall.

