# Consulting Color Palette — Strict Reference

## Primary Palette

| Role | Name | Hex | Usage |
|---|---|---|---|
| Primary | Primary Navy | `#002060` | Primary bars, key data, headlines |
| Secondary | Dark Grey | `#404040` | Secondary bars, supporting data |
| Neutral 1 | Mid Grey | `#808080` | Tertiary / background bars |
| Neutral 2 | Light Grey | `#BFBFBF` | De-emphasized bars, ghost bars in waterfall |
| Background | White | `#FFFFFF` | Slide background — always white |
| Grid | Grid Grey | `#D9D9D9` | Gridlines only — never use for data |
| Text | Body Text | `#595959` | All body text, axis labels, source line |
| Text | Headline | `#000000` | Insight headline only |

## Accent Color (use sparingly — ONE accent per chart max)

| Name | Hex | When to use |
|---|---|---|
| Accent Blue | `#0070C0` | Highlight a single bar/point that IS the insight |
| Alert Orange | `#FF6600` | Negative / risk / decline (waterfall negative bars) |
| Positive Green | `#00B050` | Positive / growth / upside (waterfall positive bars) |
| Warning Red | `#FF0000` | Warning / below target — use with extreme caution |

## Waterfall-Specific Colors
- **Positive bridge bars**: `#00B050` (Positive Green)
- **Negative bridge bars**: `#FF6600` (Alert Orange)
- **Start/End total bars**: `#002060` (Primary Navy)
- **Connector lines**: `#D9D9D9`, dashed, 1px

## Slope / Line Chart Colors
- Line 1 (highlighted): `#002060` — 2.5px stroke
- Line 2–4 (supporting): `#808080` — 1.5px stroke, opacity 0.6
- All other lines: `#BFBFBF` — 1px stroke, opacity 0.4
- Dot markers: filled circle, same color as line, r=4

## Scatter / Bubble Colors
- Default dots: `#002060`, opacity 0.7
- Highlighted cluster: `#0070C0`, opacity 0.9
- Reference lines / quadrant dividers: `#D9D9D9`, dashed

## Mekko Colors (sequential, left to right)
Use this sequence for segments:
1. `#002060`
2. `#0070C0`
3. `#808080`
4. `#BFBFBF`
5. `#404040`
Never use more than 5 segments without grouping smaller ones into "Other"

## Rules
- NEVER use more than 1 accent color per chart
- NEVER use red and green together (colorblind accessibility)
- NEVER use fills with opacity < 0.5 on primary data
- ALWAYS use `#FFFFFF` background — never cream, never off-white
