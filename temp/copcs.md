# COPCs Screen

## User's Guide Content

### Purpose

The Chemicals of Potential Concern (COPCs) screen is where you select which chemicals are present at your site for each exposure medium. The chemicals you add here carry forward to every subsequent step — they determine which rows appear on the EPC entry screen, which toxicity values are loaded, and which chemicals are included in risk calculations and reports.

### Layout

The screen uses a split-panel design:

- **Left sidebar** — Lists each exposure medium from your Site Model selections, plus an "All Media" summary entry at the bottom. Click a medium to view and manage its chemicals.
- **Right panel** — Contains the search bar, action buttons, and the chemical list for the selected medium.

### Controls

#### Media Sidebar

Each medium from the Site Model is listed with a badge showing the count of chemicals assigned to it. Click a medium to manage its chemical list. The **All Media** entry at the bottom shows a consolidated view of all chemicals across all media, with color-coded tags indicating which media each chemical belongs to.

#### Search Bar

At the top of the right panel, type a chemical name or CAS number to search the reference database. Results appear inline, grouped by chemical class, with the most relevant matches first.

- **Exact name matches** appear first, followed by name-starts-with, then CAS matches.
- Already-added chemicals show a green "Added" badge.
- Click **+ Add** to add a chemical to the current medium.
- Press **Escape** to clear the search and return to the chemical list.

The search bar is hidden on the All Media summary tab (you can only add chemicals from an individual medium tab).

#### Action Bar

When viewing an individual medium:

| Control | Description |
|---|---|
| **Copy from** buttons | Appear when other media already have chemicals. Click to copy all chemicals from another medium into the current one. Only new chemicals are added (duplicates are skipped). |
| **Clear all** | Removes all chemicals from the current medium. |

#### Chemical List

Selected chemicals are displayed grouped by chemical class (e.g., Metals, Volatiles, PAHs), with each group in a collapsible card showing a count badge. Within each group, chemicals are sorted alphabetically. Each chemical row shows:

| Element | Description |
|---|---|
| Chemical name | The analyte name from the database. |
| CAS number | The Chemical Abstracts Service registry number (monospace font). |
| Media dots | Small colored dots indicating which other media this chemical is also assigned to. Filled dots = present, empty dots = not present. |
| **×** button | Click to remove this chemical from the current medium. |

#### All Media Summary

The summary view shows every unique chemical across all media, grouped by chemical class. Each chemical row displays color-coded tags for every medium it belongs to. This view is read-only — to add or remove chemicals, switch to an individual medium tab.

#### Footer

A summary line at the bottom shows the total count of unique chemicals across all media (e.g., "12 unique COPCs selected").

### Behavior

- **Immediate save:** Every add, remove, copy, or clear action is saved to ProjectState immediately.
- **Media tabs from Site Model:** The media listed in the sidebar come directly from the Site Model screen. If you change your Site Model selections, the media tabs here update when you next visit this screen.
- **Orphan preservation:** If you remove a medium from the Site Model and later re-add it, any chemicals previously assigned to that medium are restored automatically.
- **Toxicity cleanup:** When a chemical is removed from all media (not just one), any associated toxicity value overrides and surrogate markers are cleaned up from ProjectState.
- **Chemical database:** The full chemical list is loaded from the ARC reference database. Chemicals are organized by class and searchable by name or CAS number.

### When to Use

Fill out this screen after completing the Site Model. You must select at least one medium and one pathway in the Site Model before media tabs will appear here.

The chemicals you select drive the following downstream screens:
- **EPCs** — Concentration entry rows correspond to the chemicals selected here, per medium.
- **Toxicity Values** — Toxicity reference values are loaded for each selected chemical.
- **Risk Assessment** — Only selected chemicals are included in risk calculations.
- **Reports** — Only selected chemicals appear in generated reports.

---

## Field Verification

**Field coverage: All selections verified.** The screen stores a single key `copcs.by_medium` mapping each medium name to a list of CAS numbers. Values persist across navigation and save/load.

### Data Structure

```
state.copcs["by_medium"] = {
    "Surface Soil": ["7440-38-2", "7440-43-9", "71-43-2", ...],
    "Tap Water": ["7440-38-2", "7440-39-3", ...],
    ...
}
```

### Read/Write Verification

| Aspect | Mechanism | Location |
|---|---|---|
| **Write** | `_save_to_state()` writes `by_medium` dict via `state.set("copcs", "by_medium", by_medium)`. Connected to `data_changed` signal, so fires on every add/remove/copy/clear. | line 1297-1322 |
| **Write (sync)** | `sync_to_project_state()` calls `_save_to_state()` — also fires before save and on navigation away. | line 1332-1334 |
| **Read** | `_do_populate()` reads `copc_data.get("by_medium", {})` and restores per-medium CAS sets. | lines 1288-1292 |
| **Round-trip** | Yes — immediate write on every change; full restore on load/navigate. | |
| **Orphan handling** | `_save_to_state()` starts from existing `by_medium` to preserve orphaned media entries, only overwriting active media. | lines 1306-1312 |

### Additional state interaction

| Action | Side Effect |
|---|---|
| Remove chemical from all media | Cleans up orphaned keys in `toxicity_values` section (overrides and surrogate markers with the removed CAS prefix). Lines 1174-1186. |
| Load media tabs | Reads `conceptual_model.scenarios` to determine which media to show (via `extract_unique_media`). Falls back to `copcs.by_medium` keys if scenarios are empty. Lines 385-409. |

---

## Dependencies

### Upstream

| Source | What's Used | Purpose |
|---|---|---|
| [site_model.py](../views/site_model.py) — `conceptual_model.scenarios` | Media names (via `extract_unique_media`) | Determines which medium tabs appear in the sidebar. If no scenarios exist, falls back to existing `copcs.by_medium` keys. |
| Reference database (`arc.db`) | Full chemical list and chemical classes | Populates the search database and category groupings. Loaded once on screen init via `ARCService.get_all_chemicals()` and `get_chemical_classes()`. |

### Downstream

| Consumer | What It Reads | Purpose |
|---|---|---|
| [base.py](../../core/calculations/service/base.py) — `_get_chemicals_for_medium()` | `by_medium[medium]` (CAS list) | Retrieves the list of chemicals for a given medium. Base class method used by all receptor calculation services. |
| [epcs.py](../views/epcs.py) — `_populate_from_state()` | `by_medium` | Determines which chemical rows appear in each medium's EPC entry panel. |
| [toxicity_values.py](../views/toxicity_values.py) — `_load_copcs()` | `by_medium` (all values unioned) | Collects all unique CAS numbers to load corresponding toxicity reference values. |
| [risk_assessment.py](../views/risk_assessment.py) | `by_medium[medium]` + cache invalidation | Determines which chemicals to include in risk calculations for each medium. Changes to `copcs` invalidate the calculation cache. |
| [generator.py](../../reports/generator.py) — `_extract_copcs()` | `by_medium` (all values unioned) | Collects all unique CAS numbers for report generation. Looks up full chemical data from the database. |
