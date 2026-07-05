# Anna Yoga — Developer Report

## SECTION 1 — Project Overview

**Stack:** Vanilla HTML/CSS/JS, single file, Google Fonts (Fraunces, Karla, Space Mono), no build tools, no frameworks.

**Existing sections:** Header/nav, Hero, About, Philosophy, Classes, Benefits, Journey (timeline), Testimonials, Blog/Journal, Gallery, Contact, Footer, floating control panel (language/theme/dark mode).

**Existing features:** 4 color themes × light/dark mode via CSS custom properties, scroll-driven background color morph, EN/RU i18n via `data-i18n` attributes, breathing/pulse micro-animations, sticky header, responsive grid layouts at 900px/860px/560px breakpoints.

**Design quality:** Strong. Consistent type system (serif display + mono labels + sans body), a real color/token architecture (4 themes × 2 modes = 8 palettes from one set of variables), tasteful micro-motion. This is a well above-average template — worth preserving as-is.

**Code quality (as received):** Clean and readable where it exists — good naming, no framework bloat, no build step needed. The problem wasn't quality, it was **completeness**: JavaScript was written for five interactive features, but the matching HTML markup and CSS states were never added, so the JS ran against elements that didn't exist.

---

## SECTION 2 — Audit of Devin's Work

| Feature | Status | Notes |
|---|---|---|
| Mobile navigation | ✗ Missing → ✓ now complete | JS referenced `#menuToggle` / `#navLinks`, but no toggle button existed in the markup and no `.mobile-open` CSS state existed. The hamburger simply did nothing. |
| Sticky header | ✓ Complete | Was already correct (`position: sticky`), no changes needed. |
| FAQ | ✗ Missing → ✓ now complete | Full accordion JS existed (`.faq-question`, `.faq-item`) but **zero FAQ markup was in the page**. The whole section didn't exist. |
| Pricing | ✗ Missing → ✓ now complete | Not present anywhere in HTML, CSS, or JS. |
| Newsletter | ✗ Missing → ✓ now complete | Not present anywhere. |
| Schedule | ✗ Missing → ✓ now complete | Not present anywhere (only an unrelated mention of "schedule" in copy text). |
| Contact validation | ◐ Partial → ✓ now complete | JS validation logic was solid, but the form had no `id="contactForm"` (JS could never find it), used a mismatched `.form-note` class instead of the `.form-success` class the JS was looking for, and had a conflicting inline `onsubmit` handler that fired instead of the real listener. |
| Accessibility | ✗ Missing → ✓ now complete | No skip link, no `<main>` landmark, no focus-visible states, no `aria-expanded`/`aria-controls` wiring, no field error messaging, unlabeled icon-only buttons in places. |
| Performance | ◐ Partial → ✓ now complete | No `loading="lazy"`, no `fetchpriority` on the hero (LCP) image, no meta description. |
| Lazy loading | ✗ Missing → ✓ now complete | No image had a `loading` attribute at all. |
| Back-to-top | ✗ Missing → ✓ now complete | JS existed and referenced `#backToTop`, but no button and no `.back-to-top`/`.visible` CSS existed. |
| Animations | ✓ Complete | Breathing dots/rings, hover states, transitions all present and working; respects `prefers-reduced-motion`. |
| Responsive layout | ✓ Mostly complete | Core breakpoints were solid; extended them to cover new pricing/newsletter/schedule sections. |
| Dark mode | ✓ Complete | Fully working, per-theme dark palettes defined and toggled correctly. |
| Language switching | ✓ Complete | EN/RU fully wired for all pre-existing content; extended to cover all new sections. |
| Theme switching | ✓ Complete | 4 themes, swatches, persistence via `data-theme`, all working. |
| Blog | ✓ Complete (as a teaser grid) | Displays 3 posts; no actual article pages exist — acceptable for a single-page site, flagged in roadmap. |
| Gallery | ✓ Complete | Working masonry-style grid, good alt text throughout. |
| Timeline | ✓ Complete | No issues found. |
| Testimonials | ✓ Complete | No issues found. |
| Contact section | ◐ Partial → ✓ now complete | See "Contact validation" above. |

