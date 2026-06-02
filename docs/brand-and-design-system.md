# Community Assist — Brand & Design System

Extracted from `COMMUNITYASSIST__2026.pdf` (colors sampled from the `pages/` slide exports + the logo SVG). This is the single reference for the website's look. Direction: **cleaner & more modern** — keep the brand DNA, add whitespace and editorial structure, use doodles sparingly.

---

## 1. Brand personality

Warm, youthful, grassroots-but-polished. Bali surf/skate/community energy meets a real charity mission. Optimistic, human, inclusive. On the site this reads as: confident type, lots of breathing room, candid photography, the striped-heart motif, and small hand-drawn accents that keep it from feeling corporate.

Keep: authentic photos, the heart, sky-blue, playful touches.
Lose (vs. the deck): doodle wallpaper, cramped slides, inconsistent type. Make it calmer and more premium.

---

## 2. Color palette

Sampled hex values, with roles and accessibility notes.

| Token | Hex | Role |
|---|---|---|
| **Sky Blue** | `#69A0C9` | Signature brand fill. Big brand moments, full-bleed "blue" sections, the heart, graphic accents. **Not for small text.** |
| **Deep Blue** | `#4661AD` | Primary action color: buttons, links, eyebrows, headings on light, text on blue panels. The logo's blue. |
| **Ink** | `#1F2024` | Body text on light backgrounds. |
| **Paper** | `#F1EFF1` | Default page background (warm off-white). |
| **White** | `#FFFFFF` | Cards and high-contrast surfaces. |
| **Orchid (soft)** | `#D4A5CC` | Soft accent + the **"Individuals / personal"** theme (tints, surfaces). |
| **Pink (hot)** | `#E94E9C` | High-energy accent: doodles, highlights, icon glyphs. Sparingly. |
| **Yellow** | `#F4C84B` | Sparing highlight / doodle accent. |
| **Red** | `#E23B2E` | Rare energy accent (event heart, emphasis). Optional. |
| **Grey** | `#9A9A9D` | Secondary text, hairlines. |
| **Card Grey** | `#E7E6E8` | Subtle card fill on Paper (alt to white). |

**Tints** (for soft section backgrounds / icon tiles): Blue tint `#E3EBF3`, Orchid tint `#F6E9F2`, Yellow tint `#FBF1D2`.

**Theme convention (from deck slide 23):** Individuals = orchid/pink family; Businesses = blue family. Carry this into the Ways-to-Give section.

### Contrast rules (WCAG AA) — important
- ✅ **Ink on Paper/White** — body text. High contrast.
- ✅ **White on Deep Blue `#4661AD`** — 5.9:1. Use Deep Blue for any blue panel that carries readable text/buttons.
- ✅ **Deep Blue on Paper** — 4.9:1. Good for links, eyebrows, headings.
- ⚠️ **Sky Blue `#69A0C9`** is a *fill/graphic* color, ~2.8:1 with white — **only** for huge display text or decoration, never body copy. For a blue section with paragraphs, use Deep Blue as the background instead.

---

## 3. Typography

The deck uses a neutral grotesque (Helvetica/Arial family) for headings + body, a chunky rounded grotesque for the logo wordmark, and a hand-drawn marker for the cover headline. For the web, modern + on-brand:

- **Display / headings — `General Sans`** (Fontshare, free). Geometric-humanist, friendly, complements the chunky logo. *Faithful-to-deck alt: `Inter` or licensed `Helvetica Now`.*
- **Body / UI — `Inter`** (Google Fonts). Neutral, legible — closest free match to the deck body.
- **Hand-drawn accent — `Caveat`** (Google Fonts), used **only** for the occasional marker word/label. Default to real SVG doodles instead.
- **Logo wordmark** is the SVG asset — never re-typeset it.

### Type scale (desktop; use `clamp()` for fluid sizing)
| Use | Size | Weight | Line-height |
|---|---|---|---|
| Hero display | 64–80px | 700 | 1.05 |
| H2 section | 36–44px | 600–700 | 1.1 |
| H3 | 24–28px | 600 | 1.2 |
| Lead / body-L | 18–20px | 400 | 1.5 |
| Body | 16px | 400 | 1.6 |
| Eyebrow / label | 13px | 600, UPPERCASE, +0.08em tracking, Deep Blue | 1.2 |
| Caption | 14px | 400, Grey | 1.5 |

