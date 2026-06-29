---
name: brady-document-style
description: Apply Brady Redfearn's WGU document standards whenever Claude creates, edits, or polishes any deliverable for Brady. Primary scope: workplace memos, status reports, specification documents, and research reports as DOCX and Markdown. Also trigger on 'make this look professional,' 'apply my style,' 'format this,' 'clean this up,' 'polish this,' or anything implying document quality or brand consistency. Enforces WGU FY26 branding, Aptos typography, Brady's voice (no em dashes, no AI clichés, DD MMM YYYY dates), WCAG AA, and semantic Word headings. One JSON spec compiles both outputs, automated QC validates both, and a render preview checks layout visually. Auto-builds matched figures and leadership components (charts, quadrant, KPI dashboard, timeline, pull-quote, TOC, seeded cover art) in a clean editorial flat aesthetic. Default outputs are DOCX and Markdown; any other format requires Brady's explicit request. Always apply this skill for any Brady deliverable even without an explicit formatting request.
---

# Brady's Document Style

Brady Redfearn is a Senior Technology Strategist in WGU's Program Development (PDev) organization, Salt Lake City, UT. This skill enforces the visual system, writing voice, and structural patterns his team and audience expect in 2026 and beyond.

Output is **always DOCX and Markdown** unless Brady explicitly requests otherwise. The look is edge-to-edge, typographically disciplined, anchored on WGU brand colors with an editorial neutral palette. Reference points: Vercel docs, Linear changelog, Figma blog, high-end consultancy reports.

**Design authority**, in priority order: (1) WGU FY26 Logo & Seal Guide — brand anchors and Aptos fonts are non-negotiable; (2) Barn Swallow editorial palette (`design-system.md §1.3, §1.9`) — semantic colors, callouts, and accent variant; (3) PDev 2026 Visual Design Standards — layout, quality standards, and accessibility guidance (accent palette retired 29 Jun 2026); (4) Paragon Design System v20.18.1 — spacing (4px grid → 3pt). Full tokens, type scale, spacing, and accessibility pairings live in `references/design-system.md`.

## What to load, by profile

Load only what the job needs. This is the biggest lever on speed and token cost: a one-page memo does not need the figure pass or the heavy structural rewrite, so don't load them. Read a reference file the moment the table says "yes," not before.

| Profile | Conciseness | Figures (`figure-builder.md`) | `writing-style.md` | `design-system.md` |
|---|---|---|---|---|
| **Memo** | Light pass (inline below) | skip — 0 figures | skip | skip |
| **Status** | Light pass (inline below) | skip — KPI rule is in the template | skip | skip |
| **Brief** | `conciseness-pass.md` (heavy) | load | skip | on demand |
| **Assessment** | `conciseness-pass.md` (heavy) | load | load | on demand |
| **Differential** | `conciseness-pass.md` (heavy) | load | skip | on demand |

"On demand" = load `design-system.md` when you need the full color list, the complete type scale, spacing rationale, or to verify a contrast pairing. The brand-anchor quick reference below covers the common case.

Always load `assets/build-scripts/brady-doc.js` for any DOCX. Load `brady-figures.js` only when the profile builds figures.

## Standard processing pipeline

Run in order:

