---
title: "Step 3: Conceptual Site Model"
layout: default
nav_order: 8
---

# Step 3: Conceptual Site Model

This is where you define the scope of your risk assessment by selecting which **receptors**, **exposure media**, and **exposure pathways** apply to your site. These selections drive every downstream screen.

## Layout

- **Left panel** — All available receptor types. A badge shows the count of active pathway selections for each.
- **Right panel** — The detail view for the selected receptor, showing applicable media and pathway toggles.

## Receptors

| Receptor | Age Groups | Description |
|---|---|---|
| **Resident** | No | A person living at or near the site. |
| **Indoor Worker** | No | A person working indoors on or near the site. |
| **Outdoor Worker** | No | A person working outdoors on or near the site. |
| **Composite Worker** | No | A combined indoor/outdoor worker scenario. |
| **Recreator** | Yes | A person using the site for recreation. |
| **Trespasser** | Yes | A person entering the site without authorization. |
| **Construction Worker** | No | A person performing construction or excavation at the site. |
| **Utility Worker** | No | A person performing utility maintenance (e.g., trench work). |

**Recreator** and **Trespasser** have an age-group selector (Adult, Adolescent, Child, Infant). Each checked age group generates a separate set of scenarios.

## Pathways

For each medium, up to three pathway toggles are available:

| Pathway | Color | Description |
|---|---|---|
| **Dermal** | Purple | Direct skin contact with the medium. |
| **Ingestion** | Teal | Eating or drinking the medium. |
| **Inhalation** | Amber | Breathing vapors or particulates from the medium. |

Not every pathway applies to every medium — only applicable ones are shown.

## Making Selections

- **Toggle individual pathways** by clicking the colored chips on each media row.
- **Toggle all pathways for a medium** by clicking its checkbox or name.
- **Select All / Clear** buttons at the top affect all media for that receptor.
- For age-stratified receptors, click the **age-group chips** to include or exclude groups.

A summary footer shows the total scenario count with a per-receptor breakdown.

## Downstream Impact

The selections here determine what appears on every subsequent screen:

- **COPCs / EPCs** — Only selected media get tabs.
- **Exposure Parameters** — Only selected receptors are shown.
- **Risk calculations** — Only selected receptor/medium/pathway combinations are computed.

Changing selections here will update downstream screens accordingly.
