# Landing Page TODO (v0)

- Product framing
  - Confirm audience and promise from docs/landing/landing-page.md and docs/minimal-product.md.
  - Decide brand tone (calm, efficient, privacy-first) and key proof points to emphasize in hero and trust badges.
  - **AI prompt — name**: “You are a brand strategist. Generate 12 short, globally pronounceable product names for an ATS-friendly CV tailoring tool priced at €/$0.99. Requirements: 2 syllables preferred, avoid clichés like ‘resume AI’, convey speed + precision, .com availability likely. Return table: Name, Rationale (<=12 words), Domain likelihood note.”

- Visual identity and assets
  - Lock primary/secondary colors, neutrals, and background treatment; pick typography stack.
  - Define icon style (simple line/duotone) and badge style for ATS-friendly/privacy/no signup.
  - **AI prompt — logo**: “Create a minimalist logo for an ATS-friendly CV tailoring product. Style: clean wordmark + small glyph suggesting document flow/checkmark. Colors: deep charcoal base, accent in vivid teal or electric blue. Background: transparent. Formats: SVG + 1024 px PNG. Provide light/dark variants and a square-only mark for favicon.”
  - **AI prompt — hero visual**: “Generate a hero illustration showing a CV being refined for a job ad: single-column CV sheet with subtle highlights on keywords and a small checkmark badge. Style: modern, flat with soft gradients, no realistic faces, light background with teal/blue accents. 1600x900, transparent or clean white background.”
  - **AI prompt — sample snippet image (also OG/Twitter)**: “Render a watermarked preview of Summary + first Experience entry for a tech CV. Single-column, clear headings, consistent dates, bullet list. Add faint ‘Preview’ watermark. Size 1200x630. Keep text generic (e.g., Jane Smith, Software Engineer) and ATS-friendly (no icons).”
  - **AI prompt — favicon**: “From the logo glyph, output a crisp 64x64 PNG on transparent background; ensure it reads at 16x16.”

- Copy and structure
  - Finalize hero headline/subheadline, CTA labels, trust badge text (ATS-friendly, No signup to preview, Privacy-first).
  - Draft “How it works” 3-step copy, “Why it works” value props, Sample snippet text, Pricing line, and FAQ entries from landing-page.md.
  - Prepare sample CV snippet text that matches ATS tone and shows one experience entry.
  - Set SEO text: title, meta description, keywords; match Open Graph title/description.

- Layout and UX
  - Wireframe sections in order: Hero → How it works → Why it works → Sample snippet → Pricing → FAQ → Footer.
  - Plan CTA behaviors: primary routes to Paste page; secondary opens sample (modal or static page).
  - Define trust badge layout, spacing, and responsive stacking.
  - Specify animations (staggered reveal on scroll; CTA hover states) that remain performant on mobile.

- Implementation tasks
  - Build responsive layout (desktop two-column hero → single-column mobile) with selected typography and color system.
  - Integrate logo, hero visual, sample snippet image, badges, and favicon; ensure OG/Twitter image is linked.
  - Add SEO/meta tags, canonical, and preloads as needed; include Open Graph + Twitter cards.
  - Implement FAQ accordion, pricing block, and sample snippet display with watermark overlay.
  - Wire CTA links to Paste page and sample route/modal; include “See sample CV + letter”.
  - Add lightweight analytics events: cta_click_preview, view_how_it_works, view_pricing, view_sample, start_session.

- QA and polish
  - Accessibility: semantic headings, alt text for visuals, focus states, contrast checks.
  - Performance: optimize asset sizes (compress hero/snippet), lazy-load non-critical images, ensure <2s 4G target.
  - Cross-device checks: mobile single-column behavior, button tap sizes, smooth scroll.
  - Copy/links verification: Privacy/Terms/Contact/Imprint placeholders working.

- Release
  - Package assets into source control (SVG/PNG, favicon, OG image) and document their paths.
  - Add changelog entry for landing page completion and note open experiments/variants to try next.
