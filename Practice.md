# Practice.md — Git & GitHub Practice Guide

| Field | Value |
|---|---|
| **Document type** | Version Control Practice Guide |
| **Project** | HR Employee Onboarding Dashboard |
| **Version** | 1.0 (Draft for course assignment) |
| **Date** | 2026-11-05 |
| **Related document** | `SPEC.md` (project specification) |

---

## 1. GitHub Repository Setup

### 1.1 Prerequisites

1. A GitHub account.
2. Git installed locally. Verify with:

```bash
git --version
```

3. Git identity configured once per machine:

```bash
git config --global user.name  "Your Full Name"
git config --global user.email "your.email@example.com"
```

### 1.2 Creating the Repository

**Option A: Create on GitHub first (recommended for beginners)**

1. On GitHub, select **New repository** (the **+** menu at the top right).
2. Fill in the fields:

| Field | Recommended value |
|---|---|
| Repository name | `hr-onboarding-dashboard` |
| Description | "Single-page HR dashboard tracking new-hire onboarding (mock data)." |
| Visibility | See Section 1.3 |
| Initialize with README | Yes |
| Add `.gitignore` | Optional; a custom one is provided in Section 4.3 |
| License | Optional (e.g. MIT) for public repositories |

3. Select **Create repository**.
4. Clone it locally:

```bash
git clone https://github.com/<your-username>/hr-onboarding-dashboard.git
cd hr-onboarding-dashboard
```

**Option B: Start locally, then publish**

```bash
mkdir hr-onboarding-dashboard
cd hr-onboarding-dashboard
git init -b main
# add files (SPEC.md, Practice.md, ...)
git add .
git commit -m "chore: initial project setup"
git remote add origin https://github.com/<your-username>/hr-onboarding-dashboard.git
git push -u origin main
```

(Create the empty repository on GitHub first, without a README, to avoid a history conflict on the first push.)

### 1.3 Public vs. Private Repository

| Criterion | Public | Private |
|---|---|---|
| **Who can view** | Anyone on the internet | Only the owner and invited collaborators |
| **Who can contribute** | Only the owner and invited collaborators (others may fork and open pull requests) | Only the owner and invited collaborators |
| **Best for** | Portfolios, open-source work, course submissions that must be accessible to graders | Internal work, drafts, anything containing sensitive or proprietary content |
| **GitHub Pages hosting** | Available | Availability depends on the account plan; verify against current GitHub documentation |
| **Risk** | Accidentally exposing sensitive data permanently | Lower exposure risk |

**Recommendation for this project**

- The project uses **mock data only**, so a **public** repository is acceptable and simplifies sharing with instructors and graders.
- If the course requires restricted access, use a **private** repository and add the instructor as a collaborator (Section 1.4).
- **Never commit real employee data, credentials, API keys, or tokens**, regardless of visibility. A public repository exposes them to everyone, and data removed in a later commit remains in Git history.
- Visibility can be changed later under **Settings → General → Danger Zone → Change repository visibility**.

### 1.4 Adding Collaborators

**For a personal repository**

1. Open the repository on GitHub.
2. Go to **Settings → Collaborators** (or **Collaborators and teams**).
3. Select **Add people**.
4. Enter the person's GitHub username, full name, or email address.
5. Select the correct user and confirm **Add to this repository**.
6. The invitee must accept the invitation (sent by email and shown in their GitHub notifications) before gaining access.

**Permission levels**

| Role | Typical use |
|---|---|
| **Read** | Reviewers and instructors who only need to view or clone |
| **Triage** | Managing issues and pull requests without write access |
| **Write** | Team members who push branches and merge pull requests |
| **Maintain** | Managing the repository without sensitive administrative actions |
| **Admin** | Full control, including settings and deletion |

Role options for personal repositories may differ from those for organization repositories. Verify against current GitHub documentation.

**Principle of least privilege:** grant Read to reviewers and Write to active contributors; reserve Admin for the repository owner.

### 1.5 Recommended Repository Settings

