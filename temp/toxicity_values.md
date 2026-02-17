# Toxicity Values Screen

## User's Guide Content

### Purpose

The Toxicity Values screen displays the toxicity reference values that ARC uses to calculate health risks for each chemical at your site. These values — such as reference doses, cancer slope factors, and inhalation unit risks — come from EPA and other regulatory databases. This screen lets you review the defaults, apply site-specific overrides, and assign surrogate chemicals when a chemical lacks its own toxicity data.

### Layout

The screen uses a three-panel design:

- **Left sidebar** — Lists all chemicals selected across all media on the COPCs screen, grouped by chemical class (e.g., Metals, Volatiles, PAHs). Click a chemical to view its toxicity values.
- **Middle panel** — Displays the toxicity values for the selected chemical, organized into labeled sections. Each parameter is shown as a clickable cell with a value, label, and units. A "Use Surrogate" button appears in the header.
- **Right panel** — A detail/override panel that appears when you click a cell. Shows the default value and metadata, provides an entry form for manual overrides, and displays surrogate information when applicable.

### Parameters

Toxicity values are organized into the following sections:

| Section | Parameters | Description |
|---|---|---|
| **Carcinogenic** | CSFo, IUR | Cancer Slope Factor (oral) and Inhalation Unit Risk — used to estimate lifetime cancer risk. |
| **Non-Carcinogenic (Oral/Dermal)** | RfDo, Dermal RfD, SRfDo, Dermal RfD_sc | Reference Dose (chronic and subchronic) for oral and dermal pathways. Dermal RfD values are calculated automatically. |
| **Non-Carcinogenic (Inhalation)** | RfC, SRfC | Reference Concentration (chronic and subchronic) for inhalation. |
| **Bioavailability** | GIABS, ABSd | GI Absorption Factor and Dermal Absorption Factor — adjustment factors that modify intake calculations. |
| **Acute (Oral/Dermal)** | ARfDo, Dermal RfD_acute | Acute reference dose for short-duration exposures. Dermal value is calculated. |
| **Acute (Inhalation)** | ARfC | Acute reference concentration for inhalation. |
| **Short-term (Oral/Dermal)** | STRfDo, Dermal RfD_st | Short-term reference dose. Dermal value is calculated. |
| **Short-term (Inhalation)** | STRfC | Short-term reference concentration. |

#### Units

| Parameter | Units |
|---|---|
| CSFo | (mg/kg-day)⁻¹ |
| IUR | (µg/m³)⁻¹ |
| RfDo, SRfDo, ARfDo, STRfDo, Dermal RfD variants | mg/kg-day |
| RfC, SRfC, ARfC, STRfC | mg/m³ |
| GIABS, ABSd | unitless |

#### Calculated Parameters

Four parameters are **calculated automatically** and cannot be directly edited:

| Parameter | Formula |
|---|---|
| **Dermal RfD** | RfDo × GIABS |
| **Dermal RfD_sc** (subchronic) | SRfDo × GIABS (uses chronic RfDo as fallback if SRfDo is unavailable) |
| **Dermal RfD_acute** | ARfDo × GIABS |
| **Dermal RfD_st** (short-term) | STRfDo × GIABS |

These update live when you override RfDo, SRfDo, ARfDo, STRfDo, or GIABS.

#### Subchronic Fallback

When a chemical has no subchronic value (SRfDo or SRfC) in the database, ARC automatically uses the chronic value (RfDo or RfC) as a surrogate. These fallback values are marked with a dagger symbol (†) in the grid and a footnote appears at the bottom: "† Chronic value used as surrogate for subchronic."

### How to Override a Default

1. Click a chemical in the left sidebar.
2. Click any editable cell in the middle panel. The right panel shows the default value with its source and metadata.
3. In the "Enter Manual Override" section, fill in the required fields:
   - **Value** (required) — The numeric override value. Enter "N/A" if not applicable.
   - **Source** (required) — Citation or justification for the override.
   - For **carcinogenic** parameters (CSFo, IUR): **Tumor Site** is also required.
   - For **non-carcinogenic** parameters (RfDo, RfC, etc.): **Target Organ**, **UF** (Uncertainty Factor), **MF** (Modifying Factor), and **Confidence** are all required. Enter "N/A" for any that don't apply.
   - **Footnote** (optional) — An additional note.
4. Click **Apply**. The cell turns blue to indicate an override.
5. To revert, click the overridden cell and click **Reset to Default** in the right panel.

### How Surrogates Work

When a chemical lacks toxicity data, you can borrow values from a chemically similar compound:

1. Click the chemical in the left sidebar.
2. Click the **Use Surrogate** button in the middle panel header.
3. In the Surrogate Selection Dialog:
   - Search for and select a surrogate chemical from the full database.
   - The dialog shows side-by-side comparison: the current chemical's values vs. the surrogate's values.
   - Toggle on/off which individual parameters to apply from the surrogate (e.g., apply CSFo but not RfDo).
