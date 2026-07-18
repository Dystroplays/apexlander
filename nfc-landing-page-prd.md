# PRD: NFC Tap Card Landing Page — Private Transportation

## 1. Overview

A single-page, mobile-first HTML landing page for a private transportation side business (run with Blake's dad, alongside his Uber Black driving). The page is triggered by tapping an NFC card against a phone — modeled after the church-seat NFC scanners that surface a quick info page. No app, no funnel, no scroll depth beyond what's needed. The entire point is: tap → decide → call or book in under 10 seconds.

**Primary goal:** convert a curious rider (who just asked "hey, do you do private transportation?") into a phone call or booking within seconds, on a phone screen, with zero load lag.

## 2. Constraints (non-negotiable)

- **Pure static HTML + inline CSS.** No JS frameworks, no external stylesheets, no web fonts requiring a fetch, no render-blocking resources. Everything self-contained in one `.html` file. Goal: sub-200ms perceived load on cellular data.
- **Mobile-first, mobile-only design target.** Assume a phone screen (~375–430px width) held vertically. Desktop can degrade gracefully but is not the design target.
- **No page reload / no routing.** Single scroll, single file.
- **Must be reachable at a stable URL** that can be programmed onto an NTAG213/215 NFC chip.

## 3. Page structure (top to bottom)

### Section A — Header: Headline + Contact (side-by-side on larger phones, stacked if needed)
- **Left:** Headline. Something in the spirit of "Premier Private Transportation in the DFW Metroplex." Sub-line: 1 short phrase on service style (e.g., airport runs, events, door-to-door).
- **Right:** Primary CTA block — **"Book Now"** button.
  - Tapping it should give the rider a choice (or default to the higher-converting action): `tel:` link as primary, `sms:` as secondary, `mailto:` as tertiary fallback. Recommend making the button itself a `tel:` link (one-tap call), with a smaller text link below it for "Text us instead."

### Section B — Reviews (directly below header)
- Left/main: "Leave a Review" tap target — links straight out to the Google Business Profile review URL (not a self-hosted form). This should look like a real button, not a footnote.
- Right (or below, stacked on narrow screens): **3 short review capsules** — pull quotes, 1 line each, with reviewer first name + initial. These are static text pulled from real reviews (need actual quotes before build — see Section 6).

### Section C — Trust/Detail strip (optional, keep minimal)
- One line of service area / vehicle type (e.g., "Serving DFW, Love Field & DFW Airport — Sedan & SUV").
- Optionally one vehicle photo if it doesn't slow load (compressed, inlined or lightweight hosted asset).

### Section D — Footer
- Phone number as plain visible text (not just inside a button — some users prefer to manually dial or save the contact).
- Optional: business name / "operating since" line for legitimacy.

## 4. Interaction/UX details

| Element | Behavior |
|---|---|
| Book Now | `tel:` link, large tap target (min 44x44px), high contrast |
| Text Us | `sms:` link, secondary visual weight |
| Email fallback | small `mailto:` link, tertiary |
| Leave a Review | external link to Google Business Profile review URL, opens in same tab is fine |
| Review capsules | static, no carousel/JS — just 3 stacked or side-by-side cards |

## 5. Analytics (lightweight, no JS framework)

Since this is a physical card with a fixed cost, we want to know if it's getting tapped/used. Options, in order of simplicity:
1. Route the NFC chip's programmed URL through a UTM-tagged redirect (e.g., a Bitly link or a simple 1-line server redirect) pointing to the actual HTML page, so tap volume is visible without adding any client-side JS to the page itself.
2. If a tiny bit of JS is acceptable later, a single no-dependency tracking pixel (e.g., a 1x1 image request to a logging endpoint) — deferred, not required for v1.

**Decision needed from Blake:** is a redirect-based tap counter acceptable, or do we skip analytics for v1 and just ship the flat page?

## 6. Content needed before build (blockers)

Claude Code can build the shell today, but the page isn't real without:
- [ ] Actual business name (if different from a personal name)
- [ ] Real phone number for `tel:`/`sms:` links
- [ ] Google Business Profile review link (or confirm platform — Google vs Yelp)
- [ ] 3 real review quotes, with permission to use them publicly, + first name/initial
- [ ] Service area specifics (DFW general? Specific airports — DFW/Love Field? Event/wine tours? Airport-only vs. anywhere?)
- [ ] At least one vehicle photo (or confirm text-only for v1)
- [ ] Confirm legal/operating status — this page is a public storefront; make sure whatever permit/insurance classification (TNC vs. for-hire/limousine under Texas rules) is settled before this goes live and gets shared publicly, since it's a step up in visibility from word-of-mouth.

## 7. Hosting & NFC setup

- **Hosting:** static hosting (GitHub Pages, Netlify, or Vercel) — free tier is sufficient for a single HTML file. Custom domain optional but recommended for a clean, brandable URL to program onto the chip.
- **NFC hardware:** NTAG213 (or 215 for extra margin) NFC card/sticker, programmed with the final URL, then **write-locked** so it can't be accidentally overwritten.
- **Backup access:** include a QR code somewhere on the physical card design (not the webpage) since some older Android phones and locked iPhones won't auto-trigger an NFC tap without NFC actively enabled.

## 8. Explicitly out of scope for v1
- Booking calendar / date-time picker
- Payment collection
- Multi-page site or additional service pages
- CMS or dynamic content — this is a flat file, edited manually when details change

## 9. Open questions for Blake before handing to Claude Code
1. Confirm redirect/analytics approach (Section 5) — yes/no for v1.
2. Confirm review platform link (Google vs. Yelp) and get the 3 quotes.
3. Confirm hosting preference (GitHub Pages is free and simplest if no custom domain is needed yet).
4. Confirm whether a vehicle photo is ready, or ship text-only for v1 and add later.
