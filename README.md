<div align="center">

# 🤖 M365 Copilot Adoption Dashboard

**A fully offline, browser-based analytics dashboard for Microsoft 365 Copilot adoption — no server, no installation, no data leaves your machine.**

[![Version](https://img.shields.io/badge/version-6.0-0f6cbd?style=flat-square)](https://github.com/Yogeshkum84/CoPilot-Dashboard/releases)
[![License](https://img.shields.io/badge/license-MIT-27ae60?style=flat-square)](LICENSE)
[![Made With](https://img.shields.io/badge/made%20with-HTML%20%2F%20JS-f59e0b?style=flat-square)](#)
[![No Backend](https://img.shields.io/badge/backend-none%20required-9d4edd?style=flat-square)](#)
[![GitHub Stars](https://img.shields.io/github/stars/Yogeshkum84/CoPilot-Dashboard?style=flat-square&color=0f6cbd)](https://github.com/Yogeshkum84/CoPilot-Dashboard/stargazers)

---

*Built by a practitioner, for practitioners — turning raw M365 exports into actionable adoption intelligence.*

</div>

---

## What is this?

The M365 Copilot Adoption Dashboard is a **single HTML file** that you open in any modern browser. Drop in your CSV exports from the Microsoft 365 Admin Centre and Viva Insights, and it instantly builds a rich, interactive adoption dashboard — covering everything from headline adoption rates to per-user Copilot action breakdowns, meeting hours recapped, licence planning for named users, and product heatmaps by organisation.

No Power BI licence required. No Azure deployment. No API keys. No IT request. Just open the file.

---

## Screenshots

| Overview | Consumption | VIP Analysis |
|---|---|---|
| Adoption funnel, segment cards, Viva top actions | Prompt channel split, top consumers, org breakdown | Named user licence planning with match diagnostics |

| Product Heatmap | Champions | Readiness |
|---|---|---|
| Actions by app x organisation matrix | Leaderboard with Viva actions | Signal-by-signal gap analysis |

---

## Features

**13 reporting pages covering the full adoption lifecycle:**

| Page | Data Source | What you get |
|---|---|---|
| Overview | Admin Centre | Adoption rate, segment cards (Active / Churned / Lost / Never), funnel, top actions |
| Consumption | Admin Centre + Viva | Prompt channel split donut, top consumers, org comparison, top-20 detail table |
| User Directory | Admin Centre | Searchable, sortable table of every licensed user with per-app last-activity colour coding |
| App Adoption | Admin Centre + Viva | Per-app user counts, Viva active days, multi-app adoption, recency heatmap |
| Prompts | Admin Centre | Work vs web chat split, top users, volume buckets |
| Copilot Actions | Viva only | All 16 action types (summarise email, meeting recap, draft, rewrite...), by app |
| Product Heatmap | Viva only | App x Organisation matrix with intensity colouring — shows where Copilot is actually used |
| Meetings & Teams | Viva only | Hours recapped, meetings summarised, Teams chat summaries, by org |
| Retention | Admin Centre + Viva | 7-day / 30-day gauges, recency breakdown, churned user list, Viva returning-user signals |
| Champions | Admin Centre + Viva | Leaderboard of power users — 100+ prompts, 3+ apps, 10+ active days |
| Readiness | Admin Centre | 5-signal readiness grid, score distribution, not-ready user intervention list |
| Org Breakdown | Admin Centre + Viva | Adoption by Viva Organisation or email domain, segment mix, Viva action totals |
| **VIP Analysis** | All sources | Named user list — email-matched, licence recommendations, group filtering, unmatched diagnostics |

**Technical highlights:**

- Fully offline — all processing in the browser, no data transmitted
- Supports `.xlsx`, `.xls`, and `.csv` for the VIP list (SheetJS powered)
- SHA-256 UUID-format hash matching for Viva Insights PersonId join (handles hashed tenants automatically)
- DD/MM/YYYY date parsing for Viva MetricDate (tenant format)
- 4-strategy VIP name matching: email > exact name > fuzzy name parts > UPN local-part
- Auto-detects file type from column headers — drop files in any order
- Light theme UI — Plus Jakarta Sans + JetBrains Mono

---

## Quick Start

**1. Download the dashboard**

```bash
git clone https://github.com/Yogeshkum84/CoPilot-Dashboard.git
```

Or download `Copilot_Adoption_Dashboard_v6.html` directly from [Releases](https://github.com/Yogeshkum84/CoPilot-Dashboard/releases).

**2. Export your data** — see the [Export Guide](EXPORT_GUIDE.md) for step-by-step instructions.

**3. Open the file** in Chrome, Edge, or Firefox — no installation needed.

**4. Drop your CSV exports** into the upload slots and click **Load Dashboard**.

> Want to try it without any data? Click **Use Sample Data** on the upload screen — it generates 150 realistic users with full Viva metrics so you can explore every page immediately.

---

## Data Sources

| File | Where to export | Required? |
|---|---|---|
| Active User Report | Admin Centre → Reports → Microsoft 365 → Copilot → Active users → Export | Recommended |
| User Details Report | Admin Centre → Reports → Microsoft 365 → Copilot → User details → Export | Optional |
| Readiness Report | Admin Centre → Reports → Microsoft 365 → Copilot → Readiness → Export | Optional |
| Viva Insights Export | Viva Insights Admin → Analyst → Analysis → Copilot metrics → Download | Optional |
| VIP User List | Your own Excel or CSV — see format below | Optional |

**VIP list format** (`.xlsx` or `.csv`):

```
No. | Name              | Email                        | Role                           | Group / Department  | Comments
1   | Yogesh Kumar      | Yogesh.Kumar@company.com     | Director                       | Finance             | 

```

> See [EXPORT_GUIDE.md](EXPORT_GUIDE.md) for full field descriptions, export navigation steps, and troubleshooting tips.

---

## How the Viva Join Works

Viva Insights anonymises user identities. The `PersonId` field is the first 128 bits of `SHA-256(UPN)` formatted as a UUID — for example:

```
sha256("tim.C@YourCompany.com")      →  5df66e7aa1c1ffea...
First 32 hex chars                   →  5df66e7aa1c1ffea1db9fa34dd8a6a94
UUID format                          →  5df66e7a-a1c1-ffea-1db9-fa34dd8a6a94
```

The dashboard computes this hash from your Admin Centre UPNs and matches them automatically. No manual mapping or configuration is needed. Match rate is shown in the sidebar.

---

## File Structure

```
CoPilot-Dashboard/
├── Copilot_Adoption_Dashboard_v6.html   # The dashboard (single file, open in browser)
├── EXPORT_GUIDE.md                      # Step-by-step data export guide
├── README.md                            # This file
└── LICENSE                              # MIT licence
```

---

## Roadmap

- [ ] Export current view to PDF / PNG
- [ ] Saved filter profiles (persist via localStorage)
- [ ] Multi-period comparison (month-on-month delta)
- [ ] Power Automate / SharePoint auto-refresh integration guide
- [ ] Dark / light theme toggle
- [ ] Copilot Agent usage deeper breakdown (when available in exports)

Suggestions welcome — open an [Issue](https://github.com/Yogeshkum84/CoPilot-Dashboard/issues) or start a [Discussion](https://github.com/Yogeshkum84/CoPilot-Dashboard/discussions).

---

## Contributing

Contributions are welcome. If you work in a M365 / Digital Workplace environment and have export samples that behave differently (different column names, regional date formats, anonymised GUIDs), please open an issue — real-world samples are the fastest way to make the parsing more robust.

**To contribute:**

1. Fork the repo
2. Create a branch: `git checkout -b feature/my-improvement`
3. Make your changes and test with real or sample data
4. Open a pull request with a clear description of what changed and why

---

## About the Author

<div align="center">

<img src="https://avatars.githubusercontent.com/Yogeshkum84" width="96" height="96" style="border-radius:50%" alt="Yogesh Kumar"/>

### Yogesh Kumar

**Digital Workplace & IT Service Delivery Manager**  
*15+ years in enterprise IT | Microsoft 365 | ITSM | EUC | AVD | ServiceNow | Nexthink*

</div>

I built this tool out of necessity — the native M365 reporting is powerful but fragmented across three different portals, and getting a clear picture of *who* is using Copilot, *how* they're using it, and *who needs intervention* required too many manual steps. This dashboard brings it all together in one offline file that any adoption manager or IT lead can run without needing Power BI, Azure, or admin rights beyond report export access.

My background is in Digital Workplace delivery and IT service management in the Travel domain. I manage large-scale M365 environments and EUC programmes, and I build self-contained tooling that packages domain expertise into reusable, practical deliverables — this is one of them.

**Find me:**

| Platform | Link |
|---|---|
| GitHub | [@Yogeshkum84](https://github.com/Yogeshkum84) |
| LinkedIn | [linkedin.com/in/yogesh-kumar-itil](www.linkedin.com/in/yogesh-kumar-41933124) |
| Email | Available via GitHub profile |

---

## Support & Donations

This tool is free, open source, and built in personal time. If it saves your team hours of manual reporting work or helps you make a better case for Copilot adoption with your leadership — a coffee would be appreciated.

<div align="center">

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Support%20this%20project-f59e0b?style=for-the-badge&logo=buy-me-a-coffee&logoColor=white)](https://monzo.me/yogeshkumar36?h=_Ye7K0)

[![GitHub Sponsor](https://img.shields.io/badge/GitHub%20Sponsors-Sponsor%20on%20GitHub-ea4aaa?style=for-the-badge&logo=github-sponsors&logoColor=white)](https://monzo.me/yogeshkumar36?h=_Ye7K0)
<img width="247" height="241" alt="image" src="https://github.com/user-attachments/assets/a4e89ac1-41e7-4c31-95f4-95c88c654d11" />


*Every contribution helps fund time for new features, bug fixes, and keeping the column maps updated as Microsoft releases new export formats.*

</div>

---

## Licence

This project is licensed under the **MIT Licence** — see [LICENSE](LICENSE) for details.

You are free to use, modify, and distribute this tool. If you build something on top of it or adapt it for your organisation, a credit or link back is appreciated but not required.

---

## Disclaimer

This is an **independent community tool** — it is not affiliated with, endorsed by, or supported by Microsoft. All product names, trademarks, and export formats belong to their respective owners. Export availability and column names may change as Microsoft updates the Admin Centre reporting.

---

<div align="center">

Made with care by [Yogesh Kumar](https://github.com/Yogeshkum84) &nbsp;|&nbsp; Digital Workplace & IT Service Delivery &nbsp;|&nbsp; UK

*If this tool helped you — please consider giving it a ⭐ on GitHub. It helps others find it.*

</div>
