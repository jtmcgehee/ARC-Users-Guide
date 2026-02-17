# Site Parameters Screen

## User's Guide Content

### Purpose

The Site Parameters screen captures the physical and meteorological characteristics of your site that feed into risk calculations. These values drive the Particulate Emission Factor (PEF), Volatilization Factor (VF), and related intermediate calculations used throughout the assessment. Most fields have EPA default values that load automatically — you only need to change them when you have site-specific data.

### Fields

The screen is organized into six section cards, presented in a scrollable layout.

#### Source Dimensions

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Contaminated Area (As)** | Yes | 0.5 | acres | The total area of contaminated soil at the site. Used in PEF and VF calculations. |

#### Construction Parameters

This section contains two sub-panels, each configured via a dialog button rather than inline fields:

| Control | Description |
|---|---|
| **Configure Construction PEF Parameters** | Opens a dialog to enter construction-specific emission factor parameters (excavation area, silt content, vehicle parameters, etc.). Shows "Configured" or "Not Configured" status. |
| **Configure Trench VF Parameters** | Opens a dialog to enter trench volatilization parameters (trench dimensions, groundwater temperature, etc.). Shows "Configured" or "Not Configured" status. If the groundwater temperature is changed in this dialog, the Soil Temperature field in the VF Soil Parameters section updates to match. |

#### Site-Specific Soil Type

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

#### VF Soil Parameters

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **Soil Temperature (Ts)** | Yes | 298.15 | K (switchable to °C or °F) | Average soil temperature at the site. Default is 25°C (298.15 K). A unit dropdown lets you enter the value in Kelvin, Celsius, or Fahrenheit — the display converts automatically, and the value is always stored internally in Kelvin. |
| **Fraction Organic Carbon (foc)** | Yes | 0.006 | g/g | Fraction of organic carbon in site soil. Used in VF calculations for partitioning. |

#### Climate / Meteorology

| Field | Editable | Default | Units | Description |
|---|---|---|---|---|
| **State** (dropdown) | Yes | "Default" | — | Select the state where your site is located to filter available NOAA weather stations. "Default" uses Minneapolis, MN. |
| **City** (dropdown) | Yes | Disabled when Default | — | Select the nearest NOAA station city. Populates wind speed and precipitation days below. |
| Mean Annual Wind Speed (Um) | Read-only | Minneapolis value | m/s | Auto-populated from NOAA data for the selected city. If the selected city is missing wind data, a dialog lets you enter a value manually or borrow from a nearby city. |
| Mean Precipitation Days (p) | Read-only | Minneapolis value | days/year | Auto-populated from NOAA data for the selected city. Same missing-data dialog as wind speed. |
| Equivalent Threshold Friction Velocity (Ut) | Read-only | 11.32 | m/s | EPA default value. Not user-editable. |

#### Dispersion Parameters

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

### Behavior

- **Database defaults:** All fields load with EPA/RSL default values from the reference database on first use. You only need to change values when you have site-specific data.
- **Deferred save:** Unlike the Project Info screen, this screen does not write to ProjectState on every keystroke. Values are synced to ProjectState when you navigate away from the screen or when you save the project.
- **Temperature unit conversion:** The Soil Temperature field has a dropdown to switch between Kelvin, Celsius, and Fahrenheit. Changing the unit converts the displayed value in place. The value is always stored internally in Kelvin regardless of the display unit.
- **Missing NOAA data:** When a selected city has wind speed but no precipitation data (or vice versa), a dialog appears offering two options: enter the missing value manually, or select a nearby city that has the data. The alternate source is tracked and shown in a footnote.
- **Soil type auto-population:** Selecting a soil type from the dropdown fills in six read-only parameters (porosity, density, hydraulic conductivity, capillary zone properties). Changing the dropdown updates all six at once. Selecting "Select..." clears them.

### When to Use

Fill out this screen early in your workflow, typically right after Project Info. Many downstream calculations depend on site parameters — the PEF panels, VF intermediate calculations, and all receptor-specific risk calculations read from this screen's data. If you skip it, calculations will use Minneapolis/EPA defaults, which may not be appropriate for your site.

---

## Field Verification

**Field coverage: All fields verified.** Every user-editable and auto-populated field is read from and written to `state.site_params` (or separate sections for Construction PEF and Trench VF). Values persist across navigation and save/load.

### User-Editable Input Fields (4)

| # | Field Key | Read from State | Write to State | Round-Trip |
|---|---|---|---|---|
| 1 | `As` | `_populate_from_state()` line 1425 | `sync_to_project_state()` line 1531 | Yes |
| 2 | `V` | `_populate_from_state()` line 1427 | `sync_to_project_state()` line 1531 | Yes |
| 3 | `Ts` | `_populate_from_state()` line 1429 (+ unit reset to K) | `sync_to_project_state()` line 1531 (converts to K via `_get_input_value`) | Yes |
| 4 | `foc` | `_populate_from_state()` line 1434 | `sync_to_project_state()` line 1531 | Yes |

### Dropdown Selections (4)

