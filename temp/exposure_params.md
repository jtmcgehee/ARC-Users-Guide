# Exposure Parameters Screen

## User's Guide Content

### Purpose

The Exposure Parameters screen displays the numeric assumptions that drive the risk calculations — values like body weight, exposure duration, ingestion rates, and skin surface area. Every receptor/medium combination in your Site Model has a set of these parameters, and ARC pre-loads EPA default values from its reference database. This screen lets you review those defaults and, where your site conditions differ, enter site-specific overrides.

### Layout

The screen uses a three-panel design:

- **Left sidebar** — Lists each receptor that has active selections in the Site Model (e.g., Resident, Outdoor Worker, Construction Worker). Click a receptor to view its parameter grids.
- **Middle panel** — One or more scrollable parameter grids for the selected receptor, grouped by exposure medium category (e.g., "Exposure to Soil", "Exposure to Tapwater", "Exposure to Vapor Intrusion to Indoor Air"). Each grid is a matrix of age bins (rows) vs. parameters (columns).
- **Right panel** — A detail/override panel that appears when you click a cell. Shows the parameter name, description, age bin, default value and source, and provides an entry form to apply a site-specific override or reset to the default.

### Parameters

Parameters vary by receptor and medium. Common parameters include:

| Symbol | Description | Typical Units |
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

Not every parameter appears for every receptor. For example, Construction Worker and Utility Worker include **EW** (Weeks Worked per Year), while Resident and Recreator do not. Indoor Worker has no dermal parameters (AF, SAs). The parameter set shown for each receptor is determined by the calculation equations that apply to it.

### Age Bins

For age-stratified receptors (Recreator, Trespasser), parameters are broken out by age bin:

| Age Bin | Editable | Description |
|---|---|---|
| **0–2** | Yes | Infant age range |
| **2–6** | Yes | Young child age range |
| **6–16** | Yes | Adolescent age range |
| **16–26** | Yes | Young adult age range |
| **child** | No (calculated) | Weighted aggregate of 0–2 and 2–6 |
| **adult** | No (calculated) | Weighted aggregate of 6–16 and 16–26 |

The **child** and **adult** rows are read-only calculated values displayed in gray italic cells. For ED (Exposure Duration), the child/adult values are the sum of their constituent age bins. For all other parameters, the child/adult values are ED-weighted averages of their constituent age bins.

For non-age-stratified receptors (Resident, Indoor Worker, Outdoor Worker, Composite Worker, Construction Worker, Utility Worker), there is a single "adult" row with no sub-bins.

### Medium Categories

Multiple Site Model media that share the same exposure equations are grouped under a single grid section:

| Grid Section | Site Model Media Included |
|---|---|
| Soil | Surface Soil, Subsurface Soil, Combined Soil |
| Tapwater | Tapwater |
| Surface Water | Surface Water |
| Vapor Intrusion to Indoor Air | All vapor intrusion pathways (Groundwater VI, Soil Gas VI, Trench VI) |

### How to Override a Default

1. Click any editable cell in a parameter grid. The right panel shows the default value and its source.
2. In the "Enter Site-Specific Value" section, fill in:
   - **Value** (required) — The numeric site-specific value.
   - **Source** (required) — A citation or justification for the override.
   - **Footnote** (optional) — An additional note.
3. Click **Apply**. The cell turns blue to indicate an active override.
4. To revert an override, click the overridden cell and click **Reset to Default** in the right panel.

Both Value and Source are required — ARC will show a warning if either is blank when you click Apply.

### Visual Indicators

| Element | Description |
|---|---|
| **White cell** | Using the EPA default value from the database. |
| **Blue cell** | A site-specific override has been applied. Blue border and light blue background. |
| **Selected cell** | Thick blue border on the currently selected cell. |
| **Gray italic cell** | Calculated aggregate (child/adult) — read-only. |
| **Dash (-)** | No value available in the database for this parameter/age bin combination. |

### Behavior

- **Defaults from database:** All default values are loaded from the ARC reference database (`arc.db`). These are EPA-recommended values and cannot be modified — the database is read-only.
- **Overrides only:** ARC does not store every parameter value — it only stores your site-specific overrides. If no override exists, the default is used in calculations.
- **Immediate save:** Every override you apply is written to ProjectState immediately. Clicking Reset to Default removes the override immediately. There is no separate Save step. The `sync_to_project_state()` method is a no-op because state is always current.
- **Receptors from Site Model:** Only receptors with active media/pathway selections in the Site Model appear in the sidebar. If no receptors are selected, a message directs you to the Site Model screen.
- **Medium collapsing:** Multiple Site Model media that share the same calculation parameters (e.g., Surface Soil and Subsurface Soil) appear as a single grid section rather than being repeated.
- **Calculated aggregates update live:** When you override an editable age bin (e.g., 0–2), the child and adult calculated values update immediately in the grid.

