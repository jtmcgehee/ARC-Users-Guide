---
title: "Step 7: Toxicity Values"
layout: default
nav_order: 12
---

# Step 7: Toxicity Values

This screen shows the toxicity reference values ARC uses to calculate health risks — reference doses, cancer slope factors, inhalation unit risks, etc. Values come from EPA and other regulatory databases. You can review the defaults, apply overrides, and assign surrogate chemicals when a chemical lacks its own toxicity data.

## Layout

- **Left sidebar** — All chemicals selected on the COPCs screen, grouped by class. Click a chemical to view its values.
- **Middle panel** — Toxicity values organized by section. Each parameter is a clickable cell. A "Use Surrogate" button appears in the header.
- **Right panel** — Appears when you click a cell. Shows default value and metadata, and provides an override entry form.

## Parameters

| Section | Parameters | Description |
|---|---|---|
| **Carcinogenic** | CSFo, IUR | Cancer Slope Factor (oral) and Inhalation Unit Risk. |
| **Non-Carcinogenic (Oral/Dermal)** | RfDo, Dermal RfD, SRfDo, Dermal RfD_sc | Reference Dose (chronic and subchronic). Dermal values are calculated automatically. |
| **Non-Carcinogenic (Inhalation)** | RfC, SRfC | Reference Concentration (chronic and subchronic). |
| **Bioavailability** | GIABS, ABSd | GI Absorption Factor and Dermal Absorption Factor. |
| **Acute** | ARfDo, Dermal RfD_acute, ARfC | Acute reference values for short-duration exposures. |
| **Short-term** | STRfDo, Dermal RfD_st, STRfC | Short-term reference values. |

### Calculated Parameters

These are derived automatically and can't be edited directly:

| Parameter | Formula |
|---|---|
| Dermal RfD | RfDo x GIABS |
| Dermal RfD_sc | SRfDo x GIABS |
| Dermal RfD_acute | ARfDo x GIABS |
| Dermal RfD_st | STRfDo x GIABS |

These update live when you change RfDo, SRfDo, ARfDo, STRfDo, or GIABS.

When a chemical has no subchronic value in the database, ARC uses the chronic value as a fallback, marked with a dagger in the grid.

## Overriding a Default

1. Click a chemical, then click an editable cell.
2. Fill in the required fields:
   - **Value** and **Source** (always required).
   - For carcinogenic parameters: **Tumor Site** is also required.
   - For non-carcinogenic parameters: **Target Organ**, **UF**, **MF**, and **Confidence** are required (enter "N/A" for any that don't apply).
3. Click **Apply**. The cell turns blue.
4. To revert, click the cell and click **Reset to Default**.

## Surrogates

When a chemical lacks toxicity data, you can borrow values from a similar compound:

1. Click the chemical, then click **Use Surrogate**.
2. Search for and select a surrogate chemical. The dialog shows a side-by-side comparison.
3. Toggle which parameters to apply from the surrogate.
4. Click **Apply**.

To modify or remove a surrogate, click **Edit Surrogate** (the button text changes after assignment). Surrogate-applied values show a "From Surrogate" label in the detail panel.

## Cell Colors

| Color | Meaning |
|---|---|
| White | Database default |
| Blue | Override or surrogate value applied |
| Gray italic | Calculated value (Dermal RfD variants) |
| Gray with "--" | No value available in the database |
