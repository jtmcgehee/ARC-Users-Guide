---
title: "Step 3: Conceptual Site Model"
layout: default
nav_order: 8
---

# Step 3: Conceptual Site Model

The Conceptual Site Model screen is where you define the scope of your risk assessment. You select which **receptors** (people who might be exposed), **exposure media** (soil, water, vapor, etc.), and **exposure pathways** (dermal contact, ingestion, inhalation) apply to your site. These selections drive every downstream screen — they determine which chemicals need concentrations, which exposure parameters appear, and which risk calculations are performed.

## Layout

The screen uses a split-panel design:

- **Left panel** — A list of all available receptor types. Click a receptor to view its detail panel. A badge next to each receptor shows the number of active pathway selections (or a dash if none).
- **Right panel** — The detail view for the selected receptor, showing all applicable exposure media and their pathway toggles.

## Receptors

Eight receptor types are available:

| Receptor | Age Groups | Description |
|---|---|---|
| **Resident** | No | A person living at or near the site. |
| **Indoor Worker** | No | A person working indoors at a building on or near the site. |
| **Outdoor Worker** | No | A person working outdoors on or near the site. |
| **Composite Worker** | No | A combined indoor/outdoor worker scenario. |
| **Recreator** | Yes (Adult, Adolescent, Child, Infant) | A person using the site for recreational purposes. |
| **Trespasser** | Yes (Adult, Adolescent, Child, Infant) | A person entering the site without authorization. |
| **Construction Worker** | No | A person performing construction or excavation work at the site. |
| **Utility Worker** | No | A person performing utility maintenance (e.g., trench work) at the site. |

For **Recreator** and **Trespasser**, an age-group selector bar appears at the top of the detail panel. You must check at least one age group (Adult, Adolescent, Child, or Infant) in addition to selecting media and pathways. Each checked age group generates a separate set of scenarios in the assessment.

## Exposure Media and Pathways

Each receptor has a set of applicable exposure media (determined by the reference database). For each medium, up to three pathway toggles are available:

| Pathway | Color | Description |
|---|---|---|
| **Dermal** | Purple | Direct skin contact with the medium. |
| **Ingestion** | Teal | Ingestion (eating/drinking) of the medium. |
| **Inhalation** | Amber | Breathing in vapors or particulates from the medium. |

Not every pathway applies to every medium. For example, Vapor Intrusion typically only has an Inhalation pathway. Only the applicable pathway chips are shown for each medium row.

## How to Make Selections

1. **Click a receptor** in the left panel to open its detail view.
2. **Toggle individual pathways** by clicking the colored pathway chips (Dermal, Ingestion, Inhalation) on each media row.
3. **Toggle all pathways for a medium** by clicking the checkbox or the medium name on the left side of the row. If any pathways are on, clicking the checkbox turns them all off; if all are off, it turns them all on.
4. **Select All / Clear** buttons at the top of the detail panel turn on or off all pathways across all media for that receptor.
5. For age-stratified receptors, **click age-group chips** (Adult, Adolescent, Child, Infant) to include or exclude those age groups.

## Visual Indicators

- **Checkbox states:** The checkbox next to each medium name shows three states — empty (no pathways selected), partially filled (some pathways selected), or fully checked (all pathways selected).
- **Badge counts:** The left-panel receptor list shows a count badge for each receptor with active selections. The count reflects the total number of pathway selections (multiplied by checked age groups for age-stratified receptors).
- **Summary footer:** A line at the bottom of the screen shows the total scenario count with a per-receptor breakdown (e.g., "12 scenarios: Resident (6), Outdoor Worker (6)").

## Behavior

- **Immediate save:** Every selection change is written to your project immediately. There is no need to explicitly save — your selections are preserved as soon as you toggle a chip or checkbox.
- **Available media vary by receptor:** The set of media shown in the detail panel depends on the receptor type. For example, a Construction Worker may have different applicable media than a Resident. This is determined by the reference database.
- **Pathway availability varies by medium:** Not all three pathways are available for every medium. Only applicable pathways appear as chips.

## When to Use

Fill out this screen after entering Project Info and Site Parameters. The selections you make here determine what appears on every subsequent screen:

- **COPCs** — The media tabs shown depend on which media you selected here.
- **EPCs** — Same as COPCs; only selected media appear.
- **Exposure Parameters** — Parameters are shown only for selected receptors.
- **Risk calculations** — Only selected receptor/medium/pathway combinations are computed.

If you return to this screen later and change selections, downstream screens will update accordingly.