1. **Conciseness pass.** Memos and any input under ~200 words get the **light pass** (below) — sentence-level only. Briefs, assessments, and differentials get the **heavy pass** in `references/conciseness-pass.md` (structural → paragraph → sentence). The reason for the split: short, already-tight content gains nothing from structural moves and loading the heavy reference wastes tokens; long drafts genuinely need restructuring for density.
2. **Figure detection and build** (skip for memos). Scan the tightened content with the heuristics in `references/figure-builder.md`; build matched figures with `brady-figures.js`.
3. **Compile both outputs from one spec.** Author the document ONCE as a JSON spec and run `node assets/build-scripts/build-doc.js spec.json` — it emits the `.docx` and the `.md` twin from a single walk (figures render as PNG at 2x in DOCX and inline SVG in MD so a future session can reconstruct state), computes the `slug_vN-N_DDMMMYYYY` filename from the spec's meta, validates the spec with pointed errors before building, and runs QC on both files automatically. Authoring once instead of twice is the skill's biggest token saving and makes DOCX/MD parity structural rather than disciplined. Direct `brady-doc.js` scripting remains the escape hatch for layouts the spec cannot express — use it for custom one-offs only.
4. **Visual check (long-form profiles).** qc.js sees only XML; what makes a document look amateur is only visible in a render. For briefs, assessments, and differentials run `node assets/build-scripts/preview.js out.docx` and Read the contact sheet (or the `.preview.pdf` it falls back to). Inspect adversarially — assume there are problems: page 1 full (cover + summary band), figures at intended width with all labels readable, zebra striping visible, captions on one line, no ragged gutters. Fix and re-render once. Memos skip this step. If no renderer exists, preview.js says so and QC alone gates delivery.

Both files go to `/mnt/user-data/outputs/` when that sandbox path exists (claude.ai); in the desktop app or Claude Code, save to the session's working directory or wherever Brady asked. "DOCX only" / "skip the MD" suppresses one file; QC still runs.

### Light conciseness pass (memos and short content, inline)

For one-pagers and inputs under ~200 words, the heavy reference is overkill. Apply only these sentence-level moves:

- Strip hedge stems: "it is worth noting that", "one important consideration is" → lead with the noun.
- Replace verb phrases with verbs: "make a determination of" → "determine"; "is responsible for managing" → "manages".
- Cut adjective stacks and obvious qualifiers: "users of the platform" → "users".
- Apply the writing-style quick card below (banned phrases, em-dash removal, date format, number rules).
- Do **not** restructure, merge, or cut sections. Short content is already at the structural level Brady wants.

Surface it in chat as a one-liner: `Conciseness pass: sentence-level (light).`

## Output filename

`slug_vN-N_DDMMMYYYY.ext` — always.

- **Slug:** lowercase kebab-case from the title, articles/stop words dropped, 4–6 meaningful words.
- **Version:** document version, hyphenated, `v`-prefixed (`v1-0`, `v1-3`). Use the metadata version, not a placeholder.
- **Date:** export date `DDMMMYYYY`, month in ALL CAPS (`21MAY2026`). The save date, not the internal date field.

Example: `competency-api-integration-spec_v1-0_21MAY2026.docx` / `.md`.

## Recommended model

Use `claude-sonnet-4-6` for standard documents (≤ ~30K tokens). Use `claude-opus-4-7` for very long documents (> 50K tokens) where long-context tracking matters on the conciseness pass. Never Haiku — insufficient for heavy rewrites and figure detection. Avoid model strings with the `20250514` suffix; they retire 15 Jun 2026.

## Document profiles

| Signal | Primary type | Profile | Template |
|---|---|---|---|
| Short internal communication, 1 page | Workplace memo | **Memo** | `assets/templates/memo.md` |
| Recurring 1-page Progress/Plans/Problems pulse to leadership | Status report | **Status** (builds as `memo`) | `assets/templates/status.md` |
| 1–2 pages, argument-driven, executive audience | Spec doc (short) | **Brief** | `assets/templates/brief.md` |
| Long, cited, multi-section analysis or report | Report / spec (long) | **Assessment** | `assets/templates/assessment.md` |
| Multi-system comparison, tables-first | Report (comparative) | **Differential** | `assets/templates/differential.md` |

If the request is ambiguous between profiles, ask these four in ONE message — a mis-profiled document wastes the heavy references, the figure pass, and a full build cycle, so one ~50-token round is cheap insurance:

