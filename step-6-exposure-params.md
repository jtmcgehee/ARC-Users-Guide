---
title: "Step 6: Exposure Parameters"
layout: default
nav_order: 11
---

# Step 6: Exposure Parameters

This screen shows the numeric assumptions that drive risk calculations — body weight, exposure duration, ingestion rates, skin surface area, etc. ARC pre-loads EPA defaults from its reference database. You only need to make changes when site-specific conditions justify different assumptions.

## Layout

- **Left sidebar** — Lists each receptor with active Site Model selections. Click a receptor to view its parameter grids.
- **Middle panel** — Parameter grids grouped by medium category (e.g., "Exposure to Soil", "Exposure to Tapwater"). Each grid is a matrix of age bins (rows) vs. parameters (columns).
- **Right panel** — Appears when you click a cell. Shows the default value and source, and provides an entry form for overrides.

## Common Parameters

| Symbol | Description | Units |
|---|---|---|
| **AF** | Soil Adherence Factor | mg/cm² |
| **BW** | Body Weight | kg |
| **ED** | Exposure Duration | yr |
| **EF** | Exposure Frequency | days/yr |
| **ET** | Exposure Time | hrs/day |
| **EW** | Weeks Worked per Year | weeks/yr |
| **IRS** | Soil Ingestion Rate | mg/day |
| **IRW** | Water Ingestion Rate | L/day |
| **SAs** | Skin Surface Area (soil) | cm²/day |
| **SAw** | Skin Surface Area (water) | cm² |
| **ETevent** | Event Exposure Time | hrs/event |
| **EV** | Event Frequency | events/day |

Not every parameter appears for every receptor. The set shown depends on which calculation equations apply.

## Age Bins

For age-stratified receptors (Recreator, Trespasser), parameters are broken out by age bin:

| Age Bin | Editable |
|---|---|
| 0-2, 2-6, 6-16, 16-26 | Yes |
| **child** (aggregate of 0-2 and 2-6) | No — calculated |
| **adult** (aggregate of 6-16 and 16-26) | No — calculated |

Child/adult values are ED-weighted averages of their constituent bins (except for ED itself, which is a sum). These update automatically when you change an individual bin.

For all other receptors, there is a single "adult" row.

## Medium Categories

Media that share the same exposure equations are grouped together:

| Grid Section | Includes |
|---|---|
| Soil | Surface Soil, Subsurface Soil, Combined Soil |
| Tapwater | Tapwater |
| Surface Water | Surface Water |
| Vapor Intrusion to Indoor Air | Groundwater VI, Soil Gas VI, Trench VI |

## Overriding a Default

1. Click any editable cell. The right panel shows the default value and source.
2. Enter a **Value** and **Source** (both required). Optionally add a **Footnote**.
3. Click **Apply**. The cell turns blue.
4. To revert, click the cell and click **Reset to Default**.

## Cell Colors

| Color | Meaning |
|---|---|
| White | EPA default |
| Blue | Site-specific override applied |
| Gray italic | Calculated aggregate (read-only) |
| Dash (-) | No value available for this combination |
