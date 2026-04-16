# M365 Copilot Adoption Dashboard — Data Export Guide

> How to export the source files from Microsoft 365 Admin Centre, Viva Insights, and how to prepare your VIP user list before loading them into the dashboard.

---

## Contents

1. [What files does the dashboard need?](#1-what-files-does-the-dashboard-need)
2. [Export from M365 Admin Centre](#2-export-from-m365-admin-centre)
   - [Active User Report](#21-active-user-report-most-important)
   - [User Details Report](#22-user-details-report)
   - [Readiness Report](#23-readiness-report)
3. [Export from Viva Insights](#3-export-from-viva-insights)
4. [Prepare your VIP User List](#4-prepare-your-vip-user-list)
5. [Loading files into the dashboard](#5-loading-files-into-the-dashboard)
6. [Troubleshooting](#6-troubleshooting)

---

## 1. What files does the dashboard need?

| File | Source | Required? | What it unlocks |
|---|---|---|---|
| **Active User Report** | M365 Admin Centre | Recommended | Overview, User Directory, App Adoption, Prompts, Retention, Champions |
| **User Details Report** | M365 Admin Centre | Optional | Viva join via User GUID; additional per-app last-activity dates |
| **Readiness Report** | M365 Admin Centre | Optional | Readiness page — licence status, update channel, Teams/Outlook/Office signals |
| **Viva Insights Export** | Viva Insights Admin | Optional | Copilot Actions, Product Heatmap, Meetings & Teams, deep org analytics |
| **VIP User List** | Your own Excel/CSV | Optional | VIP Analysis page — named user consumption and licence planning |

You can load any combination. The dashboard adapts to whatever is available.

---

## 2. Export from M365 Admin Centre

**Who can do this:** Global Admin, Reports Reader, or Usage Summary Reports Reader role.

**Starting point:** [admin.microsoft.com](https://admin.microsoft.com) → **Reports** → **Microsoft 365** → **Copilot**

> **Note on privacy settings:** If your tenant has user-level reporting anonymised, the exports will show GUIDs instead of names. To fix this: Admin Centre → **Settings** → **Org settings** → **Services** → **Reports** → uncheck *"Display concealed user, group, and site names in all reports"*.

---

### 2.1 Active User Report *(most important)*

This is the primary file. It contains every licensed user's name, email, prompt counts, and the last date they used each Copilot app.

**Steps:**

1. Go to **Reports** → **Microsoft 365** → **Copilot**
2. Click the **Active users** tab
3. Set the date range using the dropdown — choose **Last 30 days** for a standard monthly view
4. Click **Export** (top-right of the table)
5. Save the file — it downloads as a `.csv` named something like:
   ```
   CopilotActivityUserDetailV4_3_27_2026 4_07_06 PM.csv
   ```

**Key columns in this file:**

| Column | Description |
|---|---|
| `User Principal Name` | The user's email / UPN — this is the join key |
| `Display Name` | Full name |
| `Prompts submitted for All Apps` | Total prompts across all Copilot surfaces |
| `Prompts submitted for Copilot Chat (work)` | Work-mode chat prompts |
| `Prompts submitted for Copilot Chat (web)` | Web-mode chat prompts |
| `Active Usage Days for All Apps` | Number of days the user was active |
| `Last activity date of Teams Copilot (UTC)` | Last time Teams Copilot was used |
| `Last activity date of Word Copilot (UTC)` | Last time Word Copilot was used |
| `Last activity date of Outlook Copilot (UTC)` | Last time Outlook Copilot was used |
| *(and Excel, PowerPoint, OneNote, Loop, Edge, Agent...)* | Per-app last dates |

---

### 2.2 User Details Report

This file provides the internal User GUID which the dashboard uses to join Admin Centre data with Viva Insights data. It also has additional per-app last-activity dates not in the Active User Report.

**Steps:**

1. Go to **Reports** → **Microsoft 365** → **Copilot**
2. Click the **User details** tab (sometimes labelled **EDP Activity**)
3. Click **Export**
4. Save the file — named something like:
   ```
   CopilotEDPActivityUserDetailV3_3_27_2026 4_07_38 PM.csv
   ```

**Key columns in this file:**

| Column | Description |
|---|---|
| `User guid` | Internal AAD Object ID — used for Viva join |
| `User principal name` | UPN / email |
| `Display name` | Full name |
| `Prompts submitted` | Total prompts |
| `Active usage days` | Days active |
| `Last activity date of Teams (UTC)` | Per-app last dates (Teams, Word, Excel, PPT, Outlook, OneNote, Edge) |

---

### 2.3 Readiness Report

This file shows which users have the prerequisites in place for Copilot to work well — licence assigned, correct update channel, and active use of Teams, Outlook, and Office.

**Steps:**

1. Go to **Reports** → **Microsoft 365** → **Copilot**
2. Click the **Readiness** tab
3. Click **Export**
4. Save the file — named something like:
   ```
   CopilotReadinessActivityUserDetail_3_27_2026 4_06_25 PM.csv
   ```

**Key columns in this file:**

| Column | Values | Description |
|---|---|---|
| `User Principal Name` | email | UPN |
| `Has Copilot license assigned` | TRUE / FALSE | Whether the user has an M365 Copilot licence |
| `Uses eligible update channel` | TRUE / FALSE | Device is on Current Channel or Monthly Enterprise Channel |
| `Uses Teams meetings` | TRUE / FALSE | User attends/hosts Teams meetings |
| `Uses Teams chat` | TRUE / FALSE | User sends messages in Teams |
| `Uses Outlook email` | TRUE / FALSE | User is active in Outlook |
| `Uses Office docs` | TRUE / FALSE | User works with Word, Excel, or PowerPoint |

> **What "not ready" means:** A user with a Copilot licence but `Uses eligible update channel = FALSE` will not see Copilot features in desktop Office apps until their device is moved to the correct update channel. This is one of the most common deployment blockers.

---

## 3. Export from Viva Insights

**Who can do this:** Insights Administrator or Global Admin.

Viva Insights provides 85 detailed Copilot metric columns that go well beyond what the Admin Centre reports show — including specific actions taken per app (summarise email, meeting recap, draft document, etc.), meeting hours recapped, and returning-user signals.

> **Privacy note:** In most tenants, Viva Insights anonymises user identities. The `PersonId` field in the export is a UUID-formatted hash of the user's email address, not their actual name or email. The dashboard automatically computes this hash from the Admin Centre UPNs and matches them together — you do not need to do anything manually.

**Steps:**

1. Go to [insights.viva.office.com](https://insights.viva.office.com)
2. In the left navigation, click **Analyst** → **Analysis**
3. Click **New analysis** → select **Copilot metrics**  
   *(Alternatively: go to **Data exports** → **Copilot dashboard metrics** if your admin has pre-configured an export)*
4. Set the date range — use the same period as your Admin Centre exports for consistency
5. Click **Run** and wait for the analysis to complete (can take a few minutes)
6. Click **Download** to save the CSV — named something like:
   ```
   Copilot_dashboard_metric_Mar27_2026_1503Hours.csv
   ```

> **Alternative export path (if Analyst access is not available):**  
> Ask your Viva Insights Administrator to go to **Admin** → **Privacy settings** → **Export** and schedule a Copilot metrics export to a SharePoint location. They can then share the downloaded file with you.

**Important format notes:**

- The file is **tab-delimited**, not comma-delimited
- `MetricDate` is in **DD/MM/YYYY** format (e.g. `05/10/2025` = 5th October 2025)
- `PersonId` is a UUID-format hash — do not try to decode it manually
- `Organization` is the last column and contains the Viva department/team name, which the dashboard uses for org-level grouping

**Key metric columns (sample):**

| Column | Description |
|---|---|
| `Total Copilot actions taken` | All Copilot actions combined |
| `Copilot actions taken in Teams` | Actions taken within Teams |
| `Copilot actions taken in Outlook` | Actions taken within Outlook |
| `Summarize email thread actions taken using Copilot in Outlook` | Specific feature usage |
| `Intelligent recap actions taken` | Meeting recap actions |
| `Draft Word document actions taken using Copilot` | Document drafting |
| `Meeting hours recapped by Copilot` | Hours of meetings recapped |
| `Returning Microsoft 365 Copilot user (most recent and prior 28 days)` | Retention signal |
| `Number of weeks active in Microsoft 365 Copilot (most recent 28 days)` | Engagement depth |
| `Organization` | Department/team name from Viva |

---

## 4. Prepare your VIP User List

The VIP list is a named list of specific users — typically senior leaders, high-priority personas, or pilot cohorts — that you want to analyse separately and use for licence planning decisions.

### File format

Create an **Excel (.xlsx)** or **CSV (.csv)** file with the following columns:

| Column | Required? | Description | Example |
|---|---|---|---|
| `No.` | Yes | Row number | `1` |
| `Name` | Yes | Full name — must match the Display Name in M365 | `Adrian Talbot` |
| `Email` | **Strongly recommended** | Full UPN from M365 Admin Centre | `Adrian.Talbot@easyjet.com` |
| `Role` | Optional | Job title or role | `Director of Investor Relations` |
| `Group / Department` | Optional | Team or department — used for group filtering | `Finance - Investor Relations` |
| `Comments` | Optional | Any notes, grade, or band | `L50` |

### Example

| No. | Name | Email | Role | Group / Department | Comments |
|---|---|---|---|---|---|
| 1 | Adrian Talbot | Adrian.Talbot@easyjet.com | Director of Investor Relations | Finance - Investor Relations | L50 |
| 2 | Sarah Patel | Sarah.Patel@easyjet.com | Chief People Officer | HR & People | L50 |
| 3 | Tim Langridge | Tim.Langridge@easyjet.com | Head of Technology | Technology | L40 |
| 4 | Laura Magill | Laura.Magill@easyjet.com | Head of Investor Relations | Finance - Investor Relations | L40 |

### Tips for accurate matching

- **Use the Email column** — this is by far the most reliable way to match VIP users to Copilot data. The email must exactly match the UPN shown in the Admin Centre exports (e.g. `Tim.Langridge@easyjet.com`, not `tim.langridge@easyjet.com` — matching is case-insensitive so either works).
- **Check Display Names** — if you don't have the email, the dashboard falls back to matching by Display Name. Make sure the name in your list exactly matches what appears in M365 (e.g. `Timothy Langridge` won't match `Tim Langridge`).
- **Keep the header row** — the dashboard auto-detects column positions, but a clear header row helps.
- **One sheet only** — if using Excel, keep all data on the first sheet.

### What the VIP page shows

Once loaded, the VIP Analysis page provides:

- **Licence planning recommendation** for each user: Keep / Onboard / Assign licence
- **Per-user consumption table** with prompts, active days, last activity per app, Viva actions, and a match indicator
- **Group filter tabs** to drill into specific teams or departments
- **Comparison** of VIP usage vs the overall user population average
- **Unmatched users** table listing anyone in the VIP list not found in Copilot data, with a suggested action

---

## 5. Loading files into the dashboard

1. Open `Copilot_Adoption_Dashboard_v6.html` in Chrome, Edge, or Firefox
2. On the upload screen, drop or click each file slot to load your exports:
   - **Active User Report** → top-left slot
   - **User Details** → top-centre slot
   - **Readiness Report** → top-right slot
   - **Viva Insights** → the purple slot (labelled "+ Viva Insights")
   - **VIP List** → the gold slot (labelled "VIP / Named User List")
3. Each slot shows a green tick and row count when a file is loaded successfully
4. Click **Load Dashboard** — the Viva join runs automatically (a few seconds for large files)
5. The sidebar shows match rates for Viva and VIP

> **All processing is local.** No data is sent to any server. Nothing leaves your browser.

### Recommended refresh cadence

| Report | How often to refresh |
|---|---|
| Active User Report | Monthly (export at end of each month) |
| User Details Report | Monthly, same date as Active User |
| Readiness Report | Monthly or quarterly |
| Viva Insights | Monthly |
| VIP List | Update as the named user list changes |

---

## 6. Troubleshooting

### Viva shows 0% match rate

The most common cause is that the tenant has user-level reporting anonymised in Admin Centre settings, which means the UPNs in the Admin Centre exports don't match the hashed PersonIds in Viva. Try loading the **User Details report** alongside Viva — the User GUID in that file is used as a direct match before the hash comparison runs.

Also check: is the Active User Report or User Details report loaded? The dashboard can only compute UPN hashes if it has UPNs to work with.

### VIP users show as "not found"

Check the following in order:

1. Is the **Email** column populated with the full UPN (e.g. `Name.Surname@company.com`)?
2. Does the email domain match what is in the Admin Centre exports? (Some tenants use `.aero` or `ops.` subdomains for certain user groups)
3. Is the Admin Centre data loaded? The dashboard can only match VIP users if it has the user population to search against.
4. Open the browser console (**F12** → Console tab) — the dashboard logs VIP match results there, e.g. `VIP match: 15/19 | email:14 exactName:1 fuzzy:0 upn:0 unmatched:4`

### Dates look wrong (off by several months)

The dashboard parses Viva `MetricDate` as **DD/MM/YYYY** (e.g. `05/10/2025` = 5th October). Admin Centre dates are parsed as either `YYYY-MM-DD` or `DD/MM/YYYY`. If dates appear wrong, check which format your export is using — this can vary by tenant regional settings.

### The file shows as loaded but data looks empty

This usually means the file was exported with user anonymisation enabled. Check Admin Centre → **Settings** → **Org settings** → **Reports** and disable the concealed names setting, then re-export.

### Export menu not visible in Admin Centre

You need at minimum the **Reports Reader** role. Ask your Global Admin to assign this role in Microsoft Entra ID → Roles and administrators.

---

## File naming reference

The dashboard auto-detects file type from the column headers, so the filename does not matter. For reference, the typical export names are:

```
CopilotActivityUserDetailV4_<date>.csv          ← Active User Report
CopilotEDPActivityUserDetailV3_<date>.csv       ← User Details Report
CopilotReadinessActivityUserDetail_<date>.csv   ← Readiness Report
Copilot_dashboard_metric_<date>.csv             ← Viva Insights Export
VIP_Users_<cohort>.xlsx                         ← Your VIP list (any name)
```

---

*For questions about the dashboard tool itself, see the [README](./README.md).*