| Setting | Recommendation |
|---|---|
| Default branch | `main` |
| Branch protection on `main` | Require pull request review before merging (where available on the account plan) |
| Pull request merging | Allow squash merging for a clean history |
| Issues | Enabled, used to track tasks from `SPEC.md` |
| Secret scanning | Enable if offered |

---

## 2. Git Workflow Best Practices

### 2.1 The Core Cycle

```
Working Directory  --git add-->  Staging Area  --git commit-->  Local Repo  --git push-->  GitHub (remote)
```

| Step | Command | Purpose |
|---|---|---|
| 1 | `git status` | Inspect what changed and what is staged |
| 2 | `git add <file>` | Stage specific changes for the next commit |
| 3 | `git commit -m "<message>"` | Record the staged changes locally |
| 4 | `git push` | Upload local commits to GitHub |

### 2.2 `git status`: Check Before Every Action

```bash
git status
```

Reports:

- **Untracked files**: new files Git is not yet tracking.
- **Changes not staged for commit**: modified files not yet added.
- **Changes to be committed**: staged files ready to commit.
- The current branch and whether it is ahead of or behind the remote.

**Practice:** run `git status` before `git add` and again before `git commit` to confirm exactly what will be recorded.

Useful companions:

```bash
git status -s          # short, compact output
git diff               # unstaged changes
git diff --staged      # staged changes (what the next commit will contain)
git log --oneline -10  # last 10 commits, one line each
```

### 2.3 `git add`: Stage Intentionally

```bash
git add index.html          # stage one file
git add app.js data.json    # stage several named files
git add docs/               # stage a directory
git add -p                  # review and stage changes interactively
```

**Best practices**

- Prefer naming files explicitly over `git add .` or `git add -A`, which can stage unintended files (temporary files, local configs, secrets).
- If using `git add .`, always follow with `git status` to verify.
- Keep each commit to **one logical change**. Stage only the files that belong to it.
- To unstage a file: `git restore --staged <file>`.

### 2.4 `git commit`: Record a Logical Unit of Work

```bash
git commit -m "feat(table): add department filter dropdown"
```

**Best practices**

- Commit small and often; each commit should leave the project in a working state.
- One purpose per commit. Do not mix a bug fix, a new feature, and formatting in one commit.
- Write messages that follow Section 3.
- Review `git diff --staged` before committing.
- Never commit secrets, credentials, or personal data.
- Do not rewrite history (`--amend`, `rebase`, `push --force`) on commits that have already been pushed to a shared branch.

To correct the most recent **unpushed** commit message:

```bash
git commit --amend -m "corrected message"
```

### 2.5 `git push`: Share Your Work

```bash
git push                          # push the current branch (after upstream is set)
git push -u origin main           # first push: set upstream tracking
git push -u origin feat/charts    # first push of a feature branch
```

**Best practices**

- Run `git pull --rebase` (or `git pull`) before pushing to integrate collaborators' changes and avoid rejected pushes:

```bash
git pull --rebase origin main
git push
```

- Push at least once per working session so that work is backed up remotely.
- Do not use `git push --force` on shared branches. If it is ever necessary on your own feature branch, use `--force-with-lease`.

### 2.6 Branching Workflow (Recommended for Collaboration)

| Branch type | Naming pattern | Example |
|---|---|---|
| Main | `main` | `main` (always stable and demo-ready) |
| Feature | `feat/<short-description>` | `feat/department-chart` |
| Fix | `fix/<short-description>` | `fix/csat-average-null` |
| Documentation | `docs/<short-description>` | `docs/update-spec` |

**Typical flow**

```bash
git switch main
git pull origin main
git switch -c feat/bottleneck-chart      # create and switch to a new branch

# ... edit files ...
git status
git add app.js index.html
git commit -m "feat(charts): add bottleneck chart by stage"
git push -u origin feat/bottleneck-chart
```

Then on GitHub:

1. Open a **Pull Request** from `feat/bottleneck-chart` into `main`.
2. Request a review from a collaborator.
3. Address comments with further commits on the same branch.
4. Merge (squash merge recommended) and delete the branch.
5. Locally: `git switch main && git pull origin main`.

### 2.7 Daily Routine Checklist

