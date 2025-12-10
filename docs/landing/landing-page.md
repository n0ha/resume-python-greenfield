# Landing Page — Minimal Product (v0)

A focused landing page for **Zyvi** that clearly communicates value, builds trust, and drives users into the paste → review → preview flow with minimal friction. Brand: Zyvi. Tagline: “Tailor your CV to pass the filter.”

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
   - Sub‑headline: “Zyvi parses your CV + job ad to pass ATS filters. First tailored CV free; subscribe for more.”
   - CTA buttons: “Try free preview” (primary), “See sample CV + letter” (secondary)
   - Trust badges (lightweight): “ATS‑friendly”, “First CV free”, “No signup to preview”, “Privacy‑first”
2. How It Works (3 steps)
   - Paste your CV + job ad → Review suggested edits by section → Download your first tailored CV free; subscribe for ongoing exports
3. Why It Works (value props)
   - ATS‑friendly: Clean, single column, consistent headings/dates, keyword‑aware
   - Aligned: Zyvi parses and aligns keywords to the job ad; keeps ATS parsable
   - Control: Approve/reject section edits; no new facts added
   - Quality: PDF formatted for clarity; includes a concise 3‑paragraph cover letter
4. Sample Snippet
   - Show a mock Summary + first Experience entry (watermarked) to set expectations
5. Pricing
   - Simple: “First tailored CV free (one job). Subscription required for additional tailored CVs. Tiers: Free, Plus, Pro (details TBD).”
   - Draft inclusions (TBD numbers): Free = 1 tailored CV + cover letter; Plus = multiple tailored CVs/month + saved profile; Pro = higher limits/unlimited (fair use) + priority rendering.
6. FAQ
   - Why subscribe after the free CV? “Subscriptions cover ongoing tailored CVs and cover letters across multiple job ads.”
   - Will you invent skills? “No. We only rephrase/reorder what you provide.”
   - Is my data safe? “We don’t require accounts for preview; we store minimal data.”
   - Languages? “English only in v0.”
   - CV length? “Up to 2 pages; older roles may be hidden to fit.”
   - Refunds? “If the PDF fails to render or is unusable, contact support.”
7. Footer
   - Links: Privacy, Terms, Contact, Imprint

## Draft Copy

- Hero Headline: “Tailor your CV to any job ad — in minutes”
- Sub‑headline: “Zyvi parses your CV + job ad to pass ATS filters. First tailored CV free; subscribe for more.”
- Primary CTA: “Try free preview”
- Value bullets:
  - “ATS‑friendly formatting — single column, clear headings, consistent dates”
  - “Keyword-aligned — Zyvi parses job ads to highlight the right terms”
  - “Keep control — approve/reject edits per section”
  - “No made‑up claims — we only use what’s in your CV”
  - “Includes a concise cover letter tailored to the same job ad”

## Visuals

- Minimal: clean hero, small trust badges, simple icons (text‑based OK for v0).
- Sample snippet: Summary + most relevant Experience entry with watermark.

## SEO & Sharing

- Title: “Zyvi — Tailor your CV to pass the filter | First CV Free”
- Meta description: “Zyvi parses your CV and a job ad, aligns sections to pass ATS filters, and delivers your first tailored CV + cover letter free. Subscribe for ongoing exports.”
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
