---
title: "Step 2: Site Parameters"
layout: default
nav_order: 7
---

# Step 2: Site Parameters

The Site Parameters screen captures the physical and meteorological characteristics of your site that feed into risk calculations. These values drive the Particulate Emission Factor (PEF), Volatilization Factor (VF), and related intermediate calculations used throughout the assessment. Most fields have EPA default values that load automatically — you only need to change them when you have site-specific data.

## Fields

The screen is organized into six section cards, presented in a scrollable layout.

### Source Dimensions

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Contaminated Area (As)** | Yes | 0.5 | acres | The total area of contaminated soil at the site. Used in PEF and VF calculations. |

### Construction Parameters

This section contains two sub-panels, each configured via a dialog button rather than inline fields:

| Control | Description |
|---|---|
| **Configure Construction PEF Parameters** | Opens a dialog to enter construction-specific emission factor parameters (excavation area, silt content, vehicle parameters, etc.). Shows "Configured" or "Not Configured" status. |
| **Configure Trench VF Parameters** | Opens a dialog to enter trench volatilization parameters (trench dimensions, groundwater temperature, etc.). Shows "Configured" or "Not Configured" status. If the groundwater temperature is changed in this dialog, the Soil Temperature field in the VF Soil Parameters section updates to match. |

### Site-Specific Soil Type

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Soil Type** (dropdown) | Yes | "Select..." | — | Choose your site's soil classification (e.g., Sand, Loamy Sand, Clay). Selecting a soil type auto-populates the six read-only parameters below from EPA reference data. |
| Total Porosity (n) | Read-only | — | cm³/cm³ | Auto-populated from soil type selection. |
| Water-Filled Porosity (nw) | Read-only | — | cm³/cm³ | Auto-populated from soil type selection. |
| Bulk Density (ρ) | Read-only | — | g/cm³ | Auto-populated from soil type selection. |
| Sat. Hydraulic Conductivity (Ks) | Read-only | — | cm/h | Auto-populated from soil type selection. |
| Cap. Zone WF Porosity (ncz) | Read-only | — | cm³/cm³ | Auto-populated from soil type selection. |
| Cap. Zone Height (hcz) | Read-only | — | cm | Auto-populated from soil type selection. |
| **Fraction of Vegetative Cover (V)** | Yes | 0.5 | unitless | Fraction of the site covered by vegetation (0 to 1). Used in PEF wind erosion calculations. |

### VF Soil Parameters

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Soil Temperature (Ts)** | Yes | 298.15 | K (switchable to °C or °F) | Average soil temperature at the site. Default is 25°C (298.15 K). A unit dropdown lets you enter the value in Kelvin, Celsius, or Fahrenheit — the display converts automatically, and the value is always stored internally in Kelvin. |
| **Fraction Organic Carbon (foc)** | Yes | 0.006 | g/g | Fraction of organic carbon in site soil. Used in VF calculations for partitioning. |

### Climate / Meteorology

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **State** (dropdown) | Yes | "Default" | — | Select the state where your site is located to filter available NOAA weather stations. "Default" uses Minneapolis, MN. |
| **City** (dropdown) | Yes | Disabled when Default | — | Select the nearest NOAA station city. Populates wind speed and precipitation days below. |
| Mean Annual Wind Speed (Um) | Read-only | Minneapolis value | m/s | Auto-populated from NOAA data for the selected city. If the selected city is missing wind data, a dialog lets you enter a value manually or borrow from a nearby city. |
| Mean Precipitation Days (p) | Read-only | Minneapolis value | days/year | Auto-populated from NOAA data for the selected city. Same missing-data dialog as wind speed. |
| Equivalent Threshold Friction Velocity (Ut) | Read-only | 11.32 | m/s | EPA default value. Not user-editable. |

### Dispersion Parameters

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Climate Zone** (dropdown) | Yes | "Default" | — | Select the nearest city from EPA Exhibit D-2/D-3 tables. "Default" uses Minneapolis, MN. Populates the six dispersion constants below. |
| Dispersion Constant A (PEF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-2. Auto-populated from Climate Zone selection. |
| Dispersion Constant B (PEF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-2. Auto-populated from Climate Zone selection. |
| Dispersion Constant C (PEF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-2. Auto-populated from Climate Zone selection. |
| Dispersion Constant A (VF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-3. Auto-populated from Climate Zone selection. |
| Dispersion Constant B (VF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-3. Auto-populated from Climate Zone selection. |
| Dispersion Constant C (VF) | Read-only | Minneapolis value | unitless | From EPA Exhibit D-3. Auto-populated from Climate Zone selection. |

A **Sources** card at the bottom lists EPA and NOAA references for all parameters. If wind speed or precipitation data was borrowed from a different city, a dynamic footnote appears identifying the alternate source.

## Behavior

- **Database defaults:** All fields load with EPA/RSL default values from the reference database on first use. You only need to change values when you have site-specific data.
- **Deferred save:** Unlike the Project Info screen, this screen does not write on every keystroke. Values are synced when you navigate away from the screen or when you save the project.
- **Temperature unit conversion:** The Soil Temperature field has a dropdown to switch between Kelvin, Celsius, and Fahrenheit. Changing the unit converts the displayed value in place. The value is always stored internally in Kelvin regardless of the display unit.
- **Missing NOAA data:** When a selected city has wind speed but no precipitation data (or vice versa), a dialog appears offering two options: enter the missing value manually, or select a nearby city that has the data. The alternate source is tracked and shown in a footnote.
- **Soil type auto-population:** Selecting a soil type from the dropdown fills in six read-only parameters (porosity, density, hydraulic conductivity, capillary zone properties). Changing the dropdown updates all six at once. Selecting "Select..." clears them.

## When to Use

Fill out this screen early in your workflow, typically right after Project Info. Many downstream calculations depend on site parameters — the PEF panels, VF intermediate calculations, and all receptor-specific risk calculations read from this screen's data. If you skip it, calculations will use Minneapolis/EPA defaults, which may not be appropriate for your site.
