# SPEC.md — HR Employee Onboarding Dashboard

| Field | Value |
|---|---|
| **Document type** | Project Specification |
| **Project name** | HR Employee Onboarding Dashboard |
| **Version** | 1.0 (Draft for course assignment) |
| **Date** | 2026-11-05 |
| **Target audience** | HR Team |
| **Data** | Mock (synthetic) data only; no real employee records |

---

## 1. Project Overview & Business Value

### 1.1 Purpose

The HR Employee Onboarding Dashboard is a single-screen analytics tool that gives the HR team a real-time view of every new hire's onboarding progress. It consolidates the three workstreams that determine whether a new employee is ready to work (IT equipment, paperwork, and mandatory training) into one place, so that delays are visible as they happen rather than discovered after the fact.

### 1.2 Problem Statement

Onboarding depends on several parties (HR, IT, and hiring managers) who each own a different part of the process. Without a shared view:

- HR learns about delays only when a new hire or manager complains.
- IT equipment, document signing, and training progress are tracked in separate systems or spreadsheets.
- No single measure shows how long onboarding takes or where it stalls.
- Decisions about process changes rely on anecdotes rather than data.

### 1.3 Target Audience

**Primary users: the HR team** (HR business partners, onboarding coordinators, and the HR manager).

Secondary stakeholders, who benefit from the HR team's use of the dashboard but do not use it directly in this version, are IT, hiring managers, and department heads.

### 1.4 Core Objectives

| # | Objective | How the dashboard supports it |
|---|---|---|
| 1 | **Real-time onboarding tracking** | Shows the live status of every active new hire across equipment, paperwork, and training. |
| 2 | **Eliminate bottlenecks** | Visualizes where employees are stuck by stage, so HR can escalate the right issue to the right owner. |
| 3 | **Shorten time-to-productivity** | Tracks average onboarding duration and highlights hires exceeding the target. |
| 4 | **Inter-departmental alignment (HR, IT, Managers)** | Gives HR, IT, and managers a shared set of facts: who owns each open item and how long it has been open. |
| 5 | **Data-driven decisions** | Provides KPIs, department comparisons, and satisfaction scores to justify process changes and resourcing. |

### 1.5 Business Value

- **Faster productivity:** Shorter onboarding means new hires contribute sooner.
- **Reduced HR workload:** Fewer manual status checks and follow-up emails.
- **Accountability:** Each open item is attributable to a stage and a department.
- **Better employee experience:** Early detection of problems improves the CSAT score.
- **Continuous improvement:** Trends in duration and CSAT show whether process changes work.

### 1.6 Scope

**In scope**

- Read-only dashboard with KPI cards, two charts, and a filterable data table.
- Mock dataset of 40 to 60 employee records.
- Client-side filtering and sorting.

**Out of scope (version 1.0)**

- Authentication and role-based access.
- Live integration with HRIS, ITSM, or LMS systems.
- Editing records or sending notifications from the dashboard.
- Handling of real personal data.

---

## 2. Top-Level KPIs

All KPIs are computed from the mock data schema in Section 4. The "Population" column states which records are included.

| KPI | Definition | Formula | Population | Display format |
|---|---|---|---|---|
| **Total new hires currently onboarding** | Number of employees whose onboarding has not yet been completed. | `COUNT(employees WHERE Overall Status != "Completed")` | All employees | Integer |
| **Average onboarding duration (days)** | Mean number of days from start date to onboarding completion. | `AVG(Days to Complete)` | Employees with a completed onboarding only | One decimal, e.g. `14.3 days` |
| **Overall task completion rate (%)** | Share of all onboarding tasks that are completed. | `completed_tasks / total_tasks × 100`, where each employee has 3 tasks (equipment, paperwork, training) and a task counts as completed when its status is `Completed` (equipment: `Delivered`) | All employees | Percentage, no decimals |
| **Average onboarding satisfaction (CSAT)** | Mean satisfaction score on a 1 to 5 scale. | `AVG(CSAT Score)` | Employees who submitted a score (non-null) | One decimal with scale, e.g. `4.2 / 5` |

### 2.1 KPI Rules

- Records with a missing CSAT score or missing `Days to Complete` are excluded from those two averages, not treated as zero.
- All KPIs recalculate when table filters change (see Section 3.4), so HR can view KPIs for a single department or manager.
- A KPI with no eligible records displays `—` rather than `0`.

### 2.2 Optional Supporting Indicator