| # | Field Key(s) | Read from State | Write to State | Round-Trip |
|---|---|---|---|---|
| 5 | `soil_type_name` | `_populate_from_state()` line 1458 | `sync_to_project_state()` line 1547 | Yes |
| 6 | `climate_zone_city`, `climate_zone_state` | `_populate_from_state()` line 1466 | `sync_to_project_state()` line 1558 | Yes |
| 7 | `noaa_state` | `_populate_from_state()` line 1477 | `sync_to_project_state()` line 1566 | Yes |
| 8 | `noaa_city` | `_populate_from_state()` line 1500 | `sync_to_project_state()` line 1570 | Yes |

### Auto-Populated Read-Only Fields (15)

| # | Field Key | Read from State | Write to State | Round-Trip |
|---|---|---|---|---|
| 9 | `soil_n` | line 1437+ (implicit via dropdown restore) | line 1551 | Yes |
| 10 | `soil_nw` | implicit via dropdown restore | line 1551 | Yes |
| 11 | `soil_rho` | implicit via dropdown restore | line 1551 | Yes |
| 12 | `soil_Ks` | implicit via dropdown restore | line 1551 | Yes |
| 13 | `soil_ncz` | implicit via dropdown restore | line 1551 | Yes |
| 14 | `soil_hcz` | implicit via dropdown restore | line 1551 | Yes |
| 15 | `Um` | line 1440 | line 1537 | Yes |
| 16 | `p` | line 1442 | line 1542 | Yes |
| 17 | `Ut` | line 1438 | line 1537 | Yes |
| 18 | `A` | line 1444 | line 1537 | Yes |
| 19 | `B` | line 1446 | line 1537 | Yes |
| 20 | `C` | line 1448 | line 1537 | Yes |
| 21 | `A_vol` | line 1450 | line 1537 | Yes |
| 22 | `B_vol` | line 1452 | line 1537 | Yes |
| 23 | `C_vol` | line 1454 | line 1537 | Yes |

### NOAA Override Metadata (4)

| # | Field Key | Read from State | Write to State | Round-Trip |
|---|---|---|---|---|
| 24 | `um_manual_override` | line 1507 | line 1575 | Yes |
| 25 | `p_manual_override` | line 1508 | line 1576 | Yes |
| 26 | `um_source_city` | line 1509 | line 1577 | Yes |
| 27 | `p_source_city` | line 1510 | line 1578 | Yes |

### Dialog-Configured Sections (2)

| # | Section | Storage | Round-Trip |
|---|---|---|---|
| 28 | Construction PEF | `state.construction_pef` (separate section) | Yes — dialog reads/writes directly |
| 29 | Trench VF | `state.trench_vf` (separate section) | Yes — dialog reads/writes directly |

**Sync mechanism:** This screen uses a deferred-write pattern. `sync_to_project_state()` is called by the main window on navigation away (`_navigate_to`, line 524) and before every save (`_save_project` / `_save_project_as`).

---

## Dependencies

### Upstream

None. This screen loads all default values from the reference database (`arc.db` via `ARCService`). It does not depend on data entered on any other screen.

### Downstream

Site parameters are the most heavily consumed data section in the application.

| Consumer | Fields Used | Purpose |
|---|---|---|
| [base.py](../../core/calculations/service/base.py) — `_load_site_params()` | `As`, `Um`, `Ut`, `V`, `A`, `B`, `C`, `A_vol`, `B_vol`, `C_vol`, `soil_n`, `soil_nw`, `soil_rho`, `foc` | Base calculation service loads all site params into a cache dict. Used by `_compute_pef()` and `_compute_vf()` for all receptor types. |
| [construction_worker_service.py](../../core/calculations/service/construction_worker_service.py) | All base fields + `V_con`, `p`, `soil_n`, `soil_nw` | Construction-specific PEF (vehicle + mechanical) and trench VF calculations. |
| [utility_worker_service.py](../../core/calculations/service/utility_worker_service.py) | All base fields + `V_con`, `p`, `soil_n`, `soil_nw` | Utility worker PEF and trench VF calculations. |
| [wind_pef_panel.py](../../ui/panels/wind_pef_panel.py) | `Um`, `Ut`, `As`, `V`, `A`, `B`, `C` | Displays wind erosion PEF inputs and results. |
| [vehicle_pef_panel.py](../../ui/panels/vehicle_pef_panel.py) | `As`, `p` | Displays vehicle PEF inputs and results. |
| [other_pef_panel.py](../../ui/panels/other_pef_panel.py) | `As`, `V_con`, `Um`, `Ut` | Displays mechanical/other PEF inputs and results. |
| [construction_pef_dialog.py](../../ui/dialogs/construction_pef_dialog.py) | `As` | Pre-fills excavation area default from contaminated area. |
| [trench_vf_dialog.py](../../ui/dialogs/trench_vf_dialog.py) | `Lgw`, `Ts` | Uses groundwater depth for default trench depth; reads soil temperature. |
| [risk_assessment.py](../../ui/views/risk_assessment.py) | (section-level watch) | Invalidates calculation cache when `site_params` changes. |

**Not consumed downstream:** `soil_Ks`, `soil_ncz`, `soil_hcz` are persisted but not currently read by any calculation service or panel outside this screen.
