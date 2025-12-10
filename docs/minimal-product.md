# Minimal Product (v0) — Job‑Seeker CV Tailoring

Brand: **Zyvi** — “Tailor your CV to pass the filter.”

A minimal, compelling product for tech job seekers to generate an ATS‑friendly tailored CV PDF (up to 2 pages) and a concise cover letter from pasted text, with a lightweight paywall.

---

## Audience

- Tech job seekers who want a quick, high‑quality, ATS‑friendly CV tailored to a single job ad.

## Core Outcome

- Tailored CV PDF (ATS‑friendly, single column, up to 2 pages) + concise 3‑paragraph cover letter aligned to one pasted job ad.
- Pricing: First tailored CV free (covers 1 job ad). Additional exports require a subscription (tiers: Free, Plus, Pro; details TBD, USD). English only.

## Inputs (v0)

- Base CV: user pastes plain text of their existing CV (assumed to include contact details and name).
- Job ad: user pastes plain text of one job offer.
- No file uploads, no freeform fields, no login/registration flow.

## Outputs (v0)

- CV PDF: professionally styled, ATS‑friendly, single column; allows hiding/trim of older roles to fit two pages; consistent headings/dates/bullets.
- Cover letter: plain text on screen; copyable for free; ~150–200 words; names company/role if parsed, otherwise neutral (e.g., “your company”, “the role”).
- Preview: pre‑payment shows Summary + first (most relevant) Experience entry snippet; full PDF requires payment.

## Review Model

- Approve/reject per section (no inline edits, single pass):
  - Summary
  - Skills (keywords only from pasted CV; prioritized by job ad)
  - Experience per job entry (can reorder by relevance; may trim/hide)
- Reject = keep original section/entry from the pasted CV.

## Strict Rules

- No new facts: only rephrase/reorder/trim content present in the pasted CV.
- ATS first: single‑column, no icons/graphics, clean headings, clear bullets, consistent dates, parsable text.
- Page limit: up to 2 pages; hide/trim to fit.

## User Flow

1. Paste: user pastes CV + job ad → Continue.
2. Review: section‑by‑section approve/reject (Summary, Skills, each Experience entry).
3. Preview: show cover letter (full) + CV snippet → “Get PDF” (first PDF free).
4. Pay/Upgrade: For subsequent PDFs, require subscription checkout → Success page with immediate download and 24‑hour re‑download link via email.

## Access & Paywall

- Anonymous until paywall: users can preview and copy the cover letter without login.
- Free export: first tailored CV + cover letter for one job ad is free to download.
- Paywall: additional CV exports require subscription checkout; Stripe collects email; success page allows download; email includes 24‑hour link.

## Abuse Controls (Lightweight)

- Anonymous session/IP caps on LLM usage; optional hCaptcha/Turnstile on suspicious volume.
- Optional watermark on preview only (not on paid PDF).

## MVP UI

- Pages: Paste → Review → Preview → Pay → Success/Download.
- Design: modern, minimal, mobile‑friendly; emphasize ATS‑friendly credibility.

## Success Criteria

- Conversion: % of preview sessions that purchase.
- Quality: user rating on fit/clarity (post‑download 1‑click survey).
- Cost: average token spend per session under cap.

## Out of Scope (v0)

- Non‑English, DOCX exports, file uploads (PDF/DOCX), regeneration loops, freeform text fields, multi‑job workflows, dashboards, subscriptions, job‑URL harvesting, contact parsing.

---

## Assumptions & Constraints

- Contact details are present in pasted CV; no extra forms in v0.
- Company/role names are extracted from job ad when confident; otherwise neutral phrasing is used.
- One job ad per run; single pass (no regenerate).
