# Project Info Screen

## User's Guide Content

### Purpose

The Project Information screen is the first screen you see when starting a new project in ARC. It captures the basic identifying details for your risk assessment — who the client is, what the site is called, and where it is located. This information appears in the headers of generated reports and is saved with your project file.

### Fields

The screen is organized into two sections:

#### Project Details

| Field | Required | Description |
|---|---|---|
| **Client Name** | Yes | The name of the client or organization commissioning the risk assessment. |
| **Project Number** | No | Your internal project or task number (e.g., PRJ-2026-001). Used for your records only. |
| **Site Name** | Yes | The name of the site being assessed. This is also used as the suggested filename when saving the project. |

#### Site Location

| Field | Required | Description |
|---|---|---|
| **Street Address** | Yes | The street address of the site. Appears in report headers. |
| **City** | Yes | The city where the site is located (e.g., Durham). Appears in report headers. |
| **State** | Yes | The state where the site is located (e.g., North Carolina). Appears in report headers. |
| **ZIP Code** | Yes | The site's ZIP code (e.g., 27713). Appears in report headers. |

Below the location fields, two read-only labels display **Date Created** and **Last Modified** timestamps. These are set automatically and cannot be edited.

### Behavior

- **Required fields:** Client Name, Site Name, Street Address, City, State, and ZIP Code are all marked with a red asterisk (`*`). These fields must be filled in before reports can be generated — if any are missing, the Reports screen will show a message listing the incomplete fields. Project Number is the only optional field.
- **Auto-save:** Every change you make to a field is saved to the project automatically (once the project has been saved at least once). You do not need to press a Save button after each edit.
- **Dates:** Date Created is set when the project is first created. Last Modified updates each time the project is saved. Both are displayed in a human-readable format (e.g., "February 17, 2026" or "February 17, 2026 at 02:30 PM").
- **Navigation persistence:** If you navigate to another screen and come back, all values you entered will still be there.

### When to Use

This is typically the first screen you fill out when starting a new risk assessment. All fields except Project Number must be completed before you can generate reports.

No data from other screens is needed before filling out this screen.

---

## Field Verification

**Field coverage: All 7 fields verified.** Every user-editable field is read from and written to ProjectState. Values persist across navigation and save/load.

| # | Field Key | Storage Location | Read on Load | Write on Change | Round-Trip |
|---|---|---|---|---|---|
| 1 | `client` | `ProjectState.client` (core attribute) | `getattr(state, "client")` in `_populate_from_state()` | `setattr(state, "client", value)` in `_on_field_changed()` | Yes |
| 2 | `project_number` | `state.project_info["project_number"]` | `state.get("project_info", "project_number")` in `_populate_from_state()` | `state.set("project_info", "project_number", value)` in `_on_field_changed()` | Yes |
| 3 | `site_name` | `ProjectState.site_name` (core attribute) | `getattr(state, "site_name")` in `_populate_from_state()` | `setattr(state, "site_name", value)` in `_on_field_changed()` | Yes |
| 4 | `site_address` | `state.project_info["site_address"]` | `state.get("project_info", "site_address")` in `_populate_from_state()` | `state.set("project_info", "site_address", value)` in `_on_field_changed()` | Yes |
| 5 | `city` | `state.project_info["city"]` | `state.get("project_info", "city")` in `_populate_from_state()` | `state.set("project_info", "city", value)` in `_on_field_changed()` | Yes |
| 6 | `state` | `state.project_info["state"]` | `state.get("project_info", "state")` in `_populate_from_state()` | `state.set("project_info", "state", value)` in `_on_field_changed()` | Yes |
| 7 | `zip_code` | `state.project_info["zip_code"]` | `state.get("project_info", "zip_code")` in `_populate_from_state()` | `state.set("project_info", "zip_code", value)` in `_on_field_changed()` | Yes |

**Additional detail:**
- The `_updating_from_state` guard flag prevents write-back loops when populating fields from state.
- `sync_to_project_state()` provides a bulk flush (used before save) that writes all 7 fields with signal suppression.
- `refresh_from_state()` delegates to `_populate_from_state()` for navigation-triggered reloads.

---

## Dependencies

### Upstream

None. This is the first screen in the workflow and does not depend on data from any other screen.

### Downstream

Three modules read data originally entered on this screen:

| Consumer | Fields Used | How |
|---|---|---|
| [generator.py](../../reports/generator.py) — `_extract_site_info()` | `site_name`, `site_address`, `city`, `state`, `zip_code` | Reads `project_state.site_name` (core attr) and `project_state.get_section("project_info")` to build report headers. Combines city, state, and zip into a single "City, State ZIP" line. |
| [reports.py](../views/reports.py) | `site_name` | Reads `project_state.site_name` to suggest a default filename for exported report files (e.g., `Site_Name_toxicity_tables.xlsx`). |
| [main_window.py](../main_window.py) | `site_name` | Reads `project_state.site_name` to suggest a default `.arc` filename when using Save As. |

**Not read downstream:** `client` and `project_number` are stored but not currently consumed by any other screen or report module.
