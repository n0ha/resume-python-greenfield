# Landing Page — Minimal Product (v0)

A focused landing page that clearly communicates value, builds trust, and drives users into the paste → review → preview flow with minimal friction.

---

## Goals

- Communicate the core value: ATS‑friendly, job‑specific CV in minutes.
- Drive a single action: start a free preview (paste CV + job ad).
- Establish trust: ATS‑proof, privacy‑first, no signup required to preview.

## Primary Actions

- Primary CTA: “Try free preview” → routes to Paste page.
- Secondary CTA: “See sample CV + letter” → opens a sample modal or page.

## Page Structure

1. Hero
   - Headline: “Tailor your CV to any job ad — in minutes”
   - Sub‑headline: “ATS‑friendly and recruiter‑ready. Free preview, €/$0.99 to download.”
   - CTA buttons: “Try free preview” (primary), “See sample CV + letter” (secondary)
   - Trust badges (lightweight): “ATS‑friendly”, “No signup to preview”, “Privacy‑first”
2. How It Works (3 steps)
   - Paste your CV + job ad → Review suggested edits by section → Preview cover letter + pay €/$0.99 for a pro PDF
3. Why It Works (value props)
   - ATS‑friendly: Clean, single column, consistent headings/dates, keyword‑aware
   - Control: Approve/reject section edits; no new facts added
   - Quality: PDF formatted for clarity; includes a concise 3‑paragraph cover letter
4. Sample Snippet
   - Show a mock Summary + first Experience entry (watermarked) to set expectations
5. Pricing
   - Simple: “€/$0.99 per export — includes CV PDF and cover letter. No subscriptions.”
6. FAQ
   - Why pay? “We cover AI costs and deliver a polished PDF.”
   - Will you invent skills? “No. We only rephrase/reorder what you provide.”
   - Is my data safe? “We don’t require accounts for preview; we store minimal data.”
   - Languages? “English only in v0.”
   - CV length? “Up to 2 pages; older roles may be hidden to fit.”
   - Refunds? “If the PDF fails to render or is unusable, contact support.”
7. Footer
   - Links: Privacy, Terms, Contact, Imprint

## Draft Copy

- Hero Headline: “Tailor your CV to any job ad — in minutes”
- Sub‑headline: “ATS‑friendly and recruiter‑ready. Free preview, €/$0.99 to download.”
- Primary CTA: “Try free preview”
- Value bullets:
  - “ATS‑friendly formatting — single column, clear headings, consistent dates”
  - “Keep control — approve/reject edits per section”
  - “No made‑up claims — we only use what’s in your CV”
  - “Includes a concise cover letter tailored to the same job ad”

## Visuals

- Minimal: clean hero, small trust badges, simple icons (text‑based OK for v0).
- Sample snippet: Summary + most relevant Experience entry with watermark.

## SEO & Sharing

- Title: “ATS‑Friendly CV Tailor — Free Preview, €/$0.99 PDF”
- Meta description: “Paste your CV and a job ad. Review suggested edits by section. Get an ATS‑friendly PDF CV and a cover letter for just €/$0.99.”
- Keywords: ats resume, tailored cv, job ad resume, resume keywords, resume pdf
- Open Graph: image preview of sample snippet (static asset), og:title/description per above.

## Analytics & Experimentation

- Track events: `cta_click_preview`, `view_how_it_works`, `view_pricing`, `view_sample`, `start_session`.
- Measure: CTR to preview, completion to review, payment conversion, cost per session (via backend).
- Variant ideas: alternate headline emphasizing “ATS‑friendly” vs “Job‑specific”.

## Acceptance Criteria

- Renders in <2s on 4G; Core Web Vitals acceptable for a simple page.
- Two CTAs present; primary routes to Paste page; sample opens or routes to a static sample.
- SEO tags present; Open Graph meta present; favicon present.
- Privacy/Terms links available (can be placeholders in v0).
- Mobile responsive layout (single column on small screens).

## User Stories

- As a job seeker, I immediately understand the benefit and cost.
- As a job seeker, I can start a free preview without creating an account.
- As a job seeker, I can confirm this is ATS‑friendly and privacy‑respecting.
- As a job seeker, I can view a sample before committing.

## Out of Scope (Landing v0)

- Blog, longform SEO content, testimonials, complex pricing tables, localization.

