# Chintya Testimonial Video in Stories of Impact — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the static featured photo in the Stories of Impact section with Chintya's graduation testimonial video — muted autoplay + loop, burned-in subtitles, click-to-unmute — laid out video-left / text-right.

**Architecture:** One-time `ffmpeg` transcode of the 112 MB source `.mov` into small, trimmed, self-hosted `assets/video/chintya.{mp4,webm}` (logo-card tail removed). The featured `.story-feature` article swaps its `<img>` for a `<video>` with an unmute button. CSS turns the featured story into a two-column flex layout (stacks on mobile). `main.js` plays/pauses the video via IntersectionObserver and toggles mute — respecting `prefers-reduced-motion`.

**Tech Stack:** Static HTML + CSS + vanilla JS. `ffmpeg` (libx264 / libvpx-vp9) for transcode. No build system, no test framework — verification is by inspecting transcode output and observing behavior in a browser.

**Spec:** `docs/superpowers/specs/2026-06-09-chintya-impact-video-design.md`

---

## File Structure

- `assets/video/` — **new** directory holding the shipped video assets.
  - `chintya.mp4` — H.264/AAC, faststart. Primary source.
  - `chintya.webm` — VP9/Opus. Fallback source.
- `index.html` — featured story markup (`#impact` section, the `article.story.story-feature`).
- `styles.css` — featured two-column layout, `.story-video` rules, `.video-unmute` button, mobile stack.
- `main.js` — video play/pause observer + unmute toggle, inside the existing IIFE.
- `Community Assist Carousel.mov` — source, stays in repo root, **not** shipped/referenced by the page.

---

## Task 1: Transcode and trim the video

**Files:**
- Create: `assets/video/chintya.mp4`
- Create: `assets/video/chintya.webm`

- [ ] **Step 1: Create the output directory**

```bash
cd "/Users/beerrodenburg/Documents/Claude Code/Community Assist V2"
mkdir -p assets/video
```

- [ ] **Step 2: Find the trim point (end of testimonial, before the logo card)**

The logo card appears in the last several seconds. Extract 2 fps of frames from 58s onward and inspect them to find the last frame that still shows Chintya (not the black logo card):

```bash
rm -rf /tmp/endframes && mkdir -p /tmp/endframes
ffmpeg -ss 58 -i "Community Assist Carousel.mov" -vf "fps=2,scale=240:-1" -frames:v 44 /tmp/endframes/e%03d.jpg
ls /tmp/endframes
```

Read frames (e001 = 58.0s, each subsequent +0.5s: time = 58 + (n-1)*0.5) until you find where Chintya cuts to the logo card. Set `CUT` to the timestamp of the **last Chintya frame** (round down to a clean point). Expected: somewhere around 66–72s. Record the chosen value, e.g. `CUT=70`.

- [ ] **Step 3: Transcode the trimmed MP4**

Replace `70` with your `CUT` value:

```bash
ffmpeg -y -i "Community Assist Carousel.mov" -t 70 \
  -c:v libx264 -crf 28 -preset slow -pix_fmt yuv420p \
  -vf "scale=720:-2" \
  -c:a aac -b:a 96k -movflags +faststart \
  assets/video/chintya.mp4
```

- [ ] **Step 4: Transcode the trimmed WebM**

```bash
ffmpeg -y -i "Community Assist Carousel.mov" -t 70 \
  -c:v libvpx-vp9 -crf 33 -b:v 0 \
  -vf "scale=720:-2" \
  -c:a libopus -b:a 96k \
  assets/video/chintya.webm
```

- [ ] **Step 5: Verify output (size, dimensions, no logo-card tail)**

```bash
ls -lh assets/video/
ffprobe -v error -select_streams v:0 -show_entries stream=width,height,codec_name -show_entries format=duration -of default=noprint_wrappers=1 assets/video/chintya.mp4
ffmpeg -y -sseof -1 -i assets/video/chintya.mp4 -frames:v 1 /tmp/lastframe.jpg
```

Expected: each file is a few MB (well under ~10 MB; if larger, raise `-crf` and re-run). Width 720, height 900 (4:5), duration ≈ your `CUT`. Read `/tmp/lastframe.jpg` and confirm it shows **Chintya, not the logo card**. If the logo card is still present, lower `CUT` and redo Steps 3–5.

- [ ] **Step 6: Commit**

```bash
git add assets/video/chintya.mp4 assets/video/chintya.webm
git commit -m "Add transcoded Chintya testimonial video assets"
```

---

## Task 2: Swap the featured story image for the video

**Files:**
- Modify: `index.html` (the `article.story.story-feature` inside `#impact`, currently lines ~263–272)

