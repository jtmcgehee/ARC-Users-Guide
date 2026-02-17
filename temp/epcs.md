# EPCs Screen

## User's Guide Content

### Purpose

The Exposure Point Concentrations (EPCs) screen is where you enter the measured or estimated concentrations of each chemical at your site, organized by exposure medium. These concentration values are the core numerical inputs to the risk assessment — they feed directly into the risk calculations for every receptor and pathway.

### Layout

The screen uses a three-panel design:

- **Left sidebar** — Lists each exposure medium from your Site Model. Click a medium to view its concentration entry table. A badge shows the number of chemicals assigned to that medium (from COPCs).
- **Middle panel** — A scrollable table where you enter concentrations for each chemical in the selected medium. Chemicals are grouped by class (e.g., Metals, Volatiles, PAHs).
- **Right sidebar** — A progress tracker showing how many concentrations you've entered per chemical class, with visual progress bars and an overall completion count.

### Fields

For each chemical in each medium, you enter two values:

| Field | Type | Description |
|---|---|---|
| **Concentration** | Numeric input | The measured or estimated concentration of this chemical. Units are determined by the medium type and shown in the column header (see table below). Leave blank if no data is available for this chemical. |
| **Statistic** | Dropdown | The statistical measure the concentration represents. Options: **Maximum** (default) or **95% UCL** (95% Upper Confidence Limit). |

#### Units by Medium

| Medium | Unit |
|---|---|
| Surface Soil, Subsurface Soil, Combined Soil, Sediment | mg/kg |
| Groundwater, Surface Water, Tapwater | ug/L |
| Indoor Air, Ambient Air, Sub-slab Soil Gas, Soil Gas | ug/m³ |

### Visual Indicators

| Element | Description |
|---|---|
| **Green dot** | Appears next to chemicals that have a concentration entered. Empty/gray dot means no value yet. |
| **Category headers** | Colored bars with a left accent separate chemicals by class (e.g., "Metals (5)"). |
| **Progress bars** | The right sidebar shows a bar per category indicating how many chemicals have concentrations entered (e.g., "3/5"). Bars turn green when a category is complete. |
| **Overall counter** | At the bottom of the progress sidebar, a large number shows total entered vs. total chemicals. |
| **Sidebar badges** | The left media sidebar shows the COPC count per medium. |

### Behavior

- **Immediate save:** Every concentration entry or statistic change is written to ProjectState immediately.
- **Chemical rows from COPCs:** The chemicals shown in each medium's table come from your COPC selections. If you add or remove chemicals on the COPCs screen, the rows here update accordingly on your next visit.
- **Media tabs from Site Model:** The media listed in the sidebar come from the Site Model. If no media are selected, a message directs you to the Site Model screen.
- **No COPCs message:** If media are selected but no chemicals have been added, a message directs you to the COPCs screen.
- **Validation:** The concentration input accepts only non-negative numeric values (validated by a floating-point validator). No upper limit is enforced.
- **Default statistic:** New rows default to "Maximum" as the statistic.
- **Blank = no data:** Leaving a concentration field blank means no concentration data is available for that chemical in that medium. Blank entries are excluded from saved data and downstream calculations.

### When to Use

Fill out this screen after selecting chemicals on the COPCs screen. You need:
1. At least one medium selected in the Site Model.
2. At least one chemical assigned to that medium on the COPCs screen.

The concentration values you enter here are used by:
- **Risk Assessment** — Concentrations are multiplied by intake factors and toxicity values to calculate hazard quotients and cancer risks.
- **Reports** — Concentration values appear in generated report tables.

---

## Field Verification

**Field coverage: All fields verified.** Every concentration value and statistic selection is read from and written to `state.epcs["by_medium"]`. Values persist across navigation and save/load.

### Data Structure

```
state.epcs["by_medium"] = {
    "Surface Soil": {
        "values": [
            {
                "cas_number": "7440-38-2",
                "analyte": "Arsenic",
                "concentration": 12.5,
                "statistic": "Maximum"
            },
            ...
        ]
    },
    "Tap Water": {
        "values": [...]
    },
    ...
}
```

Concentrations are stored as floats. Only rows with a non-empty concentration are included in the `values` list — blank rows are omitted.

### Read/Write Verification

| Aspect | Mechanism | Location |
|---|---|---|
| **Write** | `_save_to_state()` iterates all `MediaEPCPanel`s, calls `panel.get_data()` to collect `{"values": [...]}` per medium, writes via `state.set("epcs", "by_medium", by_medium)`. Connected to `data_changed` signal — fires on every input change. | lines 967-974 |
| **Write (sync)** | `sync_to_project_state()` calls `_save_to_state()` — also fires before save and on navigation away. | lines 1080-1084 |
| **Read** | `_do_populate()` reads `epcs_section.get("by_medium", {})`, then calls `panel.load_data(by_medium[media])` for each medium panel, which restores concentration and statistic values per CAS number. | lines 1012-1017 |
| **Round-trip** | Yes — immediate write on every change; full restore on load/navigate. | |

### Per-row field mapping

| UI Element | Stored As | Read In | Written In |
|---|---|---|---|
| Concentration input | `values[i]["concentration"]` (float) | `ChemicalRow.set_data()` via `panel.load_data()` | `ChemicalRow.get_data()` via `panel.get_data()` |
| Statistic dropdown | `values[i]["statistic"]` (string: "Maximum" or "95% UCL") | `ChemicalRow.set_data()` via `panel.load_data()` | `ChemicalRow.get_data()` via `panel.get_data()` |
| Chemical identity | `values[i]["cas_number"]`, `values[i]["analyte"]` | Used as row key for matching on load | Included in saved data for downstream consumers |

---

## Dependencies

### Upstream

| Source | What's Used | Purpose |
|---|---|---|
| [site_model.py](../views/site_model.py) — `conceptual_model.scenarios` | Media names (via `extract_unique_media`) | Determines which medium tabs appear in the sidebar. |
| [copcs.py](../views/copcs.py) — `copcs.by_medium` | CAS number lists per medium | Determines which chemical rows appear in each medium's table. Chemical metadata (name, class) is looked up from the database by CAS number. |
| Reference database (`arc.db`) | Chemical metadata | Maps CAS numbers to analyte names and chemical classes for display and grouping. Results are cached in `_chem_cache` to avoid repeated DB queries. |

### Downstream

| Consumer | What It Reads | Purpose |
|---|---|---|
| [base.py](../../core/calculations/service/base.py) — `_get_chemicals_for_medium()` | `by_medium[medium]["values"]` — specifically `cas_number` and `concentration` | Retrieves concentration values for each chemical in a medium. This base class method is inherited by all receptor calculation services (Residential, Outdoor Worker, etc.). |
| [risk_assessment.py](../views/risk_assessment.py) | `by_medium[medium]["values"]` — `cas_number`, `analyte`, `concentration`, `statistic` | Reads EPC data for building risk display tables. Also listed in `_CACHE_INVALIDATING_SECTIONS` — any change to `epcs` triggers recalculation. |
