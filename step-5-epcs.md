---
title: "Step 5: EPCs"
layout: default
nav_order: 10
---

# Step 5: Exposure Point Concentrations (EPCs)

The EPCs screen is where you enter the measured or estimated concentrations of each chemical at your site, organized by exposure medium. These concentration values are the core numerical inputs to the risk assessment — they feed directly into the risk calculations for every receptor and pathway.

## Layout

The screen uses a three-panel design:

- **Left sidebar** — Lists each exposure medium from your Site Model. Click a medium to view its concentration entry table. A badge shows the number of chemicals assigned to that medium (from COPCs).
- **Middle panel** — A scrollable table where you enter concentrations for each chemical in the selected medium. Chemicals are grouped by class (e.g., Metals, Volatiles, PAHs).
- **Right sidebar** — A progress tracker showing how many concentrations you've entered per chemical class, with visual progress bars and an overall completion count.

## Fields

For each chemical in each medium, you enter two values:

| Field | Type | Description |
|---|---|---|
| **Concentration** | Numeric input | The measured or estimated concentration of this chemical. Units are determined by the medium type and shown in the column header (see table below). Leave blank if no data is available for this chemical. |
| **Statistic** | Dropdown | The statistical measure the concentration represents. Options: **Maximum** (default) or **95% UCL** (95% Upper Confidence Limit). |

### Units by Medium

| Medium | Unit |
|---|---|
| Surface Soil, Subsurface Soil, Combined Soil, Sediment | mg/kg |
| Groundwater, Surface Water, Tapwater | ug/L |
| Indoor Air, Ambient Air, Sub-slab Soil Gas, Soil Gas | ug/m³ |

## Visual Indicators

| Element | Description |
|---|---|
| **Green dot** | Appears next to chemicals that have a concentration entered. Empty/gray dot means no value yet. |
| **Category headers** | Colored bars with a left accent separate chemicals by class (e.g., "Metals (5)"). |
| **Progress bars** | The right sidebar shows a bar per category indicating how many chemicals have concentrations entered (e.g., "3/5"). Bars turn green when a category is complete. |
| **Overall counter** | At the bottom of the progress sidebar, a large number shows total entered vs. total chemicals. |
| **Sidebar badges** | The left media sidebar shows the COPC count per medium. |

## Behavior

- **Immediate save:** Every concentration entry or statistic change is saved immediately.
- **Chemical rows from COPCs:** The chemicals shown in each medium's table come from your COPC selections. If you add or remove chemicals on the COPCs screen, the rows here update accordingly on your next visit.
- **Media tabs from Site Model:** The media listed in the sidebar come from the Site Model. If no media are selected, a message directs you to the Site Model screen.
- **No COPCs message:** If media are selected but no chemicals have been added, a message directs you to the COPCs screen.
- **Validation:** The concentration input accepts only non-negative numeric values. No upper limit is enforced.
- **Default statistic:** New rows default to "Maximum" as the statistic.
- **Blank = no data:** Leaving a concentration field blank means no concentration data is available for that chemical in that medium. Blank entries are excluded from downstream calculations.

## When to Use

Fill out this screen after selecting chemicals on the COPCs screen. You need:
1. At least one medium selected in the Site Model.
2. At least one chemical assigned to that medium on the COPCs screen.

The concentration values you enter here are used by:
- **Risk Assessment** — Concentrations are multiplied by intake factors and toxicity values to calculate hazard quotients and cancer risks.
- **Reports** — Concentration values appear in generated report tables.