4. Click **Apply**. The selected parameters are copied from the surrogate and stored as overrides.
5. To modify or remove the surrogate, click **Edit Surrogate** (the button text changes after a surrogate is applied).
6. In the detail panel, surrogate-applied values show a "From Surrogate" label with the surrogate chemical name.

Surrogate overrides behave like manual overrides in calculations — the calculation services don't distinguish between the two. However, in the UI, surrogate overrides cannot be reset individually using the Reset button; you must use the Edit Surrogate button to clear them.

### Visual Indicators

| Element | Description |
|---|---|
| **White cell** | Using the database default value. |
| **Blue cell** | A manual override or surrogate value has been applied. |
| **Gray italic cell** | A calculated value (Dermal RfD variants) — read-only, derived from other parameters. |
| **Gray cell with "--"** | No value available in the database for this parameter. |
| **Dagger (†)** | Chronic value used as subchronic fallback. |
| **"From Surrogate" label** | In the detail panel, indicates the value came from a surrogate chemical assignment. |
| **"Use Surrogate" / "Edit Surrogate"** | Button text changes based on whether a surrogate is currently assigned. |

### Behavior

- **Defaults from database:** All default toxicity values are loaded from the ARC reference database (`arc.db`) via ARCService. GIABS and ABSd come from the chemical's physical properties record. All other values come from the toxicity endpoints table with source priority ordering.
- **Override-only storage:** ARC does not store every toxicity value — only your manual overrides and surrogate assignments are stored in ProjectState. If no override exists, the database default is used.
- **Immediate save:** Every override you apply or reset is written to ProjectState immediately. `sync_to_project_state()` is a no-op.
- **Chemical list from COPCs:** The chemicals shown in the sidebar come from the union of all CAS numbers across all media on the COPCs screen. If no chemicals have been selected, the sidebar is empty.
- **Calculated values update live:** Overriding RfDo, GIABS, or any oral RfD triggers an automatic recalculation of all Dermal RfD variants in the grid.
- **Surrogate tracking:** When you apply a surrogate, ARC stores a tracking record (`__surrogate__<cas>`) listing which parameters were applied. This allows the Edit Surrogate dialog to show the current assignment and lets the Clear function remove only surrogate-originated overrides.

### When to Use

Visit this screen after selecting chemicals on the COPCs screen. You need at least one chemical selected in any medium.

In many cases, the database defaults will be appropriate. Use this screen when:
- A chemical is missing key toxicity values and you want to assign a surrogate.
- Your regulatory program requires different values than the database defaults.
- You need to document the source and justification for a site-specific toxicity value.

The values on this screen feed directly into the risk calculations and appear in generated toxicity reports.

---

## Field Verification

**Field coverage: All fields verified.** Every user override and surrogate assignment is read from and written to `state.toxicity_values`. Values persist across navigation and save/load.

### Data Structure

```
state.toxicity_values = {
    "7440-38-2|CSFo": {
        "value": 1.5,
        "source": "IRIS (2024)",
        "tumor_site": "Lung, Skin"
    },
    "7440-38-2|RfDo": {
        "value": 0.0003,
        "source": "IRIS",
        "target_organ": "Skin",
        "uf": "3",
        "mf": "1",
        "confidence": "Medium"
    },
    "71-43-2|CSFo": {
        "value": 0.055,
        "source": "IRIS",
        "is_surrogate": true,
        "surrogate_cas": "108-88-3",
        "surrogate_name": "Toluene"
    },
    "__surrogate__71-43-2": {
        "surrogate_cas": "108-88-3",
        "surrogate_name": "Toluene",
        "applied_params": ["CSFo", "IUR", "RfDo"]
    },
    ...
}
```

Keys follow two patterns:
- **Override keys:** `"<cas_number>|<param>"` — Stores the override value, source, and category-specific metadata (tumor_site for carcinogenic; target_organ, uf, mf, confidence for non-carcinogenic). Surrogate-applied overrides additionally include `is_surrogate`, `surrogate_cas`, and `surrogate_name`.
- **Surrogate tracking keys:** `"__surrogate__<cas_number>"` — Stores which surrogate chemical is assigned and which parameters were applied. Used by the UI to manage surrogate state; filtered out by downstream consumers.

### Read/Write Verification

