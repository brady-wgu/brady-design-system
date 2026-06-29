# Design system reference

Load this reference when a document needs the full color palette, extended type scale, spacing rationale, or accessibility verification data.

This file derives directly from three source documents:
- **WGU FY26 Logo & Seal Guide** (Logo & Seal Guide.pdf + PPTX Custom 3 theme): brand anchor authority
- **PDev 2026 Visual Design Standards** (PDev_VisualDesignGuidelines_02_MAR_2026.pdf): color and visual foundations only; course content sections excluded
- **Paragon Design System v20.18.1**: spacing system and token architecture foundations

---

## 1. Color system

### 1.1 WGU FY26 brand anchors (non-negotiable)

Sourced from the WGU FY26 Logo & Seal Guide via PPTX Custom 3 theme color extraction. These values govern all WGU-affiliated documents. Altering them is prohibited.

| Token | Hex | RGB | PPTX theme slot | Role |
|---|---|---|---|---|
| NAVY | #002855 | (0, 40, 85) | accent3 | Primary brand anchor; H1 color, cover blocks, heavy rules |
| MIDNIGHT | #001730 | (0, 23, 48) | dk1, dk2 | Darkest background; cover fill, primary heading color |
| BLUE | #0070F0 | (0, 112, 240) | accent1, hlink, folHlink | Links, eyebrows, NOTE accent, CTAs |
| SKY | #46B1EF | (70, 177, 239) | accent4 | Reversed accent on navy/midnight covers; light accent |

Note on Midnight: PPTX Custom 3 theme defines #001730. PDev 2026 references #001731 (1 unit blue channel difference: imperceptible). #001730 is the FY26 authoritative value.

### 1.2 WGU extended palette (from PPTX Custom 3 theme)

| Token | Hex | PPTX slot | Role |
|---|---|---|---|
| ~~LIME~~ | ~~#97E152~~ | ~~accent2~~ | Removed 29 Jun 2026 — no semantic role; conflicts with BS_YELLOW on covers |
| ICE | #EEF6F9 | lt2 | Light surface background |
| GRAY | #A7A7A7 | accent5 | Neutral mid |
| LGRAY | #F1F1F1 | accent6 | Off-white surface |

### 1.3 Brady semantic palette

Replaced the PDev 2026 accent palette on 29 Jun 2026. Each token carries a distinct semantic role and is visually distinct from the WGU anchor blues.

| Token | Hex | PDev predecessor | Semantic role |
|---|---|---|---|
| BS_YELLOW | #FFCC00 | GOLD #F2B705 | WARNING callout accent; per-document accent variant |
| BS_TAWNY | #D97443 | ORANGE #D06A2E | AT RISK traffic light; QUOTE callout |
| BS_EMBER | #C2592A | RED #C83A3A | CAUTION callout; KPI down-delta |
| BS_MID | #1E2E56 | PURPLE #7968BA / TEAL #2C7C7A | DEFINITION / CODE callout; KPI up-delta |

### 1.4 Editorial neutrals (2030-forward document system)

These are design decisions applied on top of the source palettes to create Brady's distinctive document identity. Paragon's YIQ threshold approach (threshold 128) informed the lightness calibration.

| Token | Hex | Contrast on white | Role |
|---|---|---|---|
| INK | #0A0A0A | 19.77:1 AAA | Body text: near-black, considered rather than default |
| GRAPHITE | #2B2F36 | 13.30:1 AAA | Strong secondary text |
| SLATE | #4B5563 | 7.56:1 AAA | Metadata, deemphasized text |
| ASH | #6B7280 | 4.83:1 AA | Footer, header, eyebrow labels |
| FOG | #9CA3AF | 2.56:1: decorative | Borders, decorative rules |
| MIST | #D1D5DB | Surface | Hairlines, zebra row alternates |
| PAPER | #F8F9FA | Surface | Cool surface background for callouts |
| BONE | #F7F5EF | Surface | Warm surface background |
| WHITE | #FFFFFF |: | Default page background |

### 1.4a Editorial band tint (deeper than callout tints, for table zebra)

