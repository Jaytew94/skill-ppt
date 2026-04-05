# SKILL: ppt-design-core
**Step 2 of 4 — Universal Design Rules**
**Theme colors/decorations come from themes/theme-[X].md**

---

## MODULE 1 — 29 LAYOUT CATALOG

### COVER & STRUCTURE
```
L01 Cover Hero Left    — left text, right visual, ghost text bg
L02 Cover Centered     — centered hero, decorative rings
L03 Chapter Break      — single massive word 220px, accent line animates
L24 Closing CTA        — matches cover energy, CTA + contact + QR
```

### SPLIT LAYOUTS
```
L04 Split Content-Left  — 46% content | 54% image (diagonal clip)
L05 Split Visual-Left   — 54% image | 46% content (reversed)
L23 Person Spotlight    — 52% content+quote | 48% image with glow
L27 Product Float       — left title+stat | right 3D/PNG product floating
```

### STATEMENT
```
L06 Big Statement      — single sentence 80-100px, centered
L17 Quote Pullout      — SVG quote marks + centered quote + attribution
```

### DATA
```
L07 Data Metrics       — 2-4 large counter numbers (160px) with labels
L08 Comparison         — 1fr divider 1fr, left=before right=after
L19 Before/After       — 2 panels 50/50, visual + descriptors
L25 Pricing Table      — 3 tier cards, center highlighted with accent
L28 Map + Stat         — SVG outline map + stat card
```

### TIMELINE
```
L09 Timeline Horizontal — animated line left→right, nodes stagger
L10 Timeline Vertical   — left title, right milestone list
```

### MULTI-COLUMN (must include images or strong visual)
```
L12 Three Column       — image card 220px top + number + title + desc × 3
L13 2×2 Grid           — image card 160px top + title + desc × 4
L18 Icon Grid          — left 2×2 icon items | right full-height image
```

### FLOW
```
L20 Process Flow       — numbered circles, arrows, left→right stagger
```

### CINEMATIC / SURPRISE ⚡
```
L11 Full Bleed Image   — full-screen image, gradient overlay, text bottom
L14 Diagonal Split     — skewed color divider, type left, image right
L15 Editorial Typo     — pure typography, massive ghost letter bg
L21 Cinematic Bottom   — image 78% height, text bar at bottom
L22 Staggered Asymmetric — grid-break, overlapping elements
```

### ACCENT / SPECIAL
```
L16 Sidebar Accent     — 110px solid sidebar, rotated label
L26 Contact Grid       — left image | right title + 2×2 icon contact grid
L29 Stacked Card       — image top + overlapping info card below
```

---

## MODULE 2 — LAYOUT VARIANT SYSTEM

```
L04/L05 SPLIT — 3 variants:
  A: diagonal clip-path standard
  B: reversed diagonal angle
  C: no image — solid accent color block + large number

L06 STATEMENT — 3 variants:
  A: centered, white on dark
  B: left-aligned, key word in accent color
  C: text on skewed color band (rotate -1.5deg)

L07 DATA — 2 variants:
  A: 4 small metrics in a row
  B: 2 hero metrics dominant (large)

L11 FULL BLEED — 3 variants:
  A: text bottom-left
  B: text center with frosted glass card
  C: text top-left, strong color grade

L12 THREE COLUMN — 2 variants:
  A: image card top (default)
  B: large 01/02/03 number + accent bg, no image

L13 2×2 GRID — 2 variants:
  A: image card (default)
  B: top-left hero cell in accent, others muted
```

---

## MODULE 3 — ANTI-REPETITION RULES

```
□ Same layout + same variant: max 1× per chapter
□ Same layout (any variant): max 3× entire deck
□ L04/L05 exception: max 5× total
□ Visual surprise (L14/L15/L21/L22): max 1× each
□ 3 consecutive text-heavy slides → insert visual layout
□ 2 data slides → never in same chapter
□ Cover and Closing must match visual energy
```