- [ ] **Step 1: Replace the featured `.story-img` block with a `.story-video` block**

Find this in `index.html`:

```html
          <article class="story story-feature reveal">
            <div class="story-img"><img src="assets/img/impact-chintya.webp" width="665" height="520" alt="Ni Putu Chintya Pratiwi in cap and gown holding a Happy Graduation sign" loading="lazy" /></div>
            <div class="story-body">
```

Replace **only** the `<div class="story-img">…</div>` line with:

```html
            <div class="story-video">
              <video class="story-video__el" muted loop playsinline preload="none"
                     poster="assets/img/impact-chintya.webp"
                     aria-label="Ni Putu Chintya Pratiwi in cap and gown sharing her graduation testimonial">
                <source src="assets/video/chintya.webm" type="video/webm" />
                <source src="assets/video/chintya.mp4" type="video/mp4" />
                <img src="assets/img/impact-chintya.webp" width="665" height="520" alt="Ni Putu Chintya Pratiwi in cap and gown holding a Happy Graduation sign" />
              </video>
              <button type="button" class="video-unmute" aria-pressed="false" aria-label="Unmute Chintya's testimonial">
                <svg class="icon-muted" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H2v6h4l5 4V5z"/><line x1="23" y1="9" x2="17" y2="15"/><line x1="17" y1="9" x2="23" y2="15"/></svg>
                <svg class="icon-unmuted" viewBox="0 0 24 24" width="20" height="20" aria-hidden="true" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 5 6 9H2v6h4l5 4V5z"/><path d="M15.5 8.5a5 5 0 0 1 0 7"/><path d="M19 5a9 9 0 0 1 0 14"/></svg>
              </button>
            </div>
```

Leave the `<div class="story-body">…</div>` (tag, "Chintya" heading, paragraphs) unchanged. Leave Indah and Wahyu unchanged.

- [ ] **Step 2: Verify markup loads**

Open `index.html` in a browser and scroll to Stories of Impact. Expected: featured slot shows the poster image (video is `preload="none"` and not yet started by JS), with a round speaker button in the bottom-right corner. Layout may look unstyled/cramped — that's fixed in Task 3.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Swap featured impact photo for Chintya video markup"
```

---

## Task 3: Style the featured video layout and unmute button

**Files:**
- Modify: `styles.css` (Impact stories block ~lines 274–292, and the mobile `@media` block ~lines 565–567)

- [ ] **Step 1: Change the featured story to a two-column layout and add video/button rules**

In the Impact stories block, change line 278 from:

```css
.story-feature { grid-row: span 2; display: flex; flex-direction: column; }
```

to:

```css
.story-feature { grid-row: span 2; display: flex; flex-direction: row; align-items: stretch; }
.story-feature .story-video { flex: 0 0 44%; min-height: 360px; }
.story-feature .story-body { flex: 1; align-self: center; }
```

Then add these rules immediately after the existing `.story-img img { … }` rule (line 282):

```css
.story-video { position: relative; overflow: hidden; background: var(--tint-blue); }
.story-video video { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; }
.video-unmute {
  position: absolute; right: 12px; bottom: 12px; z-index: 2;
  width: 44px; height: 44px; border-radius: 50%; border: none; cursor: pointer;
  background: rgba(18, 20, 30, .55); color: #fff;
  display: grid; place-items: center;
  -webkit-backdrop-filter: blur(4px); backdrop-filter: blur(4px);
  transition: background .2s var(--ease), transform .2s var(--ease);
}
.video-unmute:hover { background: rgba(18, 20, 30, .8); transform: scale(1.06); }
.video-unmute:focus-visible { outline: 3px solid var(--blue); outline-offset: 2px; }
.video-unmute .icon-unmuted { display: none; }
.story-video.is-unmuted .video-unmute .icon-muted { display: none; }
.story-video.is-unmuted .video-unmute .icon-unmuted { display: block; }
```

- [ ] **Step 2: Add the mobile stack rule**

In the `@media` block where `.impact-grid` collapses to one column (~line 565), change:

```css
  .impact-grid { grid-template-columns: 1fr; }
  .story-feature { grid-row: auto; }
  .story-feature .story-img { min-height: 300px; max-height: 380px; }
```

to:

```css
  .impact-grid { grid-template-columns: 1fr; }
  .story-feature { grid-row: auto; flex-direction: column; }
  .story-feature .story-img { min-height: 300px; max-height: 380px; }
  .story-feature .story-video { flex: none; width: 100%; max-width: 360px; margin: 0 auto; aspect-ratio: 4 / 5; min-height: 0; }