- **Overdue count:** number of employees whose days since start date exceed the target onboarding duration (target is a configurable constant, default 30 days) and whose onboarding is not completed. Shown as a badge on the first KPI card.

---

## 3. Visual Components & Dashboard Layout

### 3.1 Layout Overview

```
+--------------------------------------------------------------------------+
|  HEADER: HR Employee Onboarding Dashboard          [Last updated: <date>] |
+--------------------------------------------------------------------------+
|  KPI CARD 1        KPI CARD 2        KPI CARD 3        KPI CARD 4         |
|  Currently         Avg Onboarding    Task Completion   Avg CSAT           |
|  Onboarding        Duration          Rate              Score              |
+--------------------------------------------------------------------------+
|  CHART A: Bottlenecks by Stage        |  CHART B: Department Breakdown   |
|  (IT equipment / Document signing /   |  (Engineering, Sales, Marketing, |
|   Mandatory training)                 |   etc.)                          |
+--------------------------------------------------------------------------+
|  FILTER BAR: [Department v] [Status v] [Direct Manager v] [Reset]        |
+--------------------------------------------------------------------------+
|  EMPLOYEE DATA TABLE (sortable, paginated)                               |
|  ID | Name | Dept | Manager | Start | Equip | Paper | Training | CSAT |D |
+--------------------------------------------------------------------------+
```

**Responsive behavior**

- Desktop (≥1024 px): four KPI cards in one row; charts side by side.
- Tablet (640 to 1023 px): two KPI cards per row; charts stacked.
- Mobile (<640 px): single column; table scrolls horizontally.

### 3.2 Key Metric Summary Cards

Four cards, one per KPI in Section 2, in a single row at the top of the page.

| Card | Primary value | Secondary text | Visual treatment |
|---|---|---|---|
| New hires onboarding | Integer count | "N overdue" badge (optional indicator) | Neutral blue accent |
| Avg onboarding duration | Days, one decimal | Comparison to target (e.g. "Target: 30 days") | Green if at or under target, amber if over |
| Task completion rate | Percentage | Thin progress bar | Green accent |
| Avg CSAT | Score out of 5 | Star or numeric scale indicator | Color band: ≥4.0 green, 3.0 to 3.9 amber, <3.0 red |

### 3.3 Charts

#### Chart A: Bottlenecks by Stage

- **Purpose:** Show which onboarding stage has the most unresolved items.
- **Chart type:** Horizontal or vertical bar chart (single series), or stacked bar showing `Pending` / `In Progress` / `Completed` per stage.
- **Categories (x-axis):**
  1. IT Equipment
  2. Document Signing (paperwork)
  3. Mandatory Training
- **Measure (y-axis):** Number of employees not yet completed in that stage. In the stacked variant, the count of employees in each status.
- **Behavior:** Responds to table filters; tooltip shows exact counts and percentage of employees; the stage with the highest unresolved count is highlighted.
- **Insight enabled:** "Most delays are in IT equipment, so escalate to IT."

#### Chart B: Department Breakdown

- **Purpose:** Compare onboarding load and performance across departments.
- **Chart type:** Bar chart (default) or doughnut chart for headcount share.
- **Categories:** Engineering, Sales, Marketing, Finance, Operations, HR, Customer Support (extensible).
- **Measure:** Number of employees currently onboarding per department. Optional second view: average days to complete per department.
- **Behavior:** Clicking a bar applies that department as a table filter; tooltip shows count and share of total.
- **Insight enabled:** "Engineering has the heaviest onboarding load and the longest duration."

### 3.4 Detailed Employee Data Table

**Columns (in order):** Employee ID, Full Name, Department, Direct Manager, Start Date, Equipment Status, Paperwork Status, Training Status, CSAT Score, Days to Complete.

**Filters**

| Filter | Control | Options | Default |
|---|---|---|---|
| Department | Dropdown | "All" plus each department in the data | All |
| Status | Dropdown | "All", "Not Started", "In Progress", "Completed" (based on Overall Status, see Section 4.3) | All |
| Direct Manager | Dropdown | "All" plus each manager name in the data | All |
| Reset | Button | Clears all filters | n/a |

**Behavior**

- Filters combine with AND logic.
- Changing any filter updates the KPI cards, both charts, and the table simultaneously.
- Column headers are sortable (ascending and descending).
- Pagination or scrolling: 10 rows per page.
- Status cells use color-coded badges: `Completed` = green, `In Progress` = amber, `Pending` or `Not Started` = gray, `Blocked` = red.
- Empty state: "No employees match the selected filters."
- Null values (e.g. CSAT not yet submitted) display `—`.

