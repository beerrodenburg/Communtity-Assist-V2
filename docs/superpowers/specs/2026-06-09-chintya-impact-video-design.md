# Chintya testimonial video in Stories of Impact — Design

**Date:** 2026-06-09
**Status:** Approved (design); pending implementation plan

## Goal

Add Chintya's graduation testimonial video to the **Stories of Impact** section of the
Community Assist landing page, replacing the static featured photo with a muted,
autoplaying, looping portrait video. Chintya is already the featured story, so the video
is a direct upgrade to that slot. Her words (burned-in English subtitles) carry the
message while muted; an unmute control gives the full voice.

## Source asset

- `Community Assist Carousel.mov` (repo root)
- 112 MB, H.264, **portrait 4:5 (1024×1280)**, 78.4s, 30fps, has audio (her voice)
- Content: Chintya in cap and gown giving a thank-you testimonial with burned-in English
  subtitles, ending on a full-screen Community Assist logo card.
- **Cannot ship as-is** (112 MB). Must be trimmed + compressed.

## Decisions (locked)

1. **Playback:** muted autoplay + loop, with burned-in subtitles. Click to unmute.
2. **Loop content:** trim the trailing logo card (and dead frames) so the loop stays on
   the testimonial and cycles smoothly.
3. **Featured layout:** video left, text right (desktop). Stacks to video-top / text-below
   on mobile.

## Build step — transcode the video

One-time `ffmpeg` transcode of `Community Assist Carousel.mov` into web assets:

- **Trim** off the trailing Community Assist logo card so the loop ends on her testimonial
  and cycles cleanly. Exact cut point determined at build time by inspecting frames (logo
  card appears in the last few seconds).
- **Compress** to:
  - `assets/video/chintya.mp4` — H.264, `yuv420p`, `-movflags +faststart`, target a few MB.
  - `assets/video/chintya.webm` — VP9, similar target.
- Keep portrait 4:5 dimensions (downscale if it reduces size without hurting quality).
- **Poster:** reuse existing `assets/img/impact-chintya.webp` — no new poster asset needed.
- New directory: `assets/video/`.

## Markup (`index.html`)

In the `#impact` section, the featured story (`article.story.story-feature`, currently
lines ~263–272) changes:

- Replace the `.story-img` `<img>` with a `.story-video` wrapper containing:
  - `<video>` with `muted`, `loop`, `playsinline`, `preload="none"`, `poster` =
    `assets/img/impact-chintya.webp`, both `<source>`s (webm then mp4), and the existing
    alt text intent preserved via surrounding markup / `aria-label`.
  - An **unmute button** overlaid bottom corner (speaker icon), `aria-label="Unmute
    Chintya's testimonial"`, toggling to "Mute…" when active.
- The `.story-body` (tag, "Chintya" heading, body paragraphs) is unchanged in content.
- Indah and Wahyu side stories (`.story-col`) are **untouched**.

## Styles (`styles.css`)

- `.story-feature` changes from `flex-direction: column` to a two-column internal layout
  (video left, body right) on desktop. Constrain the portrait video to a sensible width of
  the featured column so the text column stays readable.
- New `.story-video` rules: positioned wrapper, `object-fit: cover`/`contain` as
  appropriate for 4:5, rounded to match `.story-img`.
- Unmute button: small circular control, brand styling, visible focus ring, hover state.
- **Mobile** (existing `@media` where `.impact-grid` collapses to 1 column, ~line 565):
  featured story stacks to video-on-top, text-below.

## Behavior (`main.js`)

- **Lazy play/pause via IntersectionObserver** (reuse the existing pattern, ~line 98):
  - Play the video when it enters the viewport; pause when it leaves. Keeps it off the
    critical path and saves data/battery.
- **Unmute toggle:** button click unmutes the video **and restarts it from 0** so the
  visitor hears the testimonial from the top; clicking again re-mutes. Update button
  icon + `aria-label`/`aria-pressed` to reflect state.
- **`prefers-reduced-motion`:** detected at `main.js:88`. When reduced motion is
  preferred, do **not** autoplay — show the poster with a play button; the user starts
  playback manually.

## Fallbacks & accessibility

- If `<video>` or autoplay is unsupported/fails, the `poster` image stands in and the
  story still reads.
- Burned-in subtitles make the muted loop fully legible.
- Unmute control is keyboard-focusable with an `aria-label` and visible focus state.

## Tradeoff (accepted)

Even compressed, the video adds a few MB that autoplays on the page — heavier than the
static image it replaces. Lazy-load (play only when scrolled into view) keeps it off the
critical path. Accepted for the emotional payoff of the featured story.

## Files touched

- `index.html` — featured story markup.
- `styles.css` — featured two-column layout + mobile stack + unmute button.
- `main.js` — play/pause observer + unmute toggle.
- `assets/video/` — new directory with `chintya.mp4` + `chintya.webm`.
- One-time `ffmpeg` transcode of `Community Assist Carousel.mov` (source stays in repo
  root / is not shipped).

## Out of scope

- No changes to Indah/Wahyu stories or other sections.
- No backend, no analytics, no video hosting service — self-hosted static files only.