- [ ] `git switch main && git pull` (start from the latest code)
- [ ] Create or switch to a feature branch
- [ ] Make a small, focused change
- [ ] `git status`, then `git diff`
- [ ] `git add <specific files>`
- [ ] `git diff --staged`
- [ ] `git commit -m "<conventional message>"`
- [ ] `git pull --rebase`, then `git push`
- [ ] Open or update the pull request

### 2.8 Common Problems and Fixes

| Situation | Resolution |
|---|---|
| Push rejected ("non-fast-forward") | `git pull --rebase`, resolve conflicts, then `git push` |
| Committed the wrong file | If not pushed: `git reset --soft HEAD~1`, restage correctly, commit again |
| Staged a file by mistake | `git restore --staged <file>` |
| Discard local edits to a file | `git restore <file>` (irreversible for uncommitted changes) |
| Secret committed and pushed | Treat the secret as compromised: revoke or rotate it immediately, then seek guidance on removing it from history |

---

## 3. Commit Message Guidelines (Conventional Commits)

### 3.1 Format

```
<type>(<optional scope>): <short description>

[optional body]

[optional footer(s)]
```

**Rules**

1. Use the **imperative mood** ("add", not "added" or "adds").
2. Keep the subject line to about **50 characters** (hard limit: 72).
3. Use **lowercase** for the type and the start of the description; do not end the subject with a period.
4. Use the body to explain **why** the change was made, not only what changed. Wrap lines at about 72 characters.
5. Reference issues in the footer where relevant (e.g. `Closes #4`).
6. Mark breaking changes with `!` after the type or scope, and/or a `BREAKING CHANGE:` footer.

### 3.2 Commit Types

| Type | Use for |
|---|---|
| `feat` | A new feature or user-visible capability |
| `fix` | A bug fix |
| `docs` | Documentation-only changes (README, SPEC.md, Practice.md) |
| `style` | Formatting or whitespace changes that do not affect behavior |
| `refactor` | Code restructuring without changing behavior |
| `perf` | Performance improvements |
| `test` | Adding or correcting tests |
| `build` | Build system or dependency changes |
| `ci` | Continuous integration configuration |
| `chore` | Maintenance tasks that do not fit the above (setup, housekeeping) |
| `revert` | Reverting a previous commit |

### 3.3 Suggested Scopes for This Project

| Scope | Area |
|---|---|
| `kpi` | KPI summary cards and calculations |
| `charts` | Bottleneck and department charts |
| `table` | Employee data table, sorting, pagination |
| `filters` | Department, status, and manager filters |
| `data` | Mock dataset and schema |
| `ui` | Layout, styling, responsiveness |
| `spec` | `SPEC.md` |
| `repo` | Repository configuration and structure |

### 3.4 Project-Specific Examples

**Repository setup and documentation**

```
chore: initialize repository structure
docs(spec): add project specification SPEC.md
docs: add Practice.md with Git workflow guide
docs: add README with run instructions
chore(repo): add .gitignore for editor and OS files
```

**Data**

```
feat(data): add mock dataset with 50 employee records
fix(data): correct start_date format to ISO 8601
fix(data): set days_to_complete to null for in-progress hires
refactor(data): move embedded JSON into data.json
```

**KPIs**

```
feat(kpi): add total new hires onboarding card
feat(kpi): calculate average onboarding duration in days
feat(kpi): add overall task completion rate
feat(kpi): add average CSAT score card
fix(kpi): exclude null CSAT scores from average
fix(kpi): display dash instead of zero when no data
```

**Charts**

```
feat(charts): add bottleneck chart by stage
feat(charts): add department breakdown bar chart
feat(charts): apply department filter on chart bar click
fix(charts): sync chart data with active table filters
style(charts): use consistent status color palette
```

**Table and filters**

```
feat(table): render employee data table with all schema columns
feat(filters): add department filter dropdown
feat(filters): add status filter (not started, in progress, completed)
feat(filters): add direct manager filter
feat(filters): add reset button to clear all filters
feat(table): add sortable column headers
fix(table): show empty-state message when no rows match
```

**UI and quality**

