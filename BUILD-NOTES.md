# Community Assist — Build Notes

The landing page is built and live-previewable by opening **`index.html`** in any browser. No build step, no dependencies.

## ⚠️ Before you publish (must-do)

1. **Progress bar is a PLACEHOLDER.** The 2026-goal section shows a hard-coded **75%** bar. It is *not* a real figure.
   - File: [index.html](index.html) → search `data-progress` (in the `.goal` section). Set it to the real percentage toward Rp 600,000,000, and update the `75%` label text next to it.
   - To remove the bar entirely instead, delete the `<div class="progress">…</div>` block.
2. **Verify every number** against the latest from the team (the deck had minor errors):
   - Rp 942M+ raised to date · Rp 358M in 2025 · 500+ attendees · 200+ businesses · Rp 600M goal · 69% to scholarships · scholarship mix 18/8/4.
3. **Sponsor & partner names are rendered as text, not logos.** The "In partnership with" strip (credibility band) and the sponsor wall use styled wordmarks. Swap in real logo SVGs/PNGs at build for a polished look, and **transcribe the full Community Heroes list from `pages/slide-29.jpg`** — right now it only shows Champions (4) + Game Changers (10) + a "+ 180 more" tile, to avoid publishing unverified business names.

## What was built

- **Single static page**, plain HTML + hand-written CSS + vanilla JS. Stack chosen this session: *Plain HTML + CSS* (easiest to host & hand off).
- **Files**
  - `index.html` — the whole page (sticky nav → hero → credibility → mission → impact → gift → 2026 goal → ways to give → events → sponsors → final CTA → footer).
  - `styles.css` — brand tokens (`:root`), type scale, every component, responsive breakpoints, motion.
  - `main.js` — sticky-nav shadow, mobile menu, Ways-to-Give tabs (keyboard accessible), click-to-copy bank fields, scroll-reveal + progress-bar fill.
  - `assets/heart.svg` — official striped heart with the wordmark cropped out, recolored to `currentColor` (used as the `#i-logo-heart` sprite symbol in the nav lockup). `assets/logo-full.svg` — full official logo (heart + "COMMUNITY ASSIST" wordmark), single-color via `currentColor` (used as the `#i-logo-full` sprite symbol in the footer). `assets/heart-mark.svg` — simplified striped-heart favicon/motif.
  - `assets/img/*.webp` — section imagery, cropped from the deck slides and optimised (whole folder ≈ 0.9 MB).
- **Fonts** load from CDN: **General Sans** (Fontshare, display) + **Inter** (Google, body) + **Caveat** (marker accent). If the site must work fully offline, self-host these.

## Images

All photos were **cropped from `pages/slide-*.jpg`** and converted to WebP with `cwebp` (see the crop coordinates in your shell history, or re-run from the originals). They are *framed photo crops*, **not** true transparent cut-outs — the deck's neon-glow outlines on the subjects happen to give a cut-out feel. The two students flanking the 2026-goal section are masked to fade their edges into the blue. Source-image → section map is in [docs/website-outline.md](docs/website-outline.md).

To re-optimise or swap an image: `cwebp -q 82 -crop X Y W H -resize 800 0 pages/slide-NN.jpg -o assets/img/name.webp`.

## Links & behaviour (marketing-only, no backend)

- **Donate** buttons scroll to the final CTA, where the **bank-transfer card** has click-to-copy fields (Account · Number · BNI/008 · SWIFT · Address).
- Contact = `mailto:communityassist@indosole.com` and `https://wa.me/6281237609071`.
- Instagram = `https://www.instagram.com/communityassistbali/` (confirmed this session).
- "Register a team / book a stall / become a sponsor" currently point at Donate/contact — **wire to real registration or ticket links when they exist** (open item from the outline).

## Accessibility & performance

- Semantic landmarks, skip-link, `aria` on nav/tabs/dialog, alt text, visible focus rings.
- **WCAG AA contrast** checked: bright pink `#E94E9C` is decoration-only; text/white-on-pink uses `--pink-deep` (~5.3:1); `--grey` darkened to ~5:1 on Paper; blue panels use Deep Blue `#4661AD` (5.9:1 with white).
- `prefers-reduced-motion` fully respected (reveals + bar + hovers disabled). Reveal logic has a no-JS / no-IntersectionObserver fallback that shows all content.
- Images are lazy-loaded below the fold and sized to avoid layout shift.

## Still to confirm (from the outline's open items)

- Tagline **"Come · Support · Play."** still current?
- Real "raised so far" figure for the progress bar (see must-do #1).
- Whether to show **USD** anywhere (deck values were inconsistent — currently omitted, recommended).

## Deploy

Drag the project folder onto **Netlify Drop**, or push to GitHub Pages / Cloudflare Pages / Vercel — it's a static site, so any static host works. Nothing to build.
