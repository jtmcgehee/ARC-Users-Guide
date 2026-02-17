---
title: "Step 1: Project Information"
layout: default
nav_order: 6
---

# Step 1: Project Information

The Project Information screen is the first screen you see when starting a new project in ARC. It captures the basic identifying details for your risk assessment — who the client is, what the site is called, and where it is located. This information appears in the headers of generated reports and is saved with your project file.

## Fields

The screen is organized into two sections:

### Project Details

| Field | Required | Description |
|---|---|---|
| **Client Name** | Yes | The name of the client or organization commissioning the risk assessment. |
| **Project Number** | No | Your internal project or task number (e.g., PRJ-2026-001). Used for your records only. |
| **Site Name** | Yes | The name of the site being assessed. This is also used as the suggested filename when saving the project. |

### Site Location

| Field | Required | Description |
|---|---|---|
| **Street Address** | Yes | The street address of the site. Appears in report headers. |
| **City** | Yes | The city where the site is located (e.g., Durham). Appears in report headers. |
| **State** | Yes | The state where the site is located (e.g., North Carolina). Appears in report headers. |
| **ZIP Code** | Yes | The site's ZIP code (e.g., 27713). Appears in report headers. |

Below the location fields, two read-only labels display **Date Created** and **Last Modified** timestamps. These are set automatically and cannot be edited.

## Behavior

- **Required fields:** Client Name, Site Name, Street Address, City, State, and ZIP Code are all marked with a red asterisk (*). These fields must be filled in before reports can be generated — if any are missing, the Reports screen will show a message listing the incomplete fields. Project Number is the only optional field.
- **Auto-save:** Every change you make to a field is saved to the project automatically (once the project has been saved at least once). You do not need to press a Save button after each edit.
- **Dates:** Date Created is set when the project is first created. Last Modified updates each time the project is saved. Both are displayed in a human-readable format (e.g., "February 17, 2026" or "February 17, 2026 at 02:30 PM").
- **Navigation persistence:** If you navigate to another screen and come back, all values you entered will still be there.

## When to Use

This is typically the first screen you fill out when starting a new risk assessment. All fields except Project Number must be completed before you can generate reports.

No data from other screens is needed before filling out this screen.

Once you've entered the project details, click **Next** to proceed to Site Parameters.

![Project Information screen](Assets/Project Information.png)
