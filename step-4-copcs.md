---
title: "Step 4: COPCs"
layout: default
nav_order: 9
---

# Step 4: Chemicals of Potential Concern (COPCs)

The COPCs screen is where you select which chemicals are present at your site for each exposure medium. The chemicals you add here carry forward to every subsequent step — they determine which rows appear on the EPC entry screen, which toxicity values are loaded, and which chemicals are included in risk calculations and reports.

## Layout

The screen uses a split-panel design:

- **Left sidebar** — Lists each exposure medium from your Site Model selections, plus an "All Media" summary entry at the bottom. Click a medium to view and manage its chemicals.
- **Right panel** — Contains the search bar, action buttons, and the chemical list for the selected medium.

## Controls

### Media Sidebar

Each medium from the Site Model is listed with a badge showing the count of chemicals assigned to it. Click a medium to manage its chemical list. The **All Media** entry at the bottom shows a consolidated view of all chemicals across all media, with color-coded tags indicating which media each chemical belongs to.

### Search Bar

At the top of the right panel, type a chemical name or CAS number to search the reference database. Results appear inline, grouped by chemical class, with the most relevant matches first.

- **Exact name matches** appear first, followed by name-starts-with, then CAS matches.
- Already-added chemicals show a green "Added" badge.
- Click **+ Add** to add a chemical to the current medium.
- Press **Escape** to clear the search and return to the chemical list.

The search bar is hidden on the All Media summary tab (you can only add chemicals from an individual medium tab).

### Action Bar

When viewing an individual medium:

| Control | Description |
|---|---|
| **Copy from** buttons | Appear when other media already have chemicals. Click to copy all chemicals from another medium into the current one. Only new chemicals are added (duplicates are skipped). |
| **Clear all** | Removes all chemicals from the current medium. |

### Chemical List

Selected chemicals are displayed grouped by chemical class (e.g., Metals, Volatiles, PAHs), with each group in a collapsible card showing a count badge. Within each group, chemicals are sorted alphabetically. Each chemical row shows:

| Element | Description |
|---|---|
| Chemical name | The analyte name from the database. |
| CAS number | The Chemical Abstracts Service registry number. |
| Media dots | Small colored dots indicating which other media this chemical is also assigned to. Filled dots = present, empty dots = not present. |
| **x** button | Click to remove this chemical from the current medium. |

### All Media Summary

The summary view shows every unique chemical across all media, grouped by chemical class. Each chemical row displays color-coded tags for every medium it belongs to. This view is read-only — to add or remove chemicals, switch to an individual medium tab.

### Footer

A summary line at the bottom shows the total count of unique chemicals across all media (e.g., "12 unique COPCs selected").

## Behavior

- **Immediate save:** Every add, remove, copy, or clear action is saved immediately.
- **Media tabs from Site Model:** The media listed in the sidebar come directly from the Site Model screen. If you change your Site Model selections, the media tabs here update when you next visit this screen.
- **Orphan preservation:** If you remove a medium from the Site Model and later re-add it, any chemicals previously assigned to that medium are restored automatically.
- **Toxicity cleanup:** When a chemical is removed from all media (not just one), any associated toxicity value overrides and surrogate markers are cleaned up automatically.
- **Chemical database:** The full chemical list is loaded from the ARC reference database. Chemicals are organized by class and searchable by name or CAS number.

## When to Use

Fill out this screen after completing the Site Model. You must select at least one medium and one pathway in the Site Model before media tabs will appear here.

The chemicals you select drive the following downstream screens:
- **EPCs** — Concentration entry rows correspond to the chemicals selected here, per medium.
- **Toxicity Values** — Toxicity reference values are loaded for each selected chemical.
- **Risk Assessment** — Only selected chemicals are included in risk calculations.
- **Reports** — Only selected chemicals appear in generated reports.
