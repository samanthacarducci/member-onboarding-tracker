# Pavilion 90-Day Onboarding Tracker — Project Context

## What This Is
A live, interactive HTML dashboard tracking Pavilion's new 90-day Executive Member onboarding program (launched February 15, 2026). The goal is to improve Y1 retention from a **49% baseline to a 60% target** by ensuring new members "activate" within their first 90 days.

The dashboard is a single self-contained `index.html` file hosted on Netlify that fetches live data from a published Google Sheet CSV on every page load.

---

## Live Data Source
**Google Sheet CSV URL:**
```
https://docs.google.com/spreadsheets/d/1PTuOEnbQEvPwyButEaMshg7KX-2uOl1wfSIsJyIgrIU/pub?gid=0&single=true&output=csv
```
- Sheet updates every 4 hours
- Dashboard auto-refreshes every 4 hours and on manual click of the "Live Data" button
- Row 1 of the sheet is the header row (no metadata rows above it)

---

## Google Sheet Column Structure
| Col | Name |
|-----|------|
| 0 | Membership Start Date |
| 1 | Chargebee Customer ID |
| 2 | Location (Chapter) |
| 3 | Current Term Period |
| 4 | Subscription Type |
| 5 | Touched by Sales - Meeting |
| 6 | Onboarding Call |
| 7 | Initiation Meeting Date |
| 8 | Slack Sent |
| 9 | Date of First Dinner |
| 10 | No. of Days to First Learn Enrollment |
| 11 | Total Referrals |
| 12 | Last Dinner Invite Date |
| 13 | No. of Dinners Registered |
| 14 | No. of Dinners Attended |
| 15 | No. of Live Courses Enrolled In |
| 16 | No. of Live Courses Passed |
| 17 | Slack Messages (All Time) |
| 18 | Slack Messages (Last Month) |
| 19 | Slack Engagement |
| 20 | NPS Rating within 110 Days |
| 21 | Member Became At Risk On |
| 22 | Churned On |

**Important data notes:**
- `Touched by Sales - Meeting` and `Slack Sent` export as `True`/`False` strings from Google Sheets (not 1/0)
- `Date of First Dinner` and `Initiation Meeting Date` are date strings (e.g. `2/10/2026`)
- `No. of Days to First Learn Enrollment` — 0 counts as enrolled (not blank)
- `#ERROR!` values should be treated as empty/false

---

## The 5 Activation Milestones

### Milestone 1 — Touched by Sales / Onboarding Call
- `Touched by Sales - Meeting` = True OR `Onboarding Call` is non-empty

### Milestone 2 — Concierge / Initiation Call
- `Initiation Meeting Date` is non-empty (any date string = yes)

### Milestone 3 — Chapter Dinner
- `Date of First Dinner` is non-empty (date string) OR
- `No. of Dinners Registered` > 0 OR
- `No. of Dinners Attended` > 0

### Milestone 4 — Slack Intro
- `Slack Sent` = True OR
- `Slack Messages (All Time)` > 0 OR
- `Slack Messages (Last Month)` > 0

### Milestone 5 — Course Enrolled
- `No. of Days to First Learn Enrollment` is not blank (0 counts!) OR
- `No. of Live Courses Enrolled In` > 0

---

## Activation Tier Definitions

| Tier | Definition |
|------|------------|
| **Not Started** | Neither milestone 1 nor 2 complete |
| **In Progress** | Milestone 1 OR 2 complete (but not both) |
| **Called** | Milestone 1 AND 2 complete, fewer than 2 of (Dinner/Slack/Course) done |
| **Attached** | Milestone 1 AND 2 complete, PLUS any 2 of (Dinner, Slack, Course) |
| **Fully Activated** | All 5 milestones complete |

---

## Brand Colors (Pavilion)
```css
--blue-700: #180A5C
--blue-500: #432CAE
--blue-400: #6D59CF
--blue-300: #998BDF
--blue-200: #CEC6F4
--purple-400: #C57FD9
--purple-300: #DEB0EB
--purple-200: #F4E0FA
--pink-500: #FF768F
--pink-400: #FEB1BF
--green: #6EE7B7
--amber: #FCD34D
```

### Tier Color Mapping
| Tier | Color |
|------|-------|
| Not Started | `#FEB1BF` |
| In Progress | `#CEC6F4` |
| Called | `#C57FD9` |
| Attached | `#FCD34D` |
| Fully Activated | `#6EE7B7` |

---

## Dashboard Structure (4 Tabs)

### 1. Overview Tab
- KPI strip: Total Members, Monthly Members, Fully Activated, Attached+, At Risk, Churned
- Activation tier funnel (horizontal bar chart)
- Milestone completion bars (5 milestones, % of all members)
- Monthly vs Annual tier distribution (stacked segment bars)
- Top chapters by member count
- Weekly intake stacked bar chart by activation tier (Chart.js)

### 2. Member Detail Tab
- Filterable/searchable table
- Filters: Search, Billing, Tier, Status, Milestone
- Sortable columns

### 3. At-Risk & Churned Tab
- At-risk members table
- Churned members table
- Monthly members with zero milestones table

### 4. Ask Claude Tab
- AI chat powered by Anthropic API (claude-sonnet-4-20250514)
- User pastes their own API key (memory only)
- Suggested questions pre-loaded

---

## Key Business Context
- **Program launched:** February 15, 2026
- **Audience:** Executive Members only
- **Highest churn risk group:** Monthly billing members
- **Retention baseline:** 49% Y1 | **Target:** 60% Y1
- **Program owners:** Concierge calls by Bailey; Initiation calls by Rich or Sam

## Tech Stack
- Single `index.html` — no build process, no backend
- Chart.js 4.4.1 via cdnjs
- Google Fonts: Syne + DM Sans
- Anthropic API for Ask Claude tab
- Hosted on Netlify