`TINT_BAND = #DCE3EE` (~15% navy mixed with white). Used specifically as the alternating row background in `addEditorialTable`. Distinct from `TINT_BLUE` (8% blue, the NOTE callout background) — the table zebra needs more visual presence than callout backgrounds because rows are smaller visual elements than callouts and need clear differentiation from the white alternate. INK on TINT_BAND ≈ 15.3:1 contrast (AAA — see section 5.2).

### 1.5 Callout surface tints (8% brand anchor mixed into white)

Pre-calculated surface tints: WGU brand tints pair with the anchor color as text; Brady palette tints pair with INK (see note on BS_YELLOW).

| Token | Hex | Pairs with | Contrast note |
|---|---|---|---|
| TINT_NAVY | #E6EBF1 | NAVY: KEY INSIGHT callout | NAVY ≈ 8.2:1 AAA |
| TINT_BLUE | #EAF3FE | BLUE: NOTE, BOTTOM LINE | BLUE ≈ 3.9:1 AA large text |
| TINT_YELLOW | #FFFBEB | WARNING callout bg | INK text only — BS_YELLOW fails on this bg |
| TINT_EMBER | #FAF2EE | CAUTION callout bg | BS_EMBER label ≈ 3.7:1 (large text / UI) |
| TINT_TAWNY | #FCF4F0 | AT RISK / QUOTE bg | INK label — BS_TAWNY fails on this bg |
| TINT_MID | #EDEEF1 | DEFINITION / CODE bg | BS_MID text ≈ 10:1 AAA |

### 1.6 Accessibility rules

From PDev 2026 Visual Design Standards section 1.7 and WCAG AA requirements:

- Body text (under 18pt): minimum 4.5:1 contrast
- Large text (18pt bold or 24pt regular): minimum 3:1
- UI components and graphical objects: minimum 3:1
- Color is never the sole method of conveying information
- Never invent colors outside this palette (§1.1–1.4 and §1.9)
- When in doubt, use INK (#0A0A0A) on WHITE (#FFFFFF): always the most reliable choice

**Header text color rules by background (sourced from PDev 2026 section 1.7):**

| Background | Acceptable text |
|---|---|
| NAVY #002855 | White only |
| MIDNIGHT #001730 | White only |
| BLUE #0070F0 | Black or White |
| SKY #46B1EF | Black or White |
| BS_YELLOW #FFCC00 | Black only (use INK or BS_INK) |
| BS_TAWNY #D97443 | Black only (use INK) |
| BS_EMBER #C2592A | Black or White |
| BS_MID #1E2E56 | White only |
| GRAY #A7A7A7 | Black only |

### 1.7 Per-document accent variants

A document may carry ONE sanctioned accent so a portfolio of Brady's documents reads as a designed series with per-document personality while staying unmistakably WGU. Default is none — accent-less output is byte-identical to pre-accent builds. Select by subject domain:

| Accent | Hex | Tint | Domain signal |
|---|---|---|---|
| BS_YELLOW | #FFCC00 | TINT_YELLOW | Design, brand, editorial identity |

`bd.newDocument({ accent: 'BS_YELLOW' })`. The accent appears in exactly four non-semantic, light-background slots, threaded automatically: **stat-row numbers**, **key-findings numerals**, **figure highlight color** (bar/line/timeline/quadrant `accent` option), and the **cover art third color**. Hard prohibitions: never in headings, covers (text), callout variant colors, table headers, or KPI deltas (BS_MID/BS_EMBER are semantic there); never on dark bands — BS_YELLOW fails contrast against NAVY/MIDNIGHT, which is why sectionDivider numerals stay SKY. BS_YELLOW does double-duty as the WARNING callout accent and the per-document art-direction variant; these contexts are visually distinct (accent bar vs. stat numbers) and do not conflict. Max four accent appearances per document; NAVY/BLUE remain dominant. The set lives in `brady-tokens.js ACCENTS`; anything else throws at `newDocument`.

### 1.8 Cover art motif (deterministic generative)

`addCover({ art: true })` renders a flat-geometric motif band inside the Midnight cover panel via `bf.coverArt`. The art law is encoded as constants in the builder, not per-document judgment calls:

- **Seeded by the document title** (FNV-1a → mulberry32): the same title regenerates identical art on every rebuild; different documents get visibly different compositions. A shelf of Brady's documents becomes a distinct yet coherent series.
- **Three fixed motif families** (seed-selected): sparse tile grid (quarter-circles, circles, rounded squares at ~30% occupancy), concentric arc field, diagonal bar rhythm.
- **Colors:** MIDNIGHT ground; fills from NAVY / BLUE / SKY plus one third color (the document accent when set; no fallback third color since LIME was removed); max 3 fills per composition; flat 100% or the single fixed 0.55 tint.
- **Never:** strokes, gradients, textures, text, photography. Decorative only — no FIGURE number, no meaning conveyed, so text-contrast rules do not apply.
- The identical inline SVG goes into the Markdown twin under the title, preserving the dual-output reconstruction guarantee.

### 1.9 Brady editorial palette

Added 29 Jun 2026 as Brady's primary editorial identity for non-WGU-branded outputs (HTML artifacts, personal work products, web demos). In WGU-branded DOCX outputs, WGU brand anchors always take precedence; these tokens layer in where no WGU color occupies the same semantic role. None conflict with official WGU FY26 corporate colors.

| Token | Hex | WCAG vs BS_CREAM | Role |
|---|---|---|---|
| BS_CREAM | #F7F4EF | — | Warm page background; 1-unit from BONE (#F7F5EF) — interchangeable |
| BS_GREEN | #1A6635 | ~7.2:1 AAA | ON TRACK traffic light; KPI up-delta; positive/success signal |
| BS_YELLOW | #FFCC00 | ~9.5:1 AAA (on MIDNIGHT) | WARNING callout accent; per-document accent variant; stat numbers, figure highlights, cover art |
| BS_TAWNY | #D97443 | ~5.2:1 AA | AT RISK traffic light; QUOTE callout; warm surface/background |
| BS_EMBER | #C2592A | ~3.8:1 UI/large-text | CAUTION callout (do-not-proceed); KPI down-delta (large text/UI only) |
| BS_RED | #BF1E2E | ~6.1:1 AA | ALERT callout (already failing); CRITICAL traffic light; severe signal |
| BS_MID | #1E2E56 | ~13:1 AAA | DEFINITION/CODE callout; KPI up-delta; mid dark blue for HTML/web |
| TINT_GREEN | #EDF3EF | — | ON TRACK / success background; BS_GREEN text ≈ 7.2:1 AAA |
| TINT_RED | #FAEDEE | — | ALERT / CRITICAL background; BS_RED text ≈ 5.4:1 AA |
| TINT_YELLOW | #FFFBEB | — | WARNING background; INK text only (BS_YELLOW fails on this bg) |
| TINT_EMBER | #FAF2EE | — | CAUTION background; BS_EMBER label ≈ 3.7:1 |
| TINT_TAWNY | #FCF4F0 | — | AT RISK/QUOTE background; INK label (BS_TAWNY fails on this bg) |
| TINT_MID | #EDEEF1 | — | DEFINITION/CODE background; BS_MID label ≈ 10:1 AAA |

**Design authority:** WGU brand anchors (§1.1) always outrank BS tokens in branded DOCX. These are the primary palette for all outputs.

---

## 2. Typography

### 2.1 Font family

Sourced from WGU FY26 PPTX Custom 3 theme XML. All three fonts are open source under the SIL Open Font License by Steve Matteson for Microsoft. Official download: [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=106087).

All three are M365 cloud fonts — they download automatically on first document open and cache permanently. An active M365 subscription and internet access are required on first open. For web use outside M365, self-host after converting TTF to WOFF2; see `assets/fonts/README.md`.

| Role | Font | PPTX theme slot |
|---|---|---|
| Display and headings | **Aptos Display** | Major (heading) font |
| Body text | **Aptos** | Minor (body) font: Word default since 2024 |
| Code and data | **Aptos Mono** | M365 cloud font companion |

Verdana Pro (specified in PDev v1 for course content) is not used for Brady's workplace documents.

### 2.2 Type scale

Proportional relationships derived from Paragon's heading scale (h1: 2.5rem, h2: 2rem, h3: 1.375rem relative to 1.125rem base), adapted to Word's point system for document readability.

| Element | Font | Size | Weight | Leading (exact) | Tracking |
|---|---|---|---|---|---|
| Cover title | Aptos Display | 42pt | Bold | 44pt | -0.2pt |
| Stat number | Aptos Display | 36pt | Bold | 38pt | -0.2pt |
| H1 | Aptos Display | 28pt | Bold | 30pt | -0.2pt |
| H2 | Aptos Display | 18pt | Bold | 22pt | 0 |
| H3 | Aptos Display | 13.5pt | Regular | 17pt | 0 |
| H4 run-in | Aptos | 11pt | SemiBold | 14.3pt | 0 |
| Body | Aptos | 11pt | Regular | 14.3pt exact | 0 |
| Body emphasis | Aptos | 11pt | SemiBold | 14.3pt | 0 |
| Eyebrow | Aptos | 8.5pt | SemiBold ALL CAPS | 10pt | +0.8pt |
| Caption | Aptos | 9pt | Regular | 11pt | 0 |
| Table header | Aptos | 9pt | SemiBold ALL CAPS | 12pt | +0.3pt |
| Table body | Aptos | **11pt** | Regular (Bold on first column) | 13pt | 0 |
| Mono / data | Aptos Mono | 9.5pt | Regular | 13pt | 0 |
| Header/footer | Aptos / Aptos Mono | 7.5pt | Regular |: | +0.5pt |

Use `lineRule: 'exact'` with explicit point values. Never use line height multipliers: they drift across apps. Exact leading renders identically in M365 Word, Google Docs, and LibreOffice.

### 2.2a Heading style registration

Headings are rendered with `heading: HeadingLevel.HEADING_[1-4]` on the Paragraph object, AND the Document's `styles.paragraphStyles` array defines custom Heading 1/2/3/4 styles that match Brady's specs (Aptos Display, Midnight/Navy/Graphite, exact leading). The result:

- Word's Navigation Pane and document outline see the paragraphs as proper headings
- The visual rendering matches the type scale in section 2.2 above
- Tools that rely on Word's semantic style IDs (accessibility tools, table-of-contents generators, content-summary integrations) work correctly

Without semantic Heading style IDs, Word's Navigation Pane is empty and tools cannot infer document structure. This was a regression in earlier versions of the skill.

### 2.3 Fallback behavior

When a recipient opens the document without M365:
- Aptos and Aptos Mono ship with Windows 11 (separate from M365 cloud fonts)
- Bierstadt Display requires M365 subscription: this is why Aptos Display was chosen instead
- For external distribution with typography fidelity: embed fonts (File → Options → Save → "Embed fonts") or distribute as PDF

---

## 3. Spacing system

### 3.1 Paragon 4px grid → Word points

Sourced from Paragon Design System v20.18.1 section 4. The 4px base unit (1rem = 16px, spacer-1 = 0.25rem = 4px) translates to 3pt in Word (4px × 72pt/96px = 3pt). All spacing values derive from this grid.

| Paragon spacer | Pixel value | Word equivalent | Use |
|---|---|---|---|
| spacer-1 | 4px | 3pt | Tight sub-element gaps |
| spacer-2 | 8px | 6pt | Body paragraph spacing-after |
| spacer-2.5 | 12px | 9pt | List item spacing |
| spacer-3 | 16px | 12pt | Table/caption spacing |
| spacer-3.5 | 20px | 15pt | (unused; reserved) |
| spacer-4 | 24px | 18pt | H1 and H2 spacing-before; Paragon grid gutter |
| spacer-4.5 | 32px | 24pt | (no heading use after H1 was harmonized to H2 in v1.3) |
| spacer-5 | 48px | 36pt | Section-level spacing |

### 3.2 Paragraph spacing applied

| Element | Before | After |
|---|---|---|
| Body | 0 | 6pt |
| List item | 0 | 3pt |
| H1 | 18pt | 6pt |
| H2 | 18pt | 6pt |
| H3 | 12pt | 4pt |
| Caption | 6pt | 4pt |
| Table (above) | 12pt | 6pt |

Widow/orphan control enabled globally. Keep-with-next on every heading. Never use blank paragraphs to push content; use spacing-before/after.

### 3.3 Margins by profile (US Letter)

Content width is the page width minus left/right margins, so it is **profile-dependent**. `brady-doc.js` computes it per document as `this.cw`, and every full-width band component spans it (not a fixed 9792). When sizing `addEditorialTable` columns by hand, sum them to the active profile's content width.

| Profile | Orientation | Top | Bottom | Left | Right | Content width (`this.cw`) |
|---|---|---|---|---|---|---|
| Memo | Portrait | 0.6" | 0.6" | 0.7" | 0.7" | 10224 DXA |
| Brief | Portrait | 0.75" | 0.8" | 0.85" | 0.85" | 9792 DXA |
| Assessment | Portrait | 0.75" | 0.8" | 0.85" | 0.85" | 9792 DXA |
| Differential | **Landscape** | 0.4" | 0.5" | 0.5" | 0.5" | **14400 DXA** |

The exported `CONTENT_WIDTH` constant (9792) is the brief/assessment default only; don't size differential or memo tables to it.

### 3.4 Corner radius

Sourced from PDev 2026 section 5.1: "Use rounded corners on shapes whenever possible; keep consistent within a graphic."

Paragon base radius: 0.375rem = 6px ≈ 4.5pt. Applied to callout blocks, stat rows, and figure bounding regions.

---

## 4. Component design principles

### 4.1 Editorial tables

Horizontal rules only: no cell borders. This is the strongest visual differentiator from default Word tables.

- 1.5pt Navy top rule
- 0.75pt Navy header separator
- Hairline Mist row dividers
- 1pt Navy bottom rule
- Zebra rows: alternating WHITE and **TINT_BAND (#DCE3EE)**. The deeper blue-tint produces a clearly visible band without overpowering body text. INK on TINT_BAND ≈ 15.3:1 (AAA — see section 5.2). This replaces the earlier subtler TINT_BLUE zebra.
- Emphasized column (`emphasizeCol: N`): header gets NAVY background with white text; body cells get ICE background with bold NAVY text.
- Per-cell color: any cell can override the zebra with `{ text, color, bg, bold, mono }`. Used for traffic-light status columns:

| Status | text color | bg | use |
|---|---|---|---|
| BS_MID   | #1E2E56 | TINT_MID #EDEEF1   | ON TRACK / ALIGNED / SUCCESS |
| BS_TAWNY | #D97443 | TINT_TAWNY #FCF4F0 | AT RISK / PARTIAL / WARNING  |
| BS_EMBER | #C2592A | TINT_EMBER #FAF2EE | BLOCKED / CONFLICT / FAIL    |

- Row protection: every data row carries `<w:cantSplit/>` so individual rows never split across pages. Small tables (≤ 8 data rows) auto-apply `keepNext` to inner paragraphs to keep the whole table together.

### 4.1a Section band (deprecated, opt-in only)

Navy-filled horizontal band, full content width. **Not used in default outputs** — the canonical section opener is plain `addH1` per SKILL.md's section-opener rule. Retained for rare special-page treatment; the numbered `addSectionDivider` is the sanctioned band for long-assessment rhythm.

- Background: NAVY #002855
- Cell padding: 120 DXA top/bottom, 360 DXA left/right
- Eyebrow line: Aptos 8.5pt SemiBold ALL CAPS, SKY #46B1EF, +0.8pt tracked
- Title line: Aptos Display 18pt Bold, WHITE
- Semantically Heading 1 (populates Word's Navigation Pane)

### 4.1b Blockquote

Pulled-emphasis paragraph for inline quotes or short emphasis statements.

- 3pt BLUE #0070F0 left border
- 360 DXA left indent
- Aptos italic 11pt SLATE #4B5563
- 240 DXA spacing before and after

For attributed quotes with a label, use the QUOTE callout variant instead.

### 4.1c Cover summary band

Two-column band rendered directly after the cover block on the title page, to eliminate the half-empty page-1 problem.

- Left column (40% width): "AT A GLANCE" eyebrow + 2-3 sentence synopsis, PAPER background
- Right column (60% width): "KEY FINDINGS" eyebrow + 3 numbered items, WHITE background
- Cell padding: 480 DXA all sides
- Each finding: zero-padded digit (01, 02, 03) in 16pt Aptos Display Bold BLUE, finding text in 11pt Aptos INK

### 4.2 Callout system

Six variants plus two composites. Single-cell table implementation: 4pt left accent bar, 8% tint background, eyebrow label in tracked caps, body beneath.

| Variant | Accent | Background | Use |
|---|---|---|---|
| NOTE | BLUE | TINT_BLUE | Context worth noticing |
| KEY INSIGHT | NAVY | TINT_NAVY | Primary finding |
| WARNING | BS_YELLOW | TINT_YELLOW | Risks, caveats (label: BS_EMBER — BS_YELLOW fails as text) |
| CAUTION | BS_EMBER | TINT_EMBER | Do-not-proceed |
| DEFINITION | BS_MID | TINT_MID | Term introduction |
| QUOTE | BS_TAWNY | TINT_TAWNY | Attributed excerpts (label: INK) |
| BOTTOM LINE | BLUE | PAPER | TL;DR summary |
| CODE | BS_MID | TINT_MID | Monospace code block |

### 4.3 Visual design principles (from PDev 2026 sections 4, 5, 8)

**Layout:** Maintain consistent outer margins. One main idea per visual unit. Left-align body text by default; center only for short labels or cover elements.

**Strokes:** One primary line weight per visual. Lines should feel clean and even, not heavy or decorative. Stroke weight supports content, not decoration.

**Corner treatment:** Rounded corners on shapes: soft and intentional, not pill-shaped. Consistent within a single figure.

**Color restraint (PDev 2026 section 1.8):** Most figures work best with three colors total. A single warm accent adds emphasis when balanced with cooler tones. Too many strong colors compete for attention. Always anchor with one of the brand blues.

**Quality test (adapted from PDev 2026 section 12.8):** Before finalizing any document, ask: Does this look professional and credible? Is the main message clear within 5 seconds? Can all text be read without strain? Is contrast sufficient? Does it still make sense at reduced size? If any check fails, revise.

---

## 5. Accessibility

### 5.1 WCAG Level AA minimums

| Context | Minimum ratio |
|---|---|
| Normal text (under 18pt) | 4.5:1 |
| Large text (18pt bold or 24pt regular) | 3:1 |
| UI components and graphical objects | 3:1 |

### 5.2 Verified pairings

| Text | Background | Ratio | Status |
|---|---|---|---|
| WHITE #FFFFFF | MIDNIGHT #001730 | 18.1:1 | AAA |
| WHITE #FFFFFF | NAVY #002855 | 11.3:1 | AAA |
| INK #0A0A0A | WHITE #FFFFFF | 19.77:1 | AAA |
| INK #0A0A0A | ICE #EEF6F9 | 18.4:1 | AAA |
| SLATE #4B5563 | WHITE #FFFFFF | 7.56:1 | AAA |
| ASH #6B7280 | WHITE #FFFFFF | 4.83:1 | AA |
| NAVY #002855 | TINT_NAVY #E6EBF1 | 8.2:1 | AAA |
| BLUE #0070F0 | TINT_BLUE #EAF3FE | 4.9:1 | AA |
| INK #0A0A0A | TINT_BAND #DCE3EE | 15.3:1 | AAA (table zebra alternate) |
| FOG #9CA3AF | WHITE #FFFFFF | 2.56:1 | Decorative only |
| BS_CREAM #F7F4EF | MIDNIGHT #001730 | ~18:1 | AAA (demo-verified) |
| BS_YELLOW #FFCC00 | MIDNIGHT #001730 | ~9.5:1 | AAA (demo-verified) |
| BS_INK #0A1128 | BS_TAWNY #D97443 | ~5.2:1 | AA (demo-verified) |
| BS_EMBER #C2592A | BS_CREAM #F7F4EF | ~3.8:1 | UI/large text only (demo-verified) |
| BS_GREEN #1A6635 | WHITE #FFFFFF | ~7.2:1 | AAA (demo-verified) |
| BS_RED #BF1E2E | WHITE #FFFFFF | ~6.1:1 | AA (demo-verified) |

---

## 6. Cross-reference

- Full API: `assets/build-scripts/brady-doc.js` header comment
- Figure builders: `assets/build-scripts/brady-figures.js` header comment
- Writing voice and citations: `references/writing-style.md`
- Figure detection pipeline: `references/figure-builder.md`
- Font installation: `assets/fonts/README.md`