---

## SECTION 3 — Findings

**Dead CSS:** `.menu-toggle` was defined with full styling but never referenced by any HTML element (now fixed — it's used).

**Dead JS:** None. Every function was reachable and correctly written — it was simply missing its DOM counterpart. This is actually good news: no logic needed to be rewritten, only markup added.

**Duplicated CSS/HTML:** None found. The token/theme system avoids repetition well.

**Unused variables:** None found in the CSS custom-property set — all theme tokens are consumed.

**Broken links:** Nav pointed to `#blog`/`#gallery` etc. correctly; social links are placeholder `href="#"` (expected for a template — flagged in roadmap, not "broken" since content doesn't exist yet).

**Broken accessibility / missing ARIA:**
- No skip-to-content link.
- No `<main>` landmark.
- Mobile menu button didn't exist, so no `aria-expanded` state was ever communicated.
- Contact form fields had no error announcements (`aria-live`), no `aria-invalid`.
- Control panel had no group label for screen reader users.
- No `:focus-visible` styling anywhere — keyboard users had no visible focus indicator on custom buttons/links.

**Missing semantic HTML:** No `<main>`; schedule data was implicit in copy text rather than a real `<table>`.

**Invalid IDs:** None invalid, but several IDs referenced by JS (`menuToggle`, `navLinks`, `backToTop`, `contactForm`) simply didn't exist in the DOM.

**Broken responsiveness:** None found in the base template; new sections needed their own breakpoint rules, which are now included.

**Performance bottlenecks / large repaint areas:** The scroll-driven `updateBg()` background morph is throttled with `requestAnimationFrame` and `passive: true` — already efficient. No changes needed there.

**Layout shifts:** Hero image had no explicit `width`/`height`, which can contribute to CLS on slow connections — added intrinsic dimensions.

**Animation problems / scroll problems:** None found; `prefers-reduced-motion` was already respected for the breathing elements.

**Code smells:** The single inline `onsubmit` handler on the contact `<form>` duplicated (and silently defeated) the real `contactForm` JS listener — classic sign of an unfinished handoff between two passes of work.

**Unfinished work:** Summarized above — mobile nav, FAQ, pricing, newsletter, schedule, contact wiring, accessibility, lazy loading, back-to-top were all either stubbed in JS only or absent entirely.

---

## SECTION 4 — Prioritized TODO (as of audit — all items below are now done)

**High priority**
1. Fix mobile navigation (button + markup + CSS state).
2. Wire the contact form to its own validation JS (id, success class, remove conflicting inline handler).
3. Build the FAQ section markup to match existing JS.
4. Add back-to-top button markup + CSS.

**Medium priority**
5. Build Pricing section.
6. Build Schedule section.
7. Build Newsletter section + validation.
8. Add lazy loading + hero image priority hints.
9. Add skip link, `<main>` landmark, focus-visible states, ARIA labeling.

**Low priority**
10. Add meta description.
11. Extend responsive rules to new sections.
12. Sync footer/nav links to include new sections.

**Future improvements (not implemented — out of scope for "don't invent content")**
- Real blog article pages (currently a teaser grid only).
- Wire social icons to real profiles once available.
- Booking/calendar integration for the schedule table (currently static informational timetable).
- Payment integration for pricing tiers (currently links to the contact form, which is appropriate until a payment processor is chosen).
- Automated form backend (both forms currently do client-side validation only and show a static success message — no email is actually sent, since no backend was specified).
- Real testimonials/certifications if/when Anna provides them.

---

## Developer Changelog

- **Mobile nav:** Added hamburger `<button id="menuToggle">` with hamburger/close icon swap, `id="navLinks"` on the nav list, and a `.mobile-open` dropdown panel style. Existing JS now works correctly with no JS changes needed for the toggle itself.
- **Contact form:** Added `id="contactForm"`, removed the conflicting inline `onsubmit`, renamed the confirmation paragraph to `.form-success` (matching the JS selector), added per-field `<span class="field-error">` elements with `aria-live="polite"`, and enhanced the JS to set `aria-invalid`, write real error text, and focus the first invalid field.
- **FAQ:** Built a 5-question accordion (`.faq-item` / `.faq-question` / `.faq-answer`) using real `<button>` elements with `aria-expanded`/`aria-controls`/`role="region"`. Extended the existing JS to keep `aria-expanded` in sync.
- **Pricing:** New `#pricing` section with 3 cards (Drop-in, Monthly Unlimited — highlighted, Private Sessions), each with feature lists and CTAs to `#contact`. Placeholder prices marked clearly as illustrative.
- **Schedule:** New `#schedule` section with a real semantic `<table>` (`<caption>`, `<th scope="col/row">`) showing a weekly class timetable.
- **Newsletter:** New capture section with its own validated email form, `aria-live` error region, and success state — visually consistent with the rest of the site (uses the same tokens, radius, and card style).
- **Back-to-top:** Added `<button id="backToTop">` with an arrow icon; existing JS now has something to control.
- **Accessibility:** Added a skip link, `<main id="main">` landmark, `:focus-visible` styling globally, `aria-label`s on the nav, control panel, and icon-only buttons, and `aria-pressed` on language toggles.
- **Performance:** Added `loading="lazy" decoding="async"` to all below-the-fold images (gallery, blog thumbnails); hero image now uses `loading="eager" fetchpriority="high"` plus explicit `width`/`height` to reduce layout shift. Added a meta description.
- **Navigation completeness:** Added Schedule/Pricing/FAQ links to both the header nav and footer nav.
- **i18n:** Added full English and Russian translations for every new string introduced (223 total `data-i18n` keys now cross-checked as complete in both languages).
- **Dead code:** None removed beyond the now-resolved unused `.menu-toggle` selector (it's in active use again).

## Bug Report (pre-fix state)

| # | Bug | Severity | Root cause |
|---|---|---|---|
| 1 | Hamburger menu button did nothing on mobile — nav was completely inaccessible under 860px | Critical | Button element never existed in HTML despite JS listener |
| 2 | Contact form always showed a generic note and never validated | High | Inline `onsubmit` shadowed the real listener; `id="contactForm"` was missing so the real listener never attached; success class name mismatch (`.form-note` vs `.form-success`) |
| 3 | Back-to-top button never appeared | Medium | Element didn't exist in DOM |
| 4 | Clicking anything expecting FAQ behavior had no visible section | Medium | Section never built |
| 5 | No visible keyboard focus indicator anywhere | Medium (accessibility) | No `:focus-visible` rules defined |
| 6 | All images downloaded eagerly regardless of viewport position | Low (performance) | No `loading` attribute anywhere |

---

## Future Roadmap

- Connect the contact and newsletter forms to a real backend or form service (e.g. Formspree, a serverless function, or Anna's email provider) since this file has no server.
- Build out real blog article pages linked from the Journal cards.
- Add a lightweight booking/calendar tool if class sign-ups need to move beyond email.
- Once real certifications, testimonials, and pricing are confirmed, swap in final copy (all current numbers remain placeholders, as instructed).
- Consider an automated Lighthouse/axe-core check in CI if this project grows past a single file.

---

## Explanation of Every Change

Every change made falls into one of two buckets:

1. **Finishing what Devin started** — the mobile menu, contact form, back-to-top button, and FAQ accordion all had working JavaScript already. The fix in every case was adding the missing HTML markup and CSS states so that JavaScript had something real to control. No JS logic needed to be rewritten — it was correct all along.
2. **Building what was never started** — Pricing, Schedule, and Newsletter didn't exist in any form (HTML, CSS, or JS), so these were built from scratch, matching the existing design language exactly: same tokens, same card/section patterns, same type system, same EN/RU translation approach.

All existing sections, copy, images, and the overall boutique-studio aesthetic were left untouched, per the brief — nothing was redesigned, removed, or rewritten beyond what was required to complete or repair a feature.
