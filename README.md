# Handoff: Ask Me AI — Dashboard Builder

## Overview
An AI-assisted dashboard builder for Lumenore. The user describes a dashboard in chat; AI picks a data pool, asks clarifying questions, builds KPIs + charts progressively, then the user edits (data, style, layout, theme), and publishes.

## About the Design Files
`Ask Me AI Dashboard.dc.html` is a **design reference built in HTML** — a working prototype of look and behavior, not production code. Recreate it in the target codebase (React/Vue/etc.) using its existing component library, state management and charting stack. Data is mocked inside the file (`static D` at the top of the logic class).

Open it locally with any static server (`npx serve .`) — `support.js` is the runtime that renders the `.dc.html` template; `lm-icon.js` provides the `<lm-icon>` icon element; `map-widget.html` is the US choropleth (d3) embedded via iframe.

## Fidelity
**High-fidelity.** Colors, type, spacing and interactions are final. Match pixel-for-pixel, mapping values to your design tokens.

## Screens / Views
1. **Home / prompt** — hero prompt, suggestion chips, data-pool picker.
2. **Clarification** — inline question cards (data pool, measure, time) with "View all", preview of data pools (modal).
3. **Build (draft dashboard)** — split: chat (380px, left) + canvas.
   - Header row: editable title, status pill, data-pool link, then actions: Custom Insights, Regenerate | Undo, Redo | + Add ▾ | Appearance | Save draft (secondary) | Publish (primary) | Full view.
   - Filters row (Date / Region / Category) + "Select charts".
   - KPI row (4–8 cards) then a 4-col grid of widgets: Trend, By region (bars), Map, Share (mix), Table, Funnel, Bridge, Heatmap, Target rings.
   - Card: 8px radius, 1px #E2E8F0 border, 14/16 padding; header title 12/500 #1E293B, subtitle 10/500 #64748B; hover reveals ⤢ full view, ✎ edit, ⋮ menu.
4. **Chart editor (right aside, 352px)** — tabs Data / Style / Query.
   - Data: Title, Measure, Fields (aggregation, second measure, breakdown, calculated field), Filter & range (date range, granularity, dimension filter, Top N + Others), Sort & empty values, Comparison (prev period / last year, target line, values / running total / % of total), Number format (decimals, K/M/Lakh/Crore, prefix/suffix), Source (refresh, raw data, CSV).
   - Style: chart type grid (with "AI pick" badge), card bg/corners/border/shadow, title size/alignment/subtitle, trend options.
5. **Chart full view (focus)** — chat stays left; header (title + type tag + Done / ✕); dotted canvas preview with pan (drag), click-to-zoom (Alt = out), Ctrl+wheel zoom, zoom bar (−, %, +, Fit); settings aside on the right.
6. **Appearance panel** — themes (light/dark/solid/gradient/nature image + opacity, brand kit), layouts (8 presets w/ thumbnails), typography, chart palettes (Default/Vivid/Pastel/Ocean/Earth/Mono + custom), cards, titles.
7. **Add menu → empty slot** — Chart / Table / KPI open an empty dashed card with an AI prompt ("Generate"), AI suggestion cards, and "Or choose the data yourself →". Header / Text / Image / Section add free blocks.
8. **Publish modal** → **Published view** (header with Copy link, Edit, Full view; no edit toolbar).

## Interactions & Behavior
- Progressive build: steps reveal widgets one by one (`buildSpeed` prop).
- Card click selects (blue ring) / click again unselects; "Select charts" enables multi-select with bulk bar (Duplicate, Copy, Paste, Rename, Hide, Delete).
- Drag to reorder, corner handle to resize (snaps to grid columns).
- Undo/Redo (Ctrl/Cmd+Z, Shift+Ctrl/Cmd+Z) over dashboard state history.
- Adding any component scrolls to it and pulses a blue glow (1.4s).
- Chat with a chart in context applies changes (type, measure, top N, sort, compare, breakdown, target) or explains the chart.
- Esc exits full view / chart focus.
- Theme contrast: chart text/surfaces switch for dark & image themes.

## State Management (key state)
`step, messages, pending, ctx(poolId, grain), order[], wcfg{key:{type,measure,color,sort,top,others,agg,range,grain,excl,cmp,target,showAs,dec,short,pre,suf,calc,rw,rh,bg,radius,border,shadow,...}}, kpiItems[], kcfg{}, dups[], blocks[], removed{}, editKey, editTab, selMode, multi[], focusKey, pvZ/pvX/pvY, theme, layout, pg{}, brand, fullView, filters{}`.

## Design Tokens
- Text: #0F172A, #1E293B, #334155, #475569, #64748B
- Lines/surfaces: #E2E8F0, #F1F5F9, #F8FAFC, #FFFFFF
- Primary: #3B82F6 / hover #2563EB / pressed #1D4ED8; tint #EFF6FF, #DBEAFE, border #93C5FD
- Success #10B981, Danger #EF4444 / #B91C1C, Warning #F59E0B
- Default chart order: #3B82F6, #8B5CF6, #0D9488, #EA580C, #EC4899, #2563EB
- AI gradient: #38BDF8 → #818CF8
- Radius: 4 (controls), 8 (cards), 10–14 (popovers)
- Shadow (popover): 0 20px 60px rgba(15,23,42,.15)
- Type: Inter; 10/12/14/20px; weights 500/600

## Assets
- `assets/` — banner noise + dotted/gradient backgrounds.
- `lm-icon.js` — Lumenore icon set (`<lm-icon name="IconsProperty1…">`). Use your codebase's icon library equivalents.
- `map-widget.html` — d3 + us-atlas TopoJSON (loaded from CDN).

## Files
- `Ask Me AI Dashboard.dc.html` — full prototype (template + logic class).
- `support.js` — prototype runtime (not needed in production).
- `styles.css`, `lm-icon.js`, `map-widget.html`, `assets/`.
