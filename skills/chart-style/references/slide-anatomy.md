# Slide Anatomy — Layout & Typography Rules

## Canvas

| Format | Dimensions | Use |
|---|---|---|
| Landscape (default) | 1280 × 720px | Standard slide, most charts |
| Portrait | 900 × 1200px | Tall waterfall, long bar chart |
| SVG ViewBox (landscape) | `0 0 800 480` | Inline SVG in chat |
| SVG ViewBox (portrait) | `0 0 600 700` | Inline SVG in chat |

Background: always `#FFFFFF` white. Never cream, grey, or branded color backgrounds.

---

## Layout Grid (Landscape 800×480 ViewBox)

```
┌─────────────────────────────────────────────────┐  y=0
│  [Insight Headline]                      [Tag]  │  y=20–55
│─────────────────────────────────────────────────│  y=58 (divider line)
│                                                 │
│              CHART AREA                         │  y=70 to y=430
│                                                 │
│─────────────────────────────────────────────────│  y=435 (divider)
│  Source: ...                    [Notes if any]  │  y=448–465
└─────────────────────────────────────────────────┘  y=480
```

**Margins**:
- Left margin: 60px (for y-axis labels)
- Right margin: 40px
- Top margin: 70px (headline area)
- Bottom margin: 50px (source line area)
- Chart area: x=60 to x=760, y=70 to y=430

---

## Typography

| Element | Font size | Weight | Color | Alignment |
|---|---|---|---|---|
| Insight headline | 15px | Bold (700) | `#000000` | Left |
| Chart tag / subtitle | 10px | Regular | `#595959` | Right |
| Axis labels | 10px | Regular | `#595959` | Center/Right |
| Data labels (on chart) | 10px | Regular or Bold | `#000000` or `#FFFFFF` | Varies |
| Legend text (if needed) | 9px | Regular | `#595959` | Left |
| Source line | 8px | Regular | `#808080` | Left |
| Annotation text | 9px | Italic | `#595959` | Left |
| Callout box text | 10px | Regular | `#000000` | Left |

Font stack: `font-family: 'Arial', 'Helvetica Neue', sans-serif`

---

## Insight Headline Rules

The headline is the most important element on the slide.

**DO**:
- State the finding: "Segment A drives 73% of total revenue growth"
- Quantify when possible: "Cost reduction of $12M achievable through three levers"
- Use active voice: "New customer acquisition outpaces retention by 3×"

**DON'T**:
- Use descriptive titles: "Revenue by Segment" ✗
- Ask questions: "Which segment grows fastest?" ✗
- Use vague language: "Performance varies across segments" ✗

Position: x=60, y=35, max-width=600px. If headline exceeds one line, wrap at 600px, second line y=50.

---

## Divider Line

A single thin horizontal rule separates the headline from the chart body.
`<line x1="60" y1="58" x2="740" y2="58" stroke="#D9D9D9" stroke-width="0.8"/>`

---

## Source Line

Always present. Format:
`Source: [Source name(s)]; consulting analysis`

If data is illustrative: `Source: consulting analysis (illustrative)`

Position: x=60, y=465, font-size=8px, fill=`#808080`

---

## Callout / Annotation Boxes

Use sparingly — max 1–2 per chart. Reserved for the single most important insight callout.

```svg
<!-- Callout box -->
<rect x="X" y="Y" width="W" height="H" fill="#EBF3FB" stroke="#0070C0" stroke-width="1" rx="2"/>
<text x="X+8" y="Y+14" font-size="9" fill="#002060">Callout text here</text>
```

---

## Slide Frame (SVG)

Always wrap the chart in a white slide frame with subtle shadow:

```svg
<rect x="0" y="0" width="800" height="480" fill="white" 
      filter="url(#shadow)"/>
<defs>
  <filter id="shadow">
    <feDropShadow dx="0" dy="2" stdDeviation="4" flood-opacity="0.12"/>
  </filter>
</defs>
```