---

## 4. Logo & heart motif

- **Primary logo:** `LOGO_Main_Colored_CommunityAssist_Indosole.svg` — striped/scanline heart above the "COMMUNITY ASSIST" wordmark. Use as-is.
- **Heart mark alone** = the recurring brand icon. It appears in Sky Blue, Deep Blue, Orchid, and White across the deck — recolor to suit the section. Great for: nav mark, section dividers, favicon, loading state, bullets.
- Clear space around the logo ≥ the height of one heart "row." Don't stretch, recolor mid-mark, or add effects.

---

## 5. Doodles / hand-drawn accents (use sparingly)

The deck's vocabulary: crayon-textured **stars, hearts, squiggles, spirals, sparkles, smileys, and underlines**, mostly in blue, plus pink/yellow. In the *cleaner* direction, treat these as **seasoning, not wallpaper**:
- One or two accents per section max (e.g., a hand-drawn underline under a heading, a small star by a stat).
- Prefer Deep/Sky Blue doodles; pink/yellow for rare pop.
- Extract them as transparent SVG/PNG from the slides at build time (they're scattered across slides 01, 07, 08, 14, 15, 17, 26, 28, 30).
- Respect `prefers-reduced-motion` if any are animated.

---

## 6. Photography

Candid, vibrant, film-grain photos of Bali community life — beach games, basketball, gatherings, scholarship students. Treatments seen in the deck, reusable on the site:
- **Full-bleed** hero/section imagery (sky + people).
- **Cut-out portraits** with a hand-drawn glow outline (pink/yellow) — perfect for the impact-story people.
- **Collage** of cut-out figures on a sky background (hero, closing).
- Mix of color and high-contrast B&W action shots (see slide 28).
- Source images live in `pages/`; optimize (resize + WebP/AVIF) before shipping.

---

## 7. Components

- **Buttons.** Primary = Deep Blue fill, white text, pill (`rounded-full`) or `rounded-xl`, generous padding, subtle hover lift/darken. Secondary = Deep Blue outline on Paper. "Donate" is always the most prominent button on screen.
- **Cards.** White or Card-Grey on Paper, `rounded-2xl` (16–24px), hairline border or soft shadow, roomy padding. **Icon tile** = rounded square with a color tint fill + colored glyph (from slides 20–22).
- **Stat block.** Oversized number in Deep Blue + small uppercase label; optional doodle underline/star. (Slides 05, 17.)
- **Tier cards.** Three columns — Community / Game Changer / Champion — Champion highlighted with an Orchid tint; feature rows beneath. (Slide 22.)
- **Donor-ladder rows.** Stacked pill rows, light→dark blue as the amount rises. (Slide 24.)
- **Nav.** Slim sticky header, Paper/blur background, heart mark + wordmark left, anchor links, one bold Donate button right. Mobile = hamburger → sheet.

---

## 8. Layout & spacing

- Content max-width ~1200px; generous gutters; 12-column grid.
- Section vertical padding ~96–128px desktop / 56–72px mobile. Whitespace is the main "modern" lever.
- Editorial asymmetry encouraged (text one side, image the other — like the deck).
- **Section rhythm:** alternate Paper sections with occasional **full-bleed Deep/Sky Blue "brand moment"** sections (dividers, the 2026 goal, final CTA) — echoing the deck's blue divider pages — so the scroll breathes.

---

## 9. Motion (restrained)

Gentle fade / slide-up as sections enter the viewport; small hover states; optional one-time "draw-on" for a doodle accent. Nothing flashy — the modern feel comes from restraint. Always honor `prefers-reduced-motion`.

---

## 10. Quick CSS tokens (starting point)

```css
:root {
  --sky:   #69A0C9;
  --blue:  #4661AD;
  --ink:   #1F2024;
  --paper: #F1EFF1;
  --white: #FFFFFF;
  --orchid:#D4A5CC;
  --pink:  #E94E9C;
  --yellow:#F4C84B;
  --red:   #E23B2E;
  --grey:  #9A9A9D;
  --card:  #E7E6E8;
  --tint-blue:   #E3EBF3;
  --tint-orchid: #F6E9F2;
  --tint-yellow: #FBF1D2;
  --radius: 20px;
  --maxw: 1200px;
}
```
