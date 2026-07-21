# Eurobox Logistics Calculator

[![Eurobox Logistics Calculator — box size tiles grouped by footprint](screenshots/app.png)](https://danielrosehill.github.io/eurobox-calculator/)

[![TEU utilisation meters, pallet breakdown, and layer layout](screenshots/results.png)](https://danielrosehill.github.io/eurobox-calculator/)

A single-page calculator for turning Eurobox (Euronorm stacking container) quantities into logistics figures:

- **Total volume** in m³ and litres (external dimensions)
- **Euro pallet loads** (EUR-1 or EUR-6 half pallet) at a selectable max stack height, with per-layer layout diagrams
- **20 ft container (TEU) utilisation** by volume and by pallet floor positions
- **A4 PDF report** via the "Save as PDF" button (browser print-to-PDF, print stylesheet)

Box sizes are grouped by footprint in collapsible sections; each height variant has an illustrated tile (box icon with height arrow) and a quantity field.

**Live:** https://danielrosehill.github.io/eurobox-calculator/

## Box sizes

Standard footprints included: 600 × 400 (heights 12–42 cm incl. 17.5, 23, 31 cm), 400 × 300, 300 × 200, 800 × 600 — plus custom dimensions.

## Assumptions

- External box dimensions; boxes are stacked in aligned grid layers with no overhang
- Single-SKU pallets (box types are not mixed on one pallet) — counts are conservative planning estimates, not a packing optimisation
- EUR-1 pallet: 1200 × 800 × 144 mm; 11 per 20 ft container floor
- 20 ft container internal volume: 33.2 m³

## Stack

Plain HTML/CSS/JS, no build step, no dependencies. Hosted on GitHub Pages.