```
feat(ui): add responsive layout for tablet and mobile
style(ui): apply Tailwind spacing and card styling
perf(table): paginate table to 10 rows per page
test(kpi): add checks for KPI formulas against mock data
```

**Example with body and footer**

```
fix(kpi): exclude null CSAT scores from average

Employees who have not yet submitted a satisfaction score were
counted as zero, which lowered the average CSAT. Only non-null
scores are now included, matching the KPI definition in SPEC.md.

Closes #7
```

**Example of a breaking change**

```
refactor(data)!: rename days_to_complete to onboarding_days

BREAKING CHANGE: data.json field days_to_complete is now
onboarding_days. Update any code reading the old field name.
```

### 3.5 Poor vs. Good Messages

| Poor | Good | Why |
|---|---|---|
| `update` | `feat(filters): add direct manager filter` | States type, scope, and the actual change |
| `fixed stuff` | `fix(table): show empty-state message when no rows match` | Specific and searchable |
| `Added charts and fixed filter and changed colors` | Three separate commits | One purpose per commit |
| `WIP` | `feat(charts): add department chart (initial structure)` | Describes the state of the work |
| `Final version!!!` | `docs: finalize SPEC.md for submission` | Professional and informative |

---

## 4. Expected Repository File Structure

### 4.1 Directory Tree

```
hr-onboarding-dashboard/
├── .gitignore              # Files and folders excluded from version control
├── README.md               # Project summary and how to run it
├── SPEC.md                 # Project specification
├── Practice.md             # This Git and GitHub practice guide
├── index.html              # Dashboard page (layout, Tailwind, Chart.js canvases)
├── app.js                  # Data loading, filtering, KPI calculation, rendering
├── data.json               # Mock employee dataset (schema defined in SPEC.md)
└── assets/                 # Optional: screenshots and images used in README
    └── dashboard-preview.png
```

If the Streamlit alternative from `SPEC.md` is chosen, the application files change as follows:

```
hr-onboarding-dashboard/
├── .gitignore
├── README.md
├── SPEC.md
├── Practice.md
├── app.py                  # Streamlit application
├── employees.csv           # Mock employee dataset
└── requirements.txt        # Python dependencies (streamlit, pandas, plotly)
```

### 4.2 File Purposes

| File | Purpose | Tracked in Git |
|---|---|---|
| `README.md` | First page visitors see: project description, features, how to run, and a screenshot | Yes |
| `SPEC.md` | Source of truth for requirements, KPIs, layout, schema, and stack | Yes |
| `Practice.md` | Team guide for repository setup and Git conventions | Yes |
| `index.html` | Single-page dashboard markup | Yes |
| `app.js` | Application logic | Yes |
| `data.json` | Mock data (no real personal information) | Yes |
| `assets/` | Images for documentation | Yes |
| `.gitignore` | Exclusion rules | Yes |

### 4.3 Suggested `.gitignore`

```gitignore
# Operating system files
.DS_Store
Thumbs.db

# Editor and IDE settings
.vscode/
.idea/

# Dependencies and build output
node_modules/
dist/
__pycache__/
.venv/

# Logs and temporary files
*.log
*.tmp

# Environment files and secrets (never commit)
.env
.env.*
*.key
*.pem
```

### 4.4 Suggested `README.md` Outline

1. Project title and one-sentence description
2. Screenshot of the dashboard
3. Features (KPI cards, bottleneck chart, department chart, filterable table)
4. Tech stack
5. How to run (open `index.html` in a browser, or `streamlit run app.py`)
6. Data note: all data is synthetic mock data
7. Link to `SPEC.md` and `Practice.md`

---

## 5. Quick Reference

```bash
# Start
git clone <repo-url>
git switch -c feat/<name>

# Work cycle
git status
git add <files>
git commit -m "<type>(<scope>): <description>"
git pull --rebase origin main
git push -u origin feat/<name>

# Inspect
git log --oneline -10
git diff
git diff --staged
```

**Golden rules**

1. Check `git status` before every `add` and `commit`.
2. Commit small, focused changes with Conventional Commit messages.
3. Pull before you push.
4. Never commit secrets or real personal data.
5. Never force-push to a shared branch.

