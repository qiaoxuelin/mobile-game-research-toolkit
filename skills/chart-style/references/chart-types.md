# Chart Types — Construction Rules

## 1. Waterfall Chart

**When to use**: Show how an initial value is transformed by a series of positive/negative increments to reach a final value. Classic for revenue bridge, cost bridge, headcount change.

**Construction rules**:
- First bar (start) and last bar (end/total): full-height, `#002060` navy
- Positive increment bars: `#00B050` green, floating (y starts at previous cumulative)
- Negative increment bars: `#FF6600` orange, floating downward
- Thin dashed connector lines between bars: `#D9D9D9`
- Label each bar with its value (+X / -X format for increments, absolute for start/end)
- X-axis: category labels below each bar
- No Y-axis needed if all bars are directly labeled

**SVG approach**: Calculate cumulative baseline for each bar. Each bar's `y` = canvas_height - (cumulative_value * scale). Floating bars use `y` = previous_cumulative_y.

---

## 2. Horizontal Bar Chart (Ranking / Comparison)

**When to use**: Compare values across categories. Use horizontal (not vertical) when category labels are long or when ranking is the point.

**Construction rules**:
- Bars left-to-right, longest at top (sorted descending unless order has inherent meaning)
- Primary metric: `#002060`; comparison metric: `#808080` (grouped bars)
- Bar height: ~60% of row height (leave breathing room between bars)
- Data labels: right-aligned, just outside bar end
- Highlight bar (the "So What?" bar): use `#0070C0` accent
- No Y-axis line; no X-axis gridlines for horizontal bars (use direct labels instead)
- Category labels: left-aligned, `#595959`

---

## 3. Vertical Bar Chart (Time series / Category)

**When to use**: Show values over discrete time periods or categories where vertical orientation aids comparison.

**Construction rules**:
- Same color rules as horizontal bar
- Horizontal gridlines only: `#D9D9D9`, 0.5px
- X-axis: category/time labels
- Y-axis: optional if all bars directly labeled; if shown, no axis line, just labels
- Bar width: ~60% of available slot width
- For grouped bars: max 2 groups; use `#002060` + `#808080`

---

## 4. Slope Chart

**When to use**: Show change between exactly TWO time points for multiple entities. Better than grouped bars when the direction of change is the story.

**Construction rules**:
- Two vertical axes (left = T1, right = T2), no connecting gridlines between them
- Each entity = one line connecting left dot to right dot
- Highlighted entity: `#002060`, 2.5px, label both ends
- Other entities: `#808080`, 1.5px, opacity 0.5, label right end only
- Dots: filled circles r=5 at each endpoint
- Left labels: right-aligned just left of left axis
- Right labels: left-aligned just right of right axis
- Show % change or absolute delta as a small annotation on the line midpoint (optional)

---

## 5. Line Chart (Trend)

**When to use**: Continuous trend over time with 3+ time points.

**Construction rules**:
- Primary line: `#002060`, 2.5px, dot markers at data points
- Supporting lines: `#808080`, 1.5px, 0.6 opacity
- Label last data point directly (right side of chart)
- Horizontal gridlines: `#D9D9D9`, 0.5px
- X-axis: time labels (years/quarters)
- No fill under line (avoid area charts unless specifically requested)
- Annotations for key events: vertical dashed line `#808080` + text label

---

## 6. Mekko / Marimekko Chart

**When to use**: Show both segment SIZE (column width) and segment COMPOSITION (bar height split). Classic for market analysis.

**Construction rules**:
- X-axis: column width = relative market/segment size (must sum to 100% of chart width)
- Y-axis: each column = 100% height, split by sub-segment share
- Colors: use sequential palette (navy → blue → greys)
- Column labels: top of each column, centered, bold — show column name + size (e.g. "Enterprise\n$45B")
- Segment labels: inside each colored block if block is tall enough (>8% height); outside with leader line if smaller
- Column dividers: white 2px gap between columns
- Add % labels inside each block
- This is the most complex chart type — take extra care with proportional widths

**SVG approach**: 
- Total chart width W, total chart height H (use 400px height)
- Column i width = (segment_size_i / total_size) * W
- Each block height within column = (share_pct / 100) * H
- Stack blocks bottom to top

---

## 7. Scatter / Bubble Chart

**When to use**: Show relationship between two continuous variables. Add bubble size for a third dimension.

**Construction rules**:
- Dots: `#002060`, opacity 0.7, r=6 (scatter) or proportional (bubble)
- Highlighted cluster/outlier: `#0070C0`, opacity 0.9, slightly larger
- Quadrant dividers (if used): `#D9D9D9`, dashed, 1px — label each quadrant lightly
- Axis labels: descriptive (e.g. "Market growth rate (%)"), `#595959`, 10pt
- Reference line (average/target): `#808080`, dashed
- Label only notable outliers directly; avoid labeling every dot
- Regression line (if shown): `#808080`, 1.5px solid
