# Landing Page TODO (v0)

- Product framing
  - [X] Confirm audience and promise from docs/landing/landing-page.md and docs/minimal-product.md. Audience = tech job seekers needing ATS-safe, job-specific CVs quickly. Promise = Zyvi parses CV + job ad, aligns to pass ATS filters, delivers first tailored CV free; subscribe for ongoing tailored exports. Privacy-first, no new facts.
  - [X] Brand locked: **Zyvi** (“Tailor your CV to pass the filter”). Tone: precise, algorithmic, fast, trustworthy; secondary: empowering. Emphasize ATS-friendly, no new facts, privacy-first.

- Visual identity and assets
  - [X] Lock primary/secondary colors, neutrals, and background treatment; pick typography stack. Decision: Align to logo palette. Primary accent #10e7b3 (logo teal). Secondary accent #3aa0ff (cool blue). Background #131e26; surface/card #111827; border #1f2937; text #e9e9e8; muted #969695; warn #f97316. Background: subtle radial gradients; cards on solid #111827 with 1px border. Typography: “Space Grotesk” with fallback “Inter, Segoe UI, system-ui, sans-serif”; monospace: “IBM Plex Mono, SFMono-Regular, Menlo, monospace”.
  - [X] Assets provided: logo `docs/brand/logo.png`, hero `docs/brand/hero.png`, preview/OG `docs/brand/preview.png`, favicon `docs/brand/favicon.png`.
  - [X] Define icon style (simple line/duotone) and badge style for ATS-friendly/privacy/no signup. Icons: 2px stroke line icons, 20–24px, rounded caps/joins, primary color var(--brand) on dark cards, muted var(--muted) for inactive. Avoid filled/glyph icons to keep ATS-friendly vibe. Badges: pill shape, 1px border var(--border), background var(--card) with subtle top highlight (1–2% lighter), text in var(--muted); trust badges can use small leading dot in var(--brand).
  - [ ] Fix assets: current PNGs are flattened on white backgrounds. Need transparent versions for logo, hero, preview/OG, and favicon. Provide a single clean logo (wordmark + glyph) without multiple background variants; ensure dark/light background compatibility.

- Copy and structure
  - [X] Finalize hero headline/subheadline, CTA labels, trust badge text (ATS-friendly, First CV free, No signup to preview, Privacy-first). See landing-page.md and v1 index.html.
  - [X] Draft “How it works” 3-step copy, “Why it works” value props, Sample snippet text, Pricing line, and FAQ entries from landing-page.md. Updated in landing-page.md and v1 index.html with aligned copy.
  - [X] Prepare sample CV snippet text that matches ATS tone and shows one experience entry. Reflected in v1 index.html sample block.
  - [X] Set SEO text: title, meta description, keywords; match Open Graph title/description; reflect free-first-CV model (USD only, no dual currency) and Zyvi tagline. v1 index.html meta/OG updated.
  - [X] Pricing/tier framing: first tailored CV free (1 job). Tiers: Free, Plus, Pro. Placeholder inclusions added in landing-page.md and v1 index.html.

- Layout and UX
  - [X] Wireframe sections in order: Hero → How it works → Why it works → Sample snippet → Pricing → FAQ → Footer. Implemented in v1/index.html structure.
  - [X] Plan CTA behaviors: primary routes to Paste page; secondary opens sample (static page). Implemented links in v1 pages.
  - [X] Define trust badge layout, spacing, and responsive stacking. Badges set as flex wrap pills; mobile stacks via grid collapse.
  - [X] Specify animations (staggered reveal on scroll; CTA hover states) that remain performant on mobile. CTA hover states added; note to add lightweight reveal if desired.

- Implementation tasks
  - [X] Build responsive layout (desktop two-column hero → single-column mobile) with selected typography and color system. Hero now two-column with brand image; grids collapse on mobile.
  - [X] Integrate logo, hero visual, sample snippet image, badges, and favicon; ensure OG/Twitter image is linked. Logo, hero, preview image, favicon in place; OG uses preview.png.
  - [X] Add SEO/meta tags, canonical, and preloads as needed; include Open Graph + Twitter cards. Meta updated; canonical + font preconnect/preload added.
  - [X] Implement FAQ accordion, pricing block, and sample snippet display with watermark overlay. FAQ uses details/summary; sample shows text + preview image with watermark overlay.
  - [X] Wire CTA links to Paste page and sample route/modal; include “See sample CV + letter”.
  - [X] Reflect free-first-CV model in hero/subheadline and pricing section; note subscription upsell placeholders.
  - [ ] Add lightweight analytics events: cta_click_preview, view_how_it_works, view_pricing, view_sample, start_session. (CTA + section view events wired; start_session still pending in app flow.)

- QA and polish
  - [ ] Accessibility: semantic headings, alt text for visuals, focus states, contrast checks.
  - [ ] Performance: optimize asset sizes (compress hero/snippet), lazy-load non-critical images, ensure <2s 4G target.
  - [ ] Cross-device checks: mobile single-column behavior, button tap sizes, smooth scroll.
  - [ ] Copy/links verification: Privacy/Terms/Contact/Imprint placeholders working.

- Release
  - [ ] Package assets into source control (SVG/PNG, favicon, OG image) and document their paths.
  - [ ] Add changelog entry for landing page completion and note open experiments/variants to try next.