### When to Use

Fill out this screen after completing the Site Model. You need at least one receptor with media/pathway selections before parameters will appear.

In most cases, the EPA defaults will be appropriate and no overrides are needed. Use this screen when site-specific conditions justify different assumptions — for example, a shorter exposure duration, a different body weight, or a site-specific ingestion rate.

The values set here (defaults or overrides) are used directly in the risk calculations for every selected receptor.

---

## Field Verification

**Field coverage: All fields verified.** Every user override is read from and written to `state.exposure_params`. Overrides persist across navigation and save/load. Default values are sourced from the database on every populate and are not stored in ProjectState.

### Data Structure

```
state.exposure_params = {
    "Resident|soil|AF|0-2": {
        "value": "0.2",
        "source": "Site-specific study (2025)",
        "footnote": "Based on clay soil conditions"
    },
    "Resident|tapwater|IRW|adult": {
        "value": "2.5",
        "source": "Regional water survey",
        "footnote": ""
    },
    ...
}
```

Each key is a pipe-delimited composite: `receptor|media_category|base_symbol|age_bin`. Values are dicts with `value` (string), `source` (string), and `footnote` (string). Only overridden parameters have entries — unmodified defaults are not stored.

### Read/Write Verification

| Aspect | Mechanism | Location |
|---|---|---|
| **Write (apply)** | `_on_override_applied()` writes via `state.set("exposure_params", key, override_data)`. Fires immediately when user clicks Apply. | line 1367 |
| **Write (reset)** | `_on_reset_cell()` removes via `state.unset("exposure_params", key)`. Fires immediately when user clicks Reset to Default. | line 1383 |
| **Read** | `_do_populate()` reads `state.get_section("exposure_params")` and calls `panel.load_overrides(overrides_section)` for each receptor panel. Each section's `load_overrides()` applies stored values to matching cells. | lines 1457-1460 |
| **Round-trip** | Yes — immediate write on apply, immediate delete on reset; full restore on load/navigate. | |
| **Sync** | `sync_to_project_state()` is a no-op (line 1465-1471) — state is always current because overrides are written on every Apply. | |

### Per-override field mapping

| UI Element | Stored As | Read In | Written In |
|---|---|---|---|
| Value input | `overrides[key]["value"]` (string) | `ParameterCell.set_value()` via `MediaCategorySection.load_overrides()` | `DetailPanel._on_apply()` → `override_applied` signal |
| Source input | `overrides[key]["source"]` (string) | `DetailPanel.show_param()` reads from `state.get("exposure_params", key)` | `DetailPanel._on_apply()` → `override_applied` signal |
| Footnote input | `overrides[key]["footnote"]` (string) | `DetailPanel.show_param()` reads from `state.get("exposure_params", key)` | `DetailPanel._on_apply()` → `override_applied` signal |

---

## Dependencies

### Upstream

| Source | What's Used | Purpose |
|---|---|---|
| [site_model.py](../views/site_model.py) — `conceptual_model.scenarios` | Receptor names and media selections (via `extract_receptor_selections` and `get_base_receptor_name`) | Determines which receptors appear in the sidebar and which medium category grids are shown for each receptor. If no scenarios exist, the screen shows an empty message. |
| [param_config.py](../../core/calculations/param_config.py) — `RECEPTOR_PARAMS`, `MEDIUM_TO_CONFIG_KEY`, `CONFIG_KEY_DISPLAY_NAME` | Parameter definitions per receptor per medium category | Determines which parameters (symbols, units, descriptions) appear in each grid section, and maps Site Model medium names to config keys. |
| Reference database (`arc.db`) | Default parameter values, sources, and metadata | Loaded via `ARCService.get_exposure_params()` and `get_exposure_param_value()`. Provides the EPA default values displayed in cells and the detail panel. |

### Downstream

| Consumer | What It Reads | Purpose |
|---|---|---|
| [base.py](../../core/calculations/service/base.py) — `_build_overrides()` | `state.exposure_params` — all keys matching the receptor being calculated | Builds an override lookup `{(receptor, db_symbol): float_value}` by parsing the pipe-delimited keys. Overrides take precedence over database defaults in `get_param()`. Used by all 8 receptor calculation services (Residential, Indoor Worker, Outdoor Worker, Composite Worker, Recreator, Trespasser, Construction Worker, Utility Worker). |
| [risk_assessment.py](../views/risk_assessment.py) — cache invalidation | `exposure_params` section membership in `_CACHE_INVALIDATING_SECTIONS` | Any change to `exposure_params` (override applied or reset) invalidates the calculation cache, triggering recalculation on next view. |

**Not read downstream:** The `source` and `footnote` fields on each override are stored in ProjectState but are not currently consumed by any calculation service or report generator. They serve as documentation for the user's records.