### 3.5 Visual Design Guidelines

- Clean, light theme with a neutral background and a single accent color.
- Consistent status color palette across table badges and charts.
- Accessible contrast (WCAG AA) and text labels alongside color, never color alone.
- Sans-serif system font stack.

---

## 4. Mock Data Schema

### 4.1 Table: `employees`

| # | Column | Data type | Required | Allowed values / format | Description |
|---|---|---|---|---|---|
| 1 | `employee_id` | String | Yes | Pattern `EMP-####`, unique | Unique identifier for the new hire. Primary key. |
| 2 | `full_name` | String | Yes | Free text | Employee's full name (synthetic). |
| 3 | `department` | String (enum) | Yes | `Engineering`, `Sales`, `Marketing`, `Finance`, `Operations`, `HR`, `Customer Support` | Department the employee joins. |
| 4 | `direct_manager` | String | Yes | Free text | Full name of the direct manager responsible for the hire. |
| 5 | `start_date` | Date | Yes | ISO 8601 `YYYY-MM-DD` | First day of employment. |
| 6 | `equipment_status` | String (enum) | Yes | `Pending`, `Ordered`, `Delivered`, `Blocked` | Status of IT equipment provisioning (laptop, accounts, access). `Delivered` counts as completed. |
| 7 | `paperwork_status` | String (enum) | Yes | `Pending`, `In Progress`, `Completed`, `Blocked` | Status of required document signing (contract, policies, tax forms). |
| 8 | `training_status` | String (enum) | Yes | `Not Started`, `In Progress`, `Completed` | Status of mandatory training (compliance, security, safety). |
| 9 | `csat_score` | Integer | No (null until submitted) | `1` to `5` | Employee's onboarding satisfaction rating. |
| 10 | `days_to_complete` | Integer | No (null until onboarding completed) | ≥ 0 | Days between `start_date` and completion of all three stages. |

### 4.2 Data Integrity Rules

1. `employee_id` is unique.
2. `days_to_complete` is populated only when all three statuses are complete (`equipment_status = Delivered`, `paperwork_status = Completed`, `training_status = Completed`); otherwise it is null.
3. `csat_score` may be submitted only after onboarding is complete; otherwise it is null.
4. `start_date` is not later than the current date in the mock dataset.
5. Every `department` value belongs to the enumerated list.

### 4.3 Derived Field: Overall Status

Computed in the frontend, not stored:

| Overall Status | Rule |
|---|---|
| `Completed` | Equipment `Delivered`, paperwork `Completed`, and training `Completed` |
| `Not Started` | No stage has progressed beyond its initial value (`Pending` / `Not Started`) |
| `In Progress` | Any other combination |

### 4.4 Mock Dataset Guidelines

- Size: 40 to 60 records.
- Distribution: about 60% still onboarding, about 40% completed.
- Start dates spread over the previous 8 weeks.
- Departments weighted unevenly (e.g. Engineering and Sales larger) so the department chart is informative.
- Include several `Blocked` equipment records, so the bottleneck chart shows a clear leader.
- `days_to_complete` realistic range: 7 to 45 days; CSAT skewed toward 3 to 5.
- All names are fictional; no real individuals.

### 4.5 Sample Records

| employee_id | full_name | department | direct_manager | start_date | equipment_status | paperwork_status | training_status | csat_score | days_to_complete |
|---|---|---|---|---|---|---|---|---|---|
| EMP-1001 | Dana Levi | Engineering | Omer Katz | 2026-09-14 | Delivered | Completed | Completed | 5 | 12 |
| EMP-1002 | Noam Barak | Sales | Tamar Golan | 2026-10-05 | Ordered | In Progress | In Progress | null | null |
| EMP-1003 | Yael Sharon | Marketing | Ron Avni | 2026-10-12 | Delivered | Completed | In Progress | null | null |
| EMP-1004 | Eitan Mor | Engineering | Omer Katz | 2026-10-19 | Blocked | Pending | Not Started | null | null |
| EMP-1005 | Shira Ben-David | Finance | Lior Peretz | 2026-09-21 | Delivered | Completed | Completed | 4 | 18 |
| EMP-1006 | Amit Cohen | Customer Support | Maya Shalev | 2026-10-26 | Pending | Pending | Not Started | null | null |

*All names are fictional and used for demonstration only.*

---

## 5. Recommended Tech Stack

