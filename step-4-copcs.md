---
title: "Step 4: COPCs"
layout: default
nav_order: 9
---

# Step 4: Chemicals of Potential Concern (COPCs)

This is where you select which chemicals are present at your site for each exposure medium. The chemicals you add here carry forward to every subsequent step — EPCs, toxicity values, risk calculations, and reports.

## Layout

- **Left sidebar** — Lists each exposure medium from the Site Model, plus an "All Media" summary at the bottom. A badge shows the chemical count per medium.
- **Right panel** — Search bar, action buttons, and the chemical list for the selected medium.

## Adding Chemicals

Type a chemical name or CAS number in the search bar. Results appear grouped by chemical class, with exact name matches first. Click **+ Add** to add a chemical to the current medium. Already-added chemicals show a green "Added" badge.

The search bar is hidden on the All Media tab.

## Action Bar

| Control | Description |
|---|---|
| **Copy from** buttons | Copy all chemicals from another medium into the current one. Duplicates are skipped. |
| **Clear all** | Remove all chemicals from the current medium. |

## Chemical List

Chemicals are grouped by class (Metals, Volatiles, PAHs, etc.) in collapsible cards. Each row shows the chemical name, CAS number, colored dots indicating which other media include that chemical, and an **x** button to remove it.

## All Media Summary

The All Media tab shows every unique chemical across all media with color-coded tags for each medium. This view is read-only — add or remove chemicals from individual medium tabs.

A footer shows the total unique COPC count.

## Notes

- You must have at least one medium and pathway selected in the Site Model before media tabs appear here.
- If you remove a medium from the Site Model and later re-add it, previously assigned chemicals are restored.
- Removing a chemical from all media cleans up any associated toxicity value overrides and surrogate assignments.
