---
title: "Step 7: Toxicity Values"
layout: default
nav_order: 12
---

# Step 7: Toxicity Values

The Toxicity Values screen displays the toxicity reference values that ARC uses to calculate health risks for each chemical at your site. These values — such as reference doses, cancer slope factors, and inhalation unit risks — come from EPA and other regulatory databases. This screen lets you review the defaults, apply site-specific overrides, and assign surrogate chemicals when a chemical lacks its own toxicity data.

## Layout

The screen uses a three-panel design:

- **Left sidebar** — Lists all chemicals selected across all media on the COPCs screen, grouped by chemical class (e.g., Metals, Volatiles, PAHs). Click a chemical to view its toxicity values.
- **Middle panel** — Displays the toxicity values for the selected chemical, organized into labeled sections. Each parameter is shown as a clickable cell with a value, label, and units. A "Use Surrogate" button appears in the header.
- **Right panel** — A detail/override panel that appears when you click a cell. Shows the default value and metadata, provides an entry form for manual overrides, and displays surrogate information when applicable.

## Parameters

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

### Units

| Parameter | Units |
|---|---|
| CSFo | (mg/kg-day)⁻¹ |
| IUR | (ug/m³)⁻¹ |
| RfDo, SRfDo, ARfDo, STRfDo, Dermal RfD variants | mg/kg-day |
| RfC, SRfC, ARfC, STRfC | mg/m³ |
| GIABS, ABSd | unitless |

### Calculated Parameters

Four parameters are **calculated automatically** and cannot be directly edited:

| Parameter | Formula |
|---|---|
| **Dermal RfD** | RfDo x GIABS |
| **Dermal RfD_sc** (subchronic) | SRfDo x GIABS (uses chronic RfDo as fallback if SRfDo is unavailable) |
| **Dermal RfD_acute** | ARfDo x GIABS |
| **Dermal RfD_st** (short-term) | STRfDo x GIABS |

These update live when you override RfDo, SRfDo, ARfDo, STRfDo, or GIABS.

### Subchronic Fallback

When a chemical has no subchronic value (SRfDo or SRfC) in the database, ARC automatically uses the chronic value (RfDo or RfC) as a surrogate. These fallback values are marked with a dagger symbol in the grid and a footnote appears at the bottom: "Chronic value used as surrogate for subchronic."

## How to Override a Default

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

## How Surrogates Work

When a chemical lacks toxicity data, you can borrow values from a chemically similar compound:

1. Click the chemical in the left sidebar.
2. Click the **Use Surrogate** button in the middle panel header.
3. In the Surrogate Selection Dialog:
   - Search for and select a surrogate chemical from the full database.
   - The dialog shows a side-by-side comparison: the current chemical's values vs. the surrogate's values.
   - Toggle on/off which individual parameters to apply from the surrogate (e.g., apply CSFo but not RfDo).
4. Click **Apply**. The selected parameters are copied from the surrogate and stored as overrides.
5. To modify or remove the surrogate, click **Edit Surrogate** (the button text changes after a surrogate is applied).
6. In the detail panel, surrogate-applied values show a "From Surrogate" label with the surrogate chemical name.

Surrogate overrides behave like manual overrides in calculations. However, in the UI, surrogate overrides cannot be reset individually using the Reset button; you must use the Edit Surrogate button to clear them.

## Visual Indicators

| Element | Description |
|---|---|
| **White cell** | Using the database default value. |
| **Blue cell** | A manual override or surrogate value has been applied. |
| **Gray italic cell** | A calculated value (Dermal RfD variants) — read-only, derived from other parameters. |
| **Gray cell with "--"** | No value available in the database for this parameter. |
| **Dagger** | Chronic value used as subchronic fallback. |
| **"From Surrogate" label** | In the detail panel, indicates the value came from a surrogate chemical assignment. |
| **"Use Surrogate" / "Edit Surrogate"** | Button text changes based on whether a surrogate is currently assigned. |

## Behavior

- **Defaults from database:** All default toxicity values are loaded from the ARC reference database. GIABS and ABSd come from the chemical's physical properties record. All other values come from the toxicity endpoints table with source priority ordering.
- **Override-only storage:** ARC does not store every toxicity value — only your manual overrides and surrogate assignments are stored. If no override exists, the database default is used.
- **Immediate save:** Every override you apply or reset is saved immediately.
- **Chemical list from COPCs:** The chemicals shown in the sidebar come from the union of all CAS numbers across all media on the COPCs screen. If no chemicals have been selected, the sidebar is empty.
- **Calculated values update live:** Overriding RfDo, GIABS, or any oral RfD triggers an automatic recalculation of all Dermal RfD variants in the grid.

## When to Use

Visit this screen after selecting chemicals on the COPCs screen. You need at least one chemical selected in any medium.

In many cases, the database defaults will be appropriate. Use this screen when:
- A chemical is missing key toxicity values and you want to assign a surrogate.
- Your regulatory program requires different values than the database defaults.
- You need to document the source and justification for a site-specific toxicity value.

The values on this screen feed directly into the risk calculations and appear in generated toxicity reports.
