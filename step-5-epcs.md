---
title: "Step 5: EPCs"
layout: default
nav_order: 10
---

# Step 5: Exposure Point Concentrations (EPCs)

This is where you enter the measured or estimated concentrations of each chemical at your site, organized by exposure medium. These values feed directly into the risk calculations.

## Layout

- **Left sidebar** — Lists each exposure medium. A badge shows the chemical count per medium.
- **Middle panel** — A table for entering concentrations, grouped by chemical class.
- **Right sidebar** — Progress tracker showing how many concentrations you've entered per category, with progress bars and an overall count.

## Fields

For each chemical in each medium:

| Field | Description |
|---|---|
| **Concentration** | The measured or estimated concentration. Leave blank if no data is available. |
| **Statistic** | The statistical measure — **Maximum** (default) or **95% UCL**. |

### Units by Medium

| Medium | Unit |
|---|---|
| Surface Soil, Subsurface Soil, Combined Soil, Sediment | mg/kg |
| Groundwater, Surface Water, Tapwater | ug/L |
| Indoor Air, Ambient Air, Sub-slab Soil Gas, Soil Gas | ug/m³ |

## Visual Indicators

- **Green dot** — Concentration entered. Gray dot — no value yet.
- **Progress bars** — Per-category bars in the right sidebar turn green when complete.

## Notes

- Chemical rows come from your COPC selections. Adding or removing chemicals on the COPCs screen updates the rows here.
- Only non-negative numeric values are accepted.
- Blank entries are excluded from calculations.
