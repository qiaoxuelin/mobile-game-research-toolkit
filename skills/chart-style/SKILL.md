---
name: chart-style
description: >
  Transforms data and business narratives into strict consulting-style charts and visualizations.
  Use this skill whenever the user asks to: visualize data, make a chart, build a slide visual,
  create a waterfall, slope chart, Mekko, scatter, bar chart, or any business chart.
  Also triggers on phrases like "consulting style", "consulting chart", "slide visual", "deck visual",
  "make this presentation-ready", "show this as a chart", or any request involving data + visual output.
  Always apply this skill when producing charts — even casual requests like "chart this for me"
  or "show me this data visually". Produces inline SVG (renders in chat) and/or slide-ready PNG exports.
---

# Consulting Chart Style Skill

You are acting as a senior consulting visual/charting specialist. Your job is to produce
**board-ready, publication-quality charts** that follow top-tier consulting charting conventions.
Never deviate from these rules unless the user explicitly overrides them.

---

## Step 1 — Understand the Request

Before drawing anything, confirm:
1. **Data** — Do you have the numbers? If not, ask or use plausible sample data and label it "(Illustrative)"
2. **Chart type** — If not specified, select the best type using the chart selection guide below
3. **Story** — What is the "So What?" of this data? Formulate the insight headline before drawing

---

## Step 2 — Select the Right Chart Type

| Story type | Best chart |
|---|---|
| Part-to-whole, composition | Stacked bar or Mekko |
| Change over steps / bridge | Waterfall |
| Ranking or comparison | Horizontal bar |
| Trend over time | Line chart |
| Two-variable comparison | Scatter / Bubble |
| Before vs. after, shift | Slope chart |
| Distribution | Dot plot or histogram |

For detailed guidance on each type → read `references/chart-types.md`

---

## Step 3 — Apply Strict Style Rules

**Always load and follow** `references/color-palette.md` for exact hex codes.
**Always load and follow** `references/slide-anatomy.md` for layout and typography rules.

### Non-negotiable rules (never skip):
1. **Insight headline** — Bold, top-left, states the "So What?" in one sentence (e.g. "Segment A drives 73% of total growth"). Never use a descriptive title like "Revenue by Segment".
2. **Minimal ink** — No background fills, no 3D, no shadows, no gradients, no decorative borders
3. **Gridlines** — Horizontal only, light grey `#D9D9D9`, thin (0.5px)
4. **Direct labels** — Label data points directly on the chart; avoid legends wherever possible
5. **Source line** — Bottom-left, 8pt, grey `#595959`: `Source: [Source name]; consulting analysis`
6. **No chart title redundancy** — The headline IS the title. No second title.
7. **Axes** — Left axis label only if unit is ambiguous. X-axis: category labels, no title needed if obvious.

---

## Step 4 — Produce the Output

### Primary output: Inline SVG
- Render directly in chat using the `show_widget` visualizer tool
- ViewBox: `0 0 800 480` for landscape (default), `0 0 600 700` for portrait
- Font: use system sans-serif stack — `font-family: 'Arial', 'Helvetica Neue', sans-serif`
- All text via `<text>` elements, all shapes via `<rect>`, `<line>`, `<path>`, `<circle>`
- Always include a subtle drop-shadow on the slide frame: `filter: drop-shadow(0 2px 8px rgba(0,0,0,0.10))`

### Secondary output: Slide-ready PNG (when requested)
- Wrap the SVG in an HTML file with white `#FFFFFF` background, exact 1280×720px canvas
- Save to `/mnt/user-data/outputs/chart_[name].html` and call `present_files`
- Instruct the user: "Open in browser → Cmd+Shift+4 (Mac) or Snipping Tool (Windows) to screenshot at 1280×720"

---

## Step 5 — Self-Check Before Delivering

Run through this checklist mentally before showing the chart:
- [ ] Insight headline present and punchy?
- [ ] Correct color palette used?
- [ ] No legends (direct labels instead)?
- [ ] Gridlines horizontal only, light grey?
- [ ] Source line present?
- [ ] Data labels formatted correctly (1 decimal for %, whole numbers for counts)?
- [ ] Chart type matches the data story?

---

## Reference Files

Load these when you need detail:
- `references/color-palette.md` — All hex codes, usage rules, accent color system
- `references/chart-types.md` — Per-chart-type construction rules (waterfall, mekko, slope, scatter)
- `references/slide-anatomy.md` — Exact layout grid, font sizes, spacing rules
