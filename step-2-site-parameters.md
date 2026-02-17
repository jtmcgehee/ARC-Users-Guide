---
title: "Step 2: Site Parameters"
layout: default
nav_order: 7
---

# Step 2: Site Parameters

This screen captures the physical and meteorological characteristics of your site. These values drive Particulate Emission Factor (PEF), Volatilization Factor (VF), and related intermediate calculations. Most fields load with EPA defaults — you only need to change them when you have site-specific data.

## Source Dimensions

| Field | Default | Units | Description |
|---|---|---|---|
| **Contaminated Area (As)** | 0.5 | acres | Total area of contaminated soil. Used in PEF and VF calculations. |

## Construction Parameters

| Control | Description |
|---|---|
| **Configure Construction PEF Parameters** | Opens a dialog for construction-specific emission factor parameters (excavation area, silt content, vehicle parameters, etc.). |
| **Configure Trench VF Parameters** | Opens a dialog for trench volatilization parameters (trench dimensions, groundwater temperature, etc.). Changing groundwater temperature here also updates the Soil Temperature field below. |

## Site-Specific Soil Type

Select your site's soil classification (e.g., Sand, Loamy Sand, Clay) from the **Soil Type** dropdown. This auto-populates six read-only parameters from EPA reference data:

- Total Porosity (n)
- Water-Filled Porosity (nw)
- Bulk Density (ρ)
- Saturated Hydraulic Conductivity (Ks)
- Capillary Zone WF Porosity (ncz)
- Capillary Zone Height (hcz)

| Field | Default | Units | Description |
|---|---|---|---|
| **Fraction of Vegetative Cover (V)** | 0.5 | unitless | Fraction of the site covered by vegetation (0 to 1). Used in PEF wind erosion calculations. |

## VF Soil Parameters

| Field | Default | Units | Description |
|---|---|---|---|
| **Soil Temperature (Ts)** | 298.15 | K | Average soil temperature. A unit dropdown lets you enter in Kelvin, Celsius, or Fahrenheit — the value is always stored internally in Kelvin. |
| **Fraction Organic Carbon (foc)** | 0.006 | g/g | Fraction of organic carbon in site soil. Used in VF partitioning calculations. |

## Climate / Meteorology

Select the **State** and **City** dropdowns to choose the nearest NOAA weather station. This populates wind speed and precipitation data automatically.

| Field | Description |
|---|---|
| Mean Annual Wind Speed (Um) | From NOAA data for the selected city. |
| Mean Precipitation Days (p) | From NOAA data for the selected city. |
| Equivalent Threshold Friction Velocity (Ut) | EPA default (11.32 m/s). Not editable. |

If the selected city is missing wind or precipitation data, a dialog lets you enter a value manually or borrow from a nearby city. The alternate source is noted in a footnote.

When no state/city is selected, Minneapolis, MN defaults are used.

## Dispersion Parameters

Select a **Climate Zone** from the dropdown to populate six dispersion constants (A, B, C for both PEF and VF) from EPA Exhibit D-2/D-3 tables. Defaults to Minneapolis, MN.

A **Sources** card at the bottom lists EPA and NOAA references for all parameters.

## Notes

Values on this screen are saved when you navigate away or save the project (not on every keystroke). If you skip this screen, calculations will use Minneapolis/EPA defaults, which may not be appropriate for your site.