---

## MODULE 4 — PERSONALITY-DRIVEN LAYOUT SELECTION

**Translate the personality chosen in ppt-brief.md into layout decisions.**

### PERSONALITY A — "IMPACT FIRST"
```
Cover:    L11 Full Bleed if strong imagery / L01 otherwise
Content:  Image takes 55%+ space — use L05, L11, L21, L14
Statements: L06 every 3rd slide
Data:     Max 2 numbers, very large (180px), no charts
Chapter:  Full color bg, single word, NO supporting text
Rhythm (10): L11→L06→L05→L17→L03→L21→L06→L14→L07→L24
```

### PERSONALITY B — "DATA AUTHORITY"
```
Cover:    L04 split — title left, key stat/chart right
Content:  Balanced L04, data-heavy L07, structured L12, L09, L20
Data:     Progress bars, charts, counters on every other slide
Statements: Max 1 per deck (L06 only)
Chapter:  Include supporting stat below chapter word
Rhythm (10): L04→L07→L09→L08→L12→L07→L20→L04→L08→L24
```

### PERSONALITY C — "STORY JOURNEY"
```
Cover:    L01 cinematic — atmospheric headline, subtitle sets problem
Content:  L06, L17, L15 frequently — text as primary medium
Arc:      Act1 (problem) → Act2 (journey) → Act3 (resolution)
Data:     Max 2, only when it proves the story
Chapter:  Cinematic full-color bg, emotional chapter word
Rhythm (10): L01→L06→L11→L17→L03→L05→L06→L15→L22→L24
```

### COMBINED PERSONALITY + THEME = UNIQUE RESULT
```
Tech HUD + Personality A: Octagon full-bleed images + HUD text overlay
Tech HUD + Personality B: Circuit-styled charts + HUD metric panels
Tech HUD + Personality C: Cinematic octagon frames + dramatic L-bracket quotes

→ 10 themes × 3 personalities = 30 visually distinct combinations
→ Same theme regenerated = different personality = different structure ✅
```

---

## MODULE 5 — TYPOGRAPHY SCALE (FIXED PX — NEVER rem/vw/clamp)

```
Canvas: 1920×1080. Scaling via scaleToFit(). Fixed px only.

--text-eyebrow:  16px    UPPERCASE, letter-spacing: 3px (Latin) / 0 (Chinese)
--text-h1:      140px    Cover hero only
--text-h2:       80px    Full-width layouts only
--text-h3:       52px    Full-width sub-headlines
--text-body:     28px    Full-width body
--text-small:    20px    Labels, meta
--text-data:    160px    Big metrics
--text-chapter: 220px    Chapter break
--text-ghost:   280px    Background ghost text (opacity 0.04)

LAYOUT OVERRIDES (narrower areas):
  Split pane (46% ~880px):    h2=58px, h3=36px, body=24px, bullets=24px
  Three column (33% ~580px):  h3=34px, body=22px
  2×2 grid (45% ~820px):      h3=32px, body=22px
  Comparison col (48%):       h3=52px, body=24px

CHINESE:
  letter-spacing: 0 on ALL headlines
  word-break: keep-all always
  Never " " at large sizes — use SVG quote paths

WORD LIMITS:
  Cover:     ≤8 word headline, ≤12 subtitle
  Content:   ≤8 headline, max 4 bullets × ≤10 words
  Chapter:   ≤3 words total
  Data:      numbers + ≤15 supporting words
  Quote:     ≤25 words
  Closing:   ≤20 words
```

---

## MODULE 6 — VISUAL BALANCE

```
RULE: No slide may have content occupying less than 70% of slide width.

Full-width layouts     → naturally balanced, add ghost text for depth
Split layouts          → visual pane MUST have: image / color block / large number
Text-only slides       → add ghost text (280px, opacity 0.04) opposite side
Three column           → ALL 3 columns must have content + image
2×2 grid               → ALL 4 cells must have content + image

Before each slide: "Does this use full 1920px width?"
If NO → add visual element to fill empty space.
```

