\# HR Employee Onboarding Dashboard



A single-page, interactive dashboard that gives the HR team a real-time view of new-hire onboarding progress across IT equipment, document signing, and mandatory training.



> \*\*Note:\*\* All employee data in this project is fictional mock data, created for demonstration and course-assignment purposes.



\---



\## Business Problem



Onboarding a new employee involves several parties (HR, IT, and the hiring manager), each responsible for a different part of the process. Without a shared view:



\- \*\*Tracking new hires is manual.\*\* HR learns about delays only when a new hire or manager raises a complaint.

\- \*\*Bottlenecks stay hidden.\*\* IT equipment, document signing, and training are tracked separately, so it is unclear where new hires get stuck.

\- \*\*Time-to-productivity is unmeasured.\*\* There is no single figure for how long onboarding takes or whether it is improving.



This dashboard brings the three onboarding workstreams into one screen, so HR can see who is stuck, at which stage, and how long onboarding takes, and can make decisions based on data instead of anecdotes.



\---



\## Key Features



\### KPI Summary Cards



| KPI | What it shows |

|---|---|

| \*\*New hires onboarding\*\* | Number of employees whose onboarding is not yet complete, with an overdue badge (more than 30 days since start) |

| \*\*Average onboarding duration\*\* | Mean days from start date to completion, compared with a 30-day target |

| \*\*Task completion rate\*\* | Share of all onboarding tasks (equipment, paperwork, training) that are completed |

| \*\*Average CSAT\*\* | Mean onboarding satisfaction score on a 1 to 5 scale |



\### Interactive Charts



\- \*\*Bottlenecks by stage:\*\* A stacked bar chart of status (Completed, In Progress, Not Started, Blocked) for IT Equipment, Document Signing, and Mandatory Training, with a note naming the largest bottleneck.

\- \*\*Department breakdown:\*\* A bar chart of onboarding load per department, switchable to average days to complete. Clicking a bar filters the whole dashboard by that department.



\### Searchable Employee Table



\- Search by employee name or ID.

\- Filter by \*\*department\*\*, \*\*status\*\*, and \*\*direct manager\*\*; filters combine.

\- Sort by any column, with 10 rows per page.

\- Color-coded status badges.

\- KPI cards, charts, and table all update together when filters change.



\---



\## Repository Structure



```

hr-onboarding-dashboard/

├── README.md       # Project overview and usage instructions (this file)

├── SPEC.md         # Project specification: objectives, KPIs, layout, data schema, tech stack

├── Practice.md     # Git and GitHub practice guide: setup, workflow, commit conventions

└── index.html      # The complete dashboard (single file: HTML, JavaScript, and mock data)

```



| File | Purpose |

|---|---|

| `README.md` | Entry point for anyone visiting the repository |

| `SPEC.md` | Source of truth for requirements and the mock data schema |

| `Practice.md` | Team guide for repository setup and Git conventions |

| `index.html` | The runnable dashboard |



\---



\## Viewing the Dashboard Locally



No installation, build step, or server is required.



1\. \*\*Get the files.\*\* Clone the repository or download it as a ZIP and extract it:



&#x20;  ```bash

&#x20;  git clone https://github.com/<your-username>/hr-onboarding-dashboard.git

&#x20;  cd hr-onboarding-dashboard

&#x20;  ```



2\. \*\*Open `index.html` in a web browser.\*\* Either:

&#x20;  - double-click `index.html` in your file explorer, or

&#x20;  - drag the file into an open browser window, or

&#x20;  - right-click the file and choose \*\*Open with\*\* and then your browser.



3\. \*\*Use the dashboard.\*\* Try the search box, the filters, the department chart (click a bar), and the sortable column headers.



\### Requirements



\- A modern browser (current versions of Chrome, Edge, Firefox, or Safari).

\- \*\*An internet connection.\*\* Tailwind CSS and Chart.js are loaded from public CDNs (`cdn.tailwindcss.com` and `cdn.jsdelivr.net`).



\### Troubleshooting



| Symptom | Likely cause and fix |

|---|---|

| Red banner at the top naming a missing library | The CDN could not be reached. Check your internet connection or whether a firewall blocks the CDN, then reload. |

| Page appears unstyled | Same cause as above: Tailwind CSS did not load. |

| Charts are missing but the table works | Chart.js did not load. Check the connection and reload. |

| The file opens as plain text | Check that the file is named `index.html` and not `index.html.txt`. |



\---



\## Tech Stack



| Layer | Technology |

|---|---|

| Markup | HTML5 |

| Styling | Tailwind CSS (via CDN) |

| Charts | Chart.js (via CDN) |

| Logic and data | Vanilla JavaScript with an embedded mock dataset of 20 new hires |



\---



\## Documentation



\- \[`SPEC.md`](SPEC.md): full specification, including KPI formulas and the mock data schema.

\- \[`Practice.md`](Practice.md): repository setup, Git workflow, and Conventional Commits guidelines.