- **What decision or action does the reader take from this?** Awareness only → memo/status; a yes/no decision → brief; a deep evaluation → assessment; choosing between systems → differential.
- **Primary reader: executive or technical?** Sets the elaboration budget and how hard to lean on the leadership components.
- **Drafting new or polishing existing?** Polish-only triggers the lighter conciseness override (`conciseness-pass.md` section 4).
- **Expected length?** Do not default to a long format when a memo fits.

## Document generation (spec-first)

**Default path: one JSON spec → `node assets/build-scripts/build-doc.js spec.json`.** The compiler emits the DOCX and the Markdown twin from a single walk, validates the spec first (unknown op, column widths that don't sum to the profile width, missing captions, "Final" status, bad dates — all pointed errors), names the files, and runs QC on both. Block ops map 1:1 to `brady-doc.js` components; the full op list is in the build-doc.js header. Compact spec:

```json
{
  "meta": { "title": "Competency API caching assessment", "version": "1.0 (Draft)",
            "date": "10 Jun 2026", "profile": "assessment",
            "eyebrow": "WGU · TECHNOLOGY STRATEGY · ASSESSMENT",
            "subtitle": "Platform Systems · Program Development",
            "art": true, "accent": "TEAL" },
  "summary": { "synopsis": "Evaluates Redis caching ahead of the Q3 peak.",
               "findings": ["Lookup exceeds SLA by 340ms", "Cache cuts p95 to 38ms", "Six endpoints to migrate"] },
  "toc": true,
  "blocks": [
    { "op": "h1", "text": "1. Problem" },
    { "op": "body", "text": "At 500 concurrent sessions, latency exceeds the SLA by 340ms [1]." },
    { "op": "callout", "label": "KEY INSIGHT", "body": "Batch support is a launch blocker.", "variant": "insight" },
    { "op": "statRow", "stats": [{ "number": "340ms", "label": "SLA overage" }, { "number": "6", "label": "endpoints" }] },
    { "op": "figure", "builder": "quadrant", "caption": "Option positioning",
      "args": { "xAxis": ["Low effort", "High effort"], "yAxis": ["Low impact", "High impact"],
                "items": [{ "label": "Redis cache", "x": 0.25, "y": 0.9 }], "highlightLabel": "Redis cache" } },
    { "op": "closingBand", "statement": "Approve the cache rollout before the Q3 peak.",
      "actions": ["Approve shortlist by 15 Jul 2026"], "contact": "Brady Redfearn · Program Development" },
    { "op": "sources", "entries": ["WGU Platform Systems, load test report, May 2026."] }
  ],
  "control": [{ "version": "1.0", "date": "10 Jun 2026", "status": "Draft", "notes": "Initial draft" }]
}
```

**Escape hatch:** for layouts the spec cannot express, script `assets/build-scripts/brady-doc.js` directly (its header documents every method) — and keep the MD twin in sync by hand. Never hand-build from raw docx-js primitives either way.

### Section opener rule

`addH1(text)` alone is the canonical opener for every major section — no preceding eyebrow, no navy band. The dark-blue Aptos Display heading is the section label. Do not call `addEyebrow(...)` or `addSectionBand(...)` in default outputs; both remain for rare special-page use only. The eyebrow inside `addCover({ eyebrow: ... })` is part of the cover graphic and is unaffected.

`addH1`–`addH4` apply Word's semantic Heading style IDs, so View → Navigation Pane shows the outline and lets readers jump between sections. The custom styles keep the Aptos Display / brand-color rendering while the underlying IDs stay `Heading1`–`Heading4` for tooling.

### Figures and images

Figures come from `brady-figures.js` via `doc.addFigure()` / `doc.addFigures([...])` and render in the clean editorial flat aesthetic. `addFigure()` also accepts raw PNG/JPG paths for photos, headshots, logos, and screenshots — pass no `caption` for decorative images so they don't consume a FIGURE number. When a source `.md` references `![alt](path)`, render it as `doc.addFigure(path, { caption: alt })`. Full detection, captioning, and custom-figure rules are in `references/figure-builder.md` (load only when the profile builds figures). Call `doc.resetFigureCount()` to restart numbering for an appendix.

**SkillProof User Profile Catalog** (recurring): embed each persona's headshot from Brady's OneDrive at the top of its section, left-aligned at `widthIn: 1.75`, no caption (the heading names the persona). Path: `C:\Users\brady.redfearn\OneDrive - Western Governors University\Documents\_SkillProof\c_Deliverables\_From Brady to Vendor\b_User Profiles\` — files `Headshot 01 - Sally the Student.png`, `02 - Charlie the Instructor`, `03 - Alice the Tenant Admin`, `04 - Bob the Super Admin`.

### Document Control brevity

Notes column entries are labels, not sentences: ≤ 8 words. "Initial draft", "Added vendor comparison section", "Renumbered after edit". It is a change log, not a release note.

### Leadership-impact components

Beyond the body helpers, `brady-doc.js` provides high-impact page-level components for executive audiences. Use them where they earn their place — see `references/figure-builder.md` section 8 for trigger heuristics and budget:

- `addKpiDashboard(tiles)` — framed band of 3–4 large stat tiles with ▲/▼ delta indicators (GREEN up / RED down). For a metrics summary or scorecard.
- `addTimeline({ milestones })` — horizontal roadmap track with phase markers and DD MMM YYYY dates. For project/initiative plans.
- `addPullQuote(text, { attribution })` — full-width navy band, large Aptos Display. For one executive-framing line, once per document at most.
- `addSectionDivider(number, label)` — numbered full-width band ("01 · FINDINGS") for visual rhythm in long assessments.
- `addFigureWithCallout(figure, { caption, calloutLabel, calloutBody, variant })` — places a figure beside a Key Insight callout in a two-column layout.
- `addClosingBand({ statement, actions, contact })` — the dark bookend that mirrors the cover, so the recommendation is the last, most visually weighted thing leadership sees. At most one; before Document Control; never adjacent to another dark band.
- `addTableOfContents()` — hyperlinked, dot-leadered Contents in house type. 10+ page assessments and differentials, on its own page after the meta strip. Word populates it on open; Google Docs/LibreOffice may show it stale.
- `bf.quadrant({ xAxis, yAxis, items, highlightLabel })` — the executive 2×2 (effort/impact, risk/value). Positions must be stated in the content; the quadrant holding the highlighted item gets the emphasis tint.

**Per-document art direction** (both default-off; design-system.md §1.7–1.9): `newDocument({ accent: 'TEAL'|'PURPLE'|'GREEN'|'GOLD'|'SWALLOW' })` threads one domain-matched accent through stat numbers, key-findings numerals, figure highlights, and the cover art's third color — never dark bands or semantic colors, so a portfolio of documents reads as a designed series. `addCover({ art: true })` adds a deterministic flat-geometric motif seeded by the title: the same document always regenerates identical art, and every document's page 1 is distinct yet unmistakably WGU.

## Writing style quick card

Applied on every invocation. More detail in `references/writing-style.md` (assessments and cited prose).

**No em dashes (—).** They read as machine-generated and Brady's audience notices. Replace with a comma, colon, semicolon, parentheses, or a period.

**Banned phrases** (they signal filler or AI boilerplate): delve, dive into, leverage (verb), utilize, ensure, streamline, empower, enable (vague), robust, seamless, cutting-edge, state-of-the-art, game-changing, transformative, innovative (generic), holistic approach, comprehensive solution, in conclusion, to summarize, to recap, at the end of the day, moving forward, going forward, it's worth noting, it is important to note, navigate (metaphor), unlock (metaphor).

| Instead of | Write |
|---|---|
| leverage / utilize | use |
| ensure | confirm, verify, or restructure |
| streamline / robust / seamless | name the specific improvement or property |
| in conclusion / to summarize | omit |
| it is important to note | omit; lead with the point |

**Dates: DD MMM YYYY** (27 Mar 2026). Never MM/DD/YYYY, YYYY-MM-DD, or "March 27, 2026" in headers, tables, or metadata — the fixed format keeps tables and metadata scannable and unambiguous across locales.

**Numbers:** spell out one through nine, numerals for 10+. Always numerals in tables, headers, specs, version strings, and before units.

**Voice:** a 16-year strategist holding a PhD in Systems Engineering, MS in UX Research, BS in IT & Cybersecurity, AA in English. Direct, precise, evidence-based. No filler, no hedging on facts; hedge only on forward-looking recommendations. Name negative findings plainly in the first sentence (quantified gap, no softeners), then the mitigation with owner and date (`writing-style.md` §4.4).

**Status values:** Draft / In Review / Superseded. Never "Final" — no WGU document is permanently finalized, and the QC check fails when "Final" appears as a standalone status value or in a Status cell.

## Brand anchors (quick reference)

Non-negotiable WGU FY26 anchors. Full palette, neutrals, accents, tints, and the complete type/spacing scale are in `references/design-system.md`.

| Token | Hex | Use |
|---|---|---|
| NAVY | #002855 | H1, cover, heavy rules, KEY INSIGHT accent |
| MIDNIGHT | #001730 | Cover fill, H1 color |
| BLUE | #0070F0 | Links, eyebrows, NOTE accent, arrows |
| SKY | #46B1EF | Reversed accent on Midnight cover |
| INK | #0A0A0A | Body text (19.77:1 on white) |

**Barn Swallow editorial palette (primary for HTML/web outputs and HTML artifacts; full reference: `references/design-system.md §1.9`):**

| Token | Hex | Use |
|---|---|---|
| BS_CREAM | #F7F4EF | Warm page background (aliases BONE) |
| BS_GREEN | #1A6635 | ON TRACK traffic light; KPI ▲ delta; positive signal (7.2:1 AAA on white) |
| SWALLOW | #FFCC00 | 5th accent variant: stat numbers, figure highlights, cover art; NOT a WARNING signal |
| BS_TAWNY | #D97443 | Warm surface/background and large-text only (AA, 5.2:1 on BS_INK) |
| BS_EMBER | #C2592A | CAUTION callout; KPI ▼ delta; large text/UI only (3.8:1) |
| BS_RED | #BF1E2E | ALERT callout; CRITICAL traffic light; severe/failing signal (6.1:1 AA) |
| BS_MID | #1E2E56 | Mid dark blue for HTML/web gradient contexts |

Fonts: **Aptos Display** (headings), **Aptos** (body), **Aptos Mono** (data) — all open source (SIL OFL), ship with M365 and Windows 11. For **any HTML output** (artifact, canvas, web snippet), load from the CDN — one line, works everywhere: `<link rel="stylesheet" href="https://brady-wgu.github.io/brady-design-system/fonts/aptos.css">`. CSS variables: `font-family: 'Aptos Display', Arial, sans-serif` (headings), `font-family: 'Aptos', Arial, sans-serif` (body), `font-family: 'Aptos Mono', monospace` (code). Key sizes: cover 42pt, H1 28pt, H2 18pt, body 11pt, caption 9pt; use exact leading (`lineRule: 'exact'`), never multipliers. Content width is **profile-dependent** (page width minus L/R margins): brief/assessment 9792 DXA, memo 10224, and the **landscape differential 14400**. `brady-doc.js` computes it as `this.cw` and every full-width band component spans it automatically. When you size `addEditorialTable` columns yourself, sum them to the active profile's content width (9792 for a brief/assessment, **14400 for a differential**), not to the exported `CONTENT_WIDTH` constant (which is only the brief/assessment default). Contrast: normal text ≥ 4.5:1, large text ≥ 3:1, color never the sole signal, never invent colors outside the palette.

Callout variants (single-cell, 4pt left accent bar, 8% tint): **KEY INSIGHT** (NAVY), NOTE (BLUE), BOTTOM LINE (BLUE/PAPER), WARNING (SWALLOW), CAUTION (BS_EMBER — do not proceed), **ALERT** (BS_RED — already failing/severe), QUOTE (BS_TAWNY), DEFINITION / CODE (BS_MID).

## Markdown implementation

- `#` document title (one per file), `##` major section, `###` subsection, `####` group label (sparingly). Don't skip levels.
- `-` for unordered lists; fenced code blocks with language specified.
- Figures use **inline SVG** from `fig.svg`, with a `**FIGURE 01**  Caption` line above and a blank line on each side.

**Markdown → DOCX heading mapping** (not one-to-one — the title sits at cover level, body starts one level deeper):

| Markdown | DOCX method | Word style |
|---|---|---|
| `# Document title` | `addCover({ title })` | (cover only) |
| `## Section` | `addH1(text)` | Heading 1 |
| `### Subsection` | `addH2(text)` | Heading 2 |
| `#### Sub-subsection` | `addH3(text)` | Heading 3 |

Preserve section numbering in the heading text (`## 1. Executive summary` → `addH1('1. Executive summary')`). Mapping `## Section` to `addH2` would leave the document with no Heading 1, breaking the Navigation Pane and the dark-blue Aptos Display treatment on major sections.

## Pre-delivery QC check

QC is **automated** and `build-doc.js` runs it on both outputs for you. `brady-doc.js` applies preventive formatting during construction (keep-with-next on headings and captions, repeated header rows, widow/orphan control, edge-to-edge widths). To run it directly (e.g. after an escape-hatch build):

```bash
node assets/build-scripts/qc.js path/to/output.docx   # and again on the .md twin
```

`qc.js` unzips the DOCX and runs these programmatic checks: heading + caption `keepNext` protection, figure-number sequence (appendix restarts allowed), caption/image pairing and caption length (≤ 70 chars), table DXA width-sum and `cantSplit` row protection, em-dash scan, DD MMM YYYY date format, banned-word scan, and "Final" status hygiene (the last four also cover the running header/footer), plus Document Control note brevity (≤ 8 words), and soft advisories for callout-free long-form documents and dark-band over-use. It **auto-repairs** what is safe (missing `keepNext` on headings/captions) and reports the rest with a specific pointer. On a **`.md` file** it validates the twin: em dash/dates/banned words, exactly one title, no heading-level skips, FIGURE sequence + caption length + per-caption embed pairing, and [N] ↔ Sources matching. Use `--fix` to write repairs back, `--json` for machine-readable output.

Things `qc.js` cannot judge are exactly what the **preview step** (pipeline step 4) covers visually: page-1 fullness, figure rendering at intended width, ≥ 11pt figure text, zebra visibility, whole small tables, Navigation Pane outline. LibreOffice/Word substitution means the render is a layout proxy — judge structure and spacing, not letterforms.

**Reporting format:**

```
QC: [PASS | N issues found, M auto-repaired]
  - [remaining failures with affected element]
Manual: [only the visual items that apply to this document]
```

For Markdown output: heading hierarchy unbroken, every table captioned, every [N] citation has a matching Sources entry, no em dashes. HTML (only on explicit request): load Aptos via the CDN link above. Layout: `break-inside: avoid` on tables, `break-after: avoid` on headings, `widows/orphans: 3` on body.

## Templates, dependencies, fonts

- `assets/templates/{memo,status,brief,assessment,differential}.md` — load the one matching the profile.
- **Before the first build in a session:** `node assets/build-scripts/preflight.js` — verifies Node ≥ 18, installs the pinned `docx` + `sharp` from the committed lockfile only when missing, and reports which preview renderer exists. One pointed message instead of a stack trace mid-build.
- Font install help: `assets/fonts/README.md`.