---

## MODULE 7 — SPACING

```
MINIMUM PADDING:
  Cover:              0 160px
  Split content pane: 100px 80px
  Full-width:         80px 120px
  Data:               60px 100px
  Three column:       70px 100px
  Chapter:            centered, no padding

GAPS:
  After eyebrow:    margin-bottom: 20px
  After headline:   margin-bottom: 32px
  After divider:    margin: 28px 0
  Between bullets:  margin-bottom: 20-24px
  Three col gap:    32px
  2×2 grid gap:     24-28px

□ No element within 60px of slide edge
□ Three col: min-width:0 + overflow:hidden + box-sizing:border-box
```

---

## MODULE 8 — IMAGE SYSTEM

**ALWAYS picsum.photos. NEVER source.unsplash.com (deprecated).**

```html
<!-- Full bleed -->
<img src="https://picsum.photos/seed/[KEYWORD]/1920/1080"
     style="width:100%;height:100%;object-fit:cover;display:block;" alt="">

<!-- Split pane -->
<img src="https://picsum.photos/seed/[KEYWORD]/1200/1080"
     style="width:100%;height:100%;object-fit:cover;" alt="">

<!-- Vary seeds per column: [KEYWORD]1, [KEYWORD]2, [KEYWORD]3 -->
<!-- Always add gradient overlay on full-bleed images -->
```

**MANDATORY:**
```
□ Split visual pane: always has picsum image
□ Three column: each column 220px top image
□ 2×2 grid: each cell 160px top image
□ Full bleed: picsum + dark gradient overlay
□ Never leave visual zone empty
```

---

## MODULE 9 — UNIVERSAL DESIGN UPGRADES (ALL themes)

### TWO-TONE HEADLINES (mandatory every content slide)
```html
<!-- Regular white + bold accent-colored key word -->
<h2 class="animate" style="font-size:80px;line-height:1.05;
  letter-spacing:0;word-break:keep-all;">
  How It's Changing<br>
  <strong style="color:var(--color-primary);font-weight:900;">the World</strong>
</h2>
<!-- Rule: color max 40% of headline. 1-3 words only. -->
```

### CONTENT ANCHOR BOX
```html
<div style="background:var(--surface);border:1px solid var(--border,rgba(255,255,255,0.08));
  border-radius:12px;padding:28px 32px;margin-top:24px;">
  <h3 style="font-size:28px;color:var(--color-primary);margin-bottom:12px;">[SUB]</h3>
  <p style="font-size:22px;color:var(--text-secondary);">[CONTENT]</p>
</div>
```

### PAGE WATERMARK (optional)
```html
<div style="position:absolute;bottom:28px;left:60px;font-size:14px;
  color:rgba(255,255,255,0.2);letter-spacing:2px;z-index:20;">
  [BRAND] · [TOPIC]
</div>
```

---

## MODULE 10 — SLIDE CONTENT GUARD

```
Before writing each slide HTML:
□ Has real content (headline + at least one element)
□ Every element has class="animate"
□ data-animation set on <section>
□ Font sizes match layout width
□ Word count within limits
□ Image zone has picsum URL
□ Chinese: letter-spacing:0, word-break:keep-all
□ Full width used (≥70% of 1920px)

Empty slide prevention:
→ Replace with L03 Chapter Break or L06 Big Statement
→ NEVER empty <section class="slide"></section>
```

---

## MODULE 11 — THEME IDENTITY ENFORCEMENT

```
CRITICAL: Same content, different themes must look STRUCTURALLY different.
Not just color-swapped — different shapes, decorations, compositions.

Before generating, read theme's LAYOUT IDENTITY section and apply:
  MANDATORY elements on every slide
  FORBIDDEN elements to avoid

TEST: Cover colors — do two themes still look different?
If NO → you haven't applied LAYOUT IDENTITY correctly.
```