### 5.1 Recommended Option (Primary): Single-Page HTML5 + Tailwind CSS + Chart.js

| Layer | Technology | Rationale |
|---|---|---|
| Markup | HTML5 | Single file, no build step, runs by opening in a browser. |
| Styling | Tailwind CSS (via CDN) | Fast, consistent, responsive utility-based styling. |
| Charts | Chart.js (via CDN) | Lightweight, well-documented bar and doughnut charts with tooltips and click events. |
| Logic | Vanilla JavaScript (ES6+) | Sufficient for filtering, KPI calculation, sorting, and rendering; no framework required. |
| Data | Embedded JSON array (or `data.json`) | Matches the schema in Section 4; easy to replace with an API later. |
| Hosting | Local file, GitHub Pages, or any static host | Zero server cost; simple demo and submission. |

**Why this option:** minimal setup, easy to review and submit, instantly shareable, and appropriate to the scope (read-only dashboard with mock data).

### 5.2 Alternative Option: Streamlit (Python)

| Layer | Technology | Rationale |
|---|---|---|
| App framework | Streamlit | Dashboard in pure Python with built-in widgets. |
| Data handling | pandas | Simple filtering and aggregation. |
| Charts | Plotly Express or Altair | Interactive charts with minimal code. |
| Data | CSV file (`employees.csv`) | Matches the schema in Section 4. |
| Hosting | Streamlit Community Cloud or local | Quick deployment. |

**When to choose it:** if the team is more comfortable with Python or expects to extend the project with data analysis.

### 5.3 Comparison

| Criterion | HTML5 / Tailwind / Chart.js | Streamlit |
|---|---|---|
| Setup effort | None (open file) | Python environment and dependencies |
| Language | JavaScript | Python |
| Design control | High | Moderate |
| Interactivity | Custom code | Built-in widgets |
| Deployment | Any static host | Streamlit-capable host |
| Fit for this assignment | **Recommended** | Acceptable alternative |

### 5.4 Suggested Application Structure (Primary Option)

```
onboarding-dashboard/
├── index.html        # Layout, Tailwind classes, Chart.js canvases
├── app.js            # Data load, filtering, KPI calculation, rendering
├── data.json         # Mock dataset (schema in Section 4)
└── SPEC.md           # This document
```

**Processing flow**

1. Load `data.json`.
2. Apply the active filters (department, status, manager).
3. Compute the four KPIs from the filtered set.
4. Re-render the KPI cards, both charts, and the table.
5. Repeat on every filter change.

---

## 6. Non-Functional Requirements

| Category | Requirement |
|---|---|
| **Performance** | Initial render in under 2 seconds; filter updates under 300 ms for up to 500 records. |
| **Usability** | All information visible on one screen at 1080p without navigation; filters reachable in one click. |
| **Accessibility** | WCAG AA contrast; keyboard-navigable filters and table; text alternatives for chart data. |
| **Compatibility** | Latest versions of Chrome, Edge, Firefox, and Safari. |
| **Privacy** | Mock data only; if adapted to real data in future, access control and data-protection review are required before use. |
| **Maintainability** | Single source of truth for the data schema; configuration constants (target duration, color thresholds) defined in one place. |

---

## 7. Acceptance Criteria

1. The dashboard displays four KPI cards whose values match the formulas in Section 2 when checked against the mock data.
2. The bottleneck chart shows the three stages and updates when filters change.
3. The department chart shows all departments present in the data.
4. The table lists all ten schema columns, and the department, status, and direct manager filters work individually and in combination.
5. Resetting filters restores the full dataset view.
6. Null values display as `—` and are excluded from the CSAT and duration averages.
7. The layout is usable on desktop and mobile widths.

---

## 8. Suggested Delivery Plan

| Phase | Deliverable |
|---|---|
| 1. Data | Mock dataset created and validated against Section 4. |
| 2. Layout | Static page with KPI cards, chart placeholders, and table. |
| 3. Logic | KPI calculation, filters, and sorting. |
| 4. Charts | Bottleneck and department charts connected to filtered data. |
| 5. Polish and test | Responsive styling, accessibility check, acceptance-criteria review. |

---

## 9. Future Enhancements (Beyond Version 1.0)

- Integration with HRIS, IT service management, and learning management systems for live data.
- Automated reminders to IT and managers for overdue items.
- Trend view of average duration and CSAT over time.
- Role-based views for IT and hiring managers.
- Export of the filtered table to CSV.

