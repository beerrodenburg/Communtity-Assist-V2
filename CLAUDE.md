# Community Assist — Landing Page

Project context for Claude Code. Read this first every session.

## What we're building

A **fundraising-first, single-page marketing landing page** for **Community Assist**, a Bali-based charity collective. It re-tells the story in their 2026 deck (`COMMUNITYASSIST__2026.pdf`) as a public website whose #1 job is to get visitors to **donate / sponsor**.

**Decisions locked in (from brainstorming):**
- **Primary goal:** donate / sponsor first. Donate is the sticky primary CTA everywhere.
- **Scope:** one long-scroll landing page with anchor nav (no multi-page site).
- **Functionality:** marketing site only — links out. Donate → bank details / email, events → external links / Instagram, forms → email. **No backend, no payments, no DB.**
- **Aesthetic:** *cleaner & more modern* take on the deck. Same brand colors + striped-heart logo, but more whitespace, editorial layout, and hand-drawn doodles used **sparingly as accents** (not wallpaper).
- **Narrative arc:** *impact-led funnel* — hook → credibility → mission → impact stories → what your gift does → 2026 goal → Ways to Give → Events → Sponsors → final donate.

## Current phase

**Planning only.** We are NOT building the site in this session. Deliverables now:
1. `CLAUDE.md` (this file)
2. `docs/brand-and-design-system.md` — extracted colors, type, doodle/photo language, components
3. `docs/website-outline.md` — the section-by-section build brief

**The build happens in a later, fresh session.**

## The Google Stitch workflow (important)

Between planning and building, the human will generate visual mockups with **Google Stitch**. For each section they will paste that section's brief from `docs/website-outline.md` **plus the matching `pages/slide-XX.jpg`(s)** into Stitch to get a mockup. So the outline is written *per section as a paste-ready design prompt*, and every section names the exact slide files to attach. Keep it that way.

## Assets on disk

- `COMMUNITYASSIST__2026.pdf` — source of truth (31 pages, 70.8 MB). Read with the `pages:` param only.
- `pages/slide-01.jpg` … `slide-31.jpg` — every deck page exported as JPG. These are the image source for the site and the Stitch references. (Full-slide exports; individual photo cut-outs / doodles still need extracting at build time.)
- `LOGO_Main_Colored_CommunityAssist_Indosole.svg` — **official vector logo** (blue striped-line heart + "COMMUNITY ASSIST" wordmark). Use this, don't recreate it.

Slide → content map lives in `docs/website-outline.md`.

## About Community Assist (for accurate copy)

- A non-profit collective **founded by Indonesian businesses**, based in **Bali (Canggu)**. Founding partners: **Indosole** (recycled footwear, @indosole) and **PNNY** (Bali music collective est. 2014, @pnny_).
- Raises funds for the scholarship programs of two beneficiaries: **Ragam Foundation** (@ragamfoundation; "ragam" = *diversity* in Indonesian; legal entity **Ragam Kemanusiaan Indonesia**) and the **Green School Foundation**.
- Best known for an annual **3x3 charity basketball tournament** (5 editions so far) plus community events.

### Key figures (verify before publishing)
- **Raised to date:** IDR 942,936,401.
- **Raised in 2025:** IDR 358,760,536 (deck also rounds this to ~Rp 358,000,000).
- **2026 fundraising goal:** IDR 600,000,000 → ~30 scholarships + a dedicated community manager.
- **Event-day "Giveback" goal:** Rp 480,000,000 (referenced for the basketball day specifically).
- **2025 by the numbers:** 500+ attendees, 21 open teams, 5 women's teams, 5 youth teams.
- **2026 budget split:** 69.2% to the scholarship program, 30.8% to event management/logistics.
- **Scholarship mix:** 18 middle-school (SMP, 60.4%), 8 high-school (SMA, 26.2%), 4 university (13.4%).

### The 4 events in 2026
1. **Beach Games** @ Times Beach Warung — **20 June 2026** (dodgeball, sandcastle contest, community market).
2. **Merch Launch** @ Luigi's — **12 September 2026** (2026 merch, pizza-making, bocce).
3. **3x3 Charity Basketball Tournament** — **24 October 2026**.
4. **Auction Night** — **December 2026** (20 exclusive lots).

### Ways to give
- **Individuals — Donor Ladder** (pay monthly / quarterly / annually): Rp 1M (⅙ scholarship), Rp 6M (1), Rp 12M (2), Rp 24M (4), Rp 60M (10), Rp 120M (20).
- **Sponsor a Child:** Rp 6M / year = one child's full year (tuition, learning materials, school uniform, dedicated community manager).
- **Community Partner:** Rp 20M / year bundle (3 scholarships, full event registration, 2 auction-dinner tickets, IG mentions + custom social ad video, quarterly marketing support).
- **Business Sponsor tiers:** Community **Rp 20M** (3 scholarships) · Game Changer **Rp 60M** (10) · Champion **Rp 120M** (20 + event naming rights). Each adds logo placement, on/off-site banners, PA mentions, social features.
- **Be part of the day (event participation):** sponsor/donate · register a basketball team (Rp 1,000,000; 3 players + 2 reserves) · run a workshop/presentation · donate a raffle/auction prize · book a community-market stall.

### How to donate (bank transfer)
- Account name: **Ragam Kemanusiaan Indonesia** · Acct #: **800210750800** · Bank code: **008** (BNI) · SWIFT: **BNIAIDJAXXX**
- Bank address: Canggu Berawa, Jl. Subak Sari No. 11, Tibubeneng, Bali 80361

### Contact & links
- Email: **communityassist@indosole.com** · Phone/WA: **+62 812-3760-9071**
- Existing site: **www.communityassistbali.com** (review at build time)

### Copy caveats (don't copy errors verbatim)
- The deck has minor typos ("we've collectively raise", "one children", "n-store"). **Polish all copy for the site.**
- The **Donor Ladder USD figures look inconsistent** (Rp 60M is shown at the same $1,500 as Rp 24M). **Re-derive USD from a single IDR→USD rate, or drop USD,** rather than copying the deck values.

## Proposed tech (confirm at build time — do NOT build yet)
- Static site, no backend. Leaning **HTML + Tailwind CSS** (pairs well with Stitch output) or **Astro** for content + automatic image optimization.
- Optimize the large slide JPGs (resize + WebP/AVIF) before shipping.
- Mobile-first, responsive, accessible (WCAG AA contrast — see design system), prefers-reduced-motion respected.
- Build with the `frontend-design` skill.

## Pointers
- Brand & design system → `docs/brand-and-design-system.md`
- Full section-by-section build brief → `docs/website-outline.md`