| Aspect | Mechanism | Location |
|---|---|---|
| **Write (manual override)** | `_on_override_applied()` writes via `state.set("toxicity_values", key, override_data)`. Fires immediately when user clicks Apply. Key format: `"cas_number\|param"`. | line 1990 |
| **Write (reset)** | `_on_reset_clicked()` removes via `state.unset("toxicity_values", key)`. Fires immediately when user clicks Reset to Default. | line 2020 |
| **Write (surrogate apply)** | `_apply_surrogate()` writes a tracking key (`__surrogate__<cas>`) and one override key per selected parameter, all within `suppress_signals()`. Each override includes `is_surrogate: True`. | lines 2106-2146 |
| **Write (surrogate clear)** | `_clear_surrogate()` removes all surrogate override keys (where `is_surrogate` is True) and the tracking key, within `suppress_signals()`. | lines 2152-2168 |
| **Read** | `_get_overrides()` reads the entire `toxicity_values` section via `state.get_section("toxicity_values")`. Called on every chemical selection and value selection to populate override state. | lines 1907-1911 |
| **Read (surrogate)** | `_get_surrogate_info()` reads `overrides.get(f"__surrogate__{cas}")` to check if a surrogate is assigned. | lines 2053-2060 |
| **Round-trip** | Yes — immediate write on every override/surrogate action; full restore on load/navigate via `_populate_from_state()`. | |
| **Sync** | `sync_to_project_state()` is a no-op (line 2220-2225) — state is always current. | |

### Per-field mapping (manual override)

| UI Element | Stored As | Applicable To |
|---|---|---|
| Value input | `overrides[key]["value"]` (float or None for "N/A") | All overridable params |
| Source input | `overrides[key]["source"]` (string) | All overridable params |
| Tumor Site input | `overrides[key]["tumor_site"]` (string) | Carcinogenic only (CSFo, IUR) |
| Target Organ input | `overrides[key]["target_organ"]` (string) | Non-carcinogenic only |
| UF input | `overrides[key]["uf"]` (string) | Non-carcinogenic only |
| MF input | `overrides[key]["mf"]` (string) | Non-carcinogenic only |
| Confidence input | `overrides[key]["confidence"]` (string) | Non-carcinogenic only |
| Footnote input | `overrides[key]["footnote"]` (string) | All (optional) |

### Per-field mapping (surrogate)

| UI Element | Stored As | Location |
|---|---|---|
| Surrogate chemical selection | `__surrogate__<cas>["surrogate_cas"]`, `["surrogate_name"]` | Tracking key |
| Parameter toggle switches | `__surrogate__<cas>["applied_params"]` (list of param names) | Tracking key |
| Individual parameter values | `<cas>\|<param>["value"]`, `["source"]`, `["is_surrogate"]`, `["surrogate_cas"]`, `["surrogate_name"]` | Per-param override keys |

---

## Dependencies

### Upstream

| Source | What's Used | Purpose |
|---|---|---|
| [copcs.py](../views/copcs.py) — `copcs.by_medium` | Union of all CAS numbers across all media | Determines which chemicals appear in the left sidebar. Loaded via `_load_copcs()` which collects unique CAS numbers and looks up chemical details from the database. |
| Reference database (`arc.db`) | Toxicity endpoints, chemical properties (GIABS, ABSd), full chemical list | Provides default toxicity values via `ARCService.get_toxicity_values()` and chemical properties via `get_chemical_full()`. Also provides the full chemical list for the surrogate selection dialog. |

### Downstream

| Consumer | What It Reads | Purpose |
|---|---|---|
| [base.py](../../core/calculations/service/base.py) — `_apply_tox_overrides()` | `state.toxicity_values["<cas>\|<param>"]["value"]` for CSFo, IUR, RfDo, RfC | Applies user overrides (including surrogate values) on top of database defaults before feeding toxicity values into risk calculations. Called by `_get_toxicity()` which is used by all 8 receptor calculation services. |
| [base.py](../../core/calculations/service/base.py) — `_get_chemical_properties()` | `state.toxicity_values["<cas>\|GIABS"]["value"]` | Checks for GIABS override specifically, since GIABS flows through the chemical properties path rather than the toxicity path. Applied to the `giabs` property of the chemical record used in dermal absorption and volatilization calculations. |
| [generator.py](../../reports/generator.py) — `_extract_overrides()` | Entire `toxicity_values` section, filtered to exclude `__surrogate__` keys | Passes overrides to report table generators so that toxicity reports reflect user overrides and surrogate values. |
| [toxicity_oral_dermal_nc.py](../../reports/toxicity_oral_dermal_nc.py) | Override values for RfDo, SRfDo, GIABS and their metadata (source, target_organ, uf, mf, confidence) | Builds oral/dermal non-carcinogenic toxicity report tables, applying overrides and showing footnotes for modified values. |
| [toxicity_inhalation_nc.py](../../reports/toxicity_inhalation_nc.py) | Override values for RfC, SRfC and their metadata | Builds inhalation non-carcinogenic toxicity report tables with the same override/footnote logic. |
| [risk_assessment.py](../views/risk_assessment.py) — cache invalidation | `toxicity_values` section membership in `_CACHE_INVALIDATING_SECTIONS` | Any change to `toxicity_values` invalidates the calculation cache, triggering recalculation on next view. |

**Not read downstream:** The `__surrogate__` tracking keys are internal bookkeeping for the UI's surrogate management. They are explicitly filtered out by report generators (`_extract_overrides` excludes keys starting with `__surrogate__`) and are not read by any calculation service.