```

- [ ] **Step 3: Verify layout (desktop + mobile)**

Reload `index.html`. Expected (desktop ≥ ~900px): portrait video on the left (~44% width) at full featured height, Chintya's text vertically centered to its right; the speaker button sits over the bottom-right of the video. Resize to a narrow/mobile width: the featured story stacks to video-on-top (centered, 4:5, max 360px wide) with text below. Indah/Wahyu unchanged.

- [ ] **Step 4: Commit**

```bash
git add styles.css
git commit -m "Style featured impact story as video-left / text-right with unmute button"
```

---

## Task 4: Wire up autoplay-in-view and unmute toggle

**Files:**
- Modify: `main.js` (inside the IIFE, before the closing `})();` at line 118)

- [ ] **Step 1: Add the video controller block**

Insert this block immediately before the final `})();` (after the reveal/progress-bar `if/else` that ends at line 117). It reuses the `reduce` variable declared at line 88:

```js
  /* ---------- Impact testimonial video ---------- */
  var storyVideo = doc.querySelector(".story-video");
  if (storyVideo) {
    var vid = storyVideo.querySelector("video");
    var unmuteBtn = storyVideo.querySelector(".video-unmute");

    // Autoplay (muted) only when in view and motion is allowed; pause when out of view.
    if (vid && !reduce && "IntersectionObserver" in window) {
      var vio = new IntersectionObserver(function (entries) {
        entries.forEach(function (e) {
          if (e.isIntersecting) { vid.play().catch(function () {}); }
          else { vid.pause(); }
        });
      }, { threshold: 0.4 });
      vio.observe(vid);
    }

    // Unmute button: unmute + restart from the top; click again to re-mute.
    if (vid && unmuteBtn) {
      unmuteBtn.addEventListener("click", function () {
        if (vid.muted) {
          vid.muted = false;
          vid.currentTime = 0;
          vid.play().catch(function () {});
          storyVideo.classList.add("is-unmuted");
          unmuteBtn.setAttribute("aria-pressed", "true");
          unmuteBtn.setAttribute("aria-label", "Mute Chintya's testimonial");
        } else {
          vid.muted = true;
          storyVideo.classList.remove("is-unmuted");
          unmuteBtn.setAttribute("aria-pressed", "false");
          unmuteBtn.setAttribute("aria-label", "Unmute Chintya's testimonial");
        }
      });
    }
  }
```

- [ ] **Step 2: Verify behavior**

Reload `index.html` and check each behavior:
- Scroll the featured video into view → it **starts playing muted and loops** (subtitles burned in). Scroll it well out of view → it **pauses**.
- Click the speaker button → audio plays, video **restarts from the top**, icon switches to the "unmuted" speaker, button `aria-label` becomes "Mute…". Click again → mutes, icon/label revert.
- In DevTools, emulate `prefers-reduced-motion: reduce` and hard-reload → video does **not** autoplay (poster stays); clicking the button still plays it with sound.

- [ ] **Step 3: Commit**

```bash
git add main.js
git commit -m "Play/pause Chintya video in view and toggle unmute"
```

---

## Task 5: Final verification

- [ ] **Step 1: Full-page smoke check**

Reload `index.html` and confirm, in order:
- Page loads with no console errors.
- Impact section: featured video autoplays muted in view, pauses out of view, unmute works and restarts from 0.
- Indah and Wahyu stories are visually unchanged.
- Mobile width: featured story stacks (video top, text below); button still tappable.
- Reduced-motion: no autoplay, manual play via button works.

- [ ] **Step 2: Confirm the 112 MB source is not referenced by the page**

```bash
grep -n "Carousel.mov" index.html main.js styles.css; echo "exit: $?"
```

Expected: no matches (grep prints nothing / exit 1). The shipped page references only `assets/video/chintya.{webm,mp4}`.

- [ ] **Step 3: Final commit (if any stray changes)**

```bash
git add -A && git status
git commit -m "Finalize Chintya impact video integration" || echo "nothing to commit"
```

---

## Notes for the implementer

- **Source `.mov` stays in the repo root**, untracked or as-is — do not delete it and do not reference it from the page. Only the transcoded `assets/video/` files ship.
- If `chintya.mp4`/`chintya.webm` come out larger than ~10 MB, raise the CRF values (e.g. MP4 `-crf 30`, WebM `-crf 36`) and re-run Task 1 Steps 3–5. Smaller is better for mobile viewers on slower connections.
- The two SVGs inside the button are a muted speaker (with an "x") and an unmuted speaker (with sound waves); CSS toggles which is shown via the `.is-unmuted` class on `.story-video`.
- The leftover `.story-feature .story-img` rules in `styles.css` (lines 279–280) are harmless dead rules once the featured `<img>` is gone — leave them to keep the diff focused.
