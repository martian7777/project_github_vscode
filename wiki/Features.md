# Features & Capabilities

RepoDeck is designed to bring your GitHub project management into the center of your coding workspace. Here is a deep dive into the features that make RepoDeck unique.

---

## 1. Interactive Project Boards (v2)

GitHub Projects v2 is built entirely on GraphQL and represents a major departure from legacy project boards. RepoDeck supports three primary layout views, which load from cache instantly and switch dynamically without making redundant network requests:

### A. Board View (Kanban)
- **Drag-and-Drop Columns**: Change the status of cards by dragging them between columns. Column headers are sticky at the top, and columns scroll independently.
- **Card Cards**: Displays the card title, issue/PR number, assignees, and issue/PR state (open, closed, merged, draft).
- **No-Status Column**: Cards that do not have a value defined for the board's single-select grouping field are collected in a special "No Status" tray.
- **Draft Items**: Create draft items directly on the board and convert/promote them to fully fledged issues later.

### B. Table View (Spreadsheet Interface)
- **Inline Editing**: Double-click cells to edit field values directly in the spreadsheet view. Supports titles, assignees, dates, iteration values, single-select options, and text/number inputs.
- **Virtual Scrolling**: Only renders rows that are visible on the screen, keeping performance smooth even for projects with 1,000+ items.
- **Sorting & Filtering**: Sort table headers instantly and filter cards using the search toolbar.

### C. Roadmap View (Gantt Chart)
- **Time Axis Display**: Plots cards across a calendar timeline using start and target date fields.
- **Visual Duration**: Hover over roadmap items to see precise dates and adjust timelines.

---

## 2. Smart Branch Checkout ("Start Working on Issue")

Starting a new task is simplified using the `repodeck.startWorkOnIssue` command:
1. **Branch Name Generation**: Reads the issue number and sanitizes the title into a clean git slug (e.g., `Issue #104: Fix auth cookies` becomes branch name `104-fix-auth-cookies`).
2. **Safe Branching Context**: Fetches the default branch (e.g., `main`) from the remote repository and checks out the new branch branching *from that remote default branch*. This prevents you from accidentally branching off another active feature branch.
3. **Dirty-Tree Check**: Warns you if you have uncommitted changes in your workspace, giving you a chance to commit, stash, or carry them over.

---

## 3. Local-to-Remote Repository Initializer

When you open a fresh, empty workspace folder, RepoDeck can configure your entire GitHub integration in one go via `repodeck.initRepo`:
1. **Details Input**: Prompts you for the repository name (defaults to the folder name), description, and visibility (Public or Private).
2. **Context Selection**: Detects if your user belongs to GitHub organizations and lets you choose between publishing the repository under your personal account or an organization.
3. **Local Setup**: Performs `git init` locally if it hasn't been initialized, checks out the default branch, stages local files, and commits them. If the folder is empty, it makes an empty commit to ensure the branch can be pushed.
4. **Remote Hook & Push**: Automatically links the remote clone URL (`origin`) and pushes the default branch up with tracking enabled.

---

## 4. Performant Sidebar Trees

RepoDeck populates your VS Code sidebar with two customizable tree views:
- **Issues View**: Hierarchical categories including:
  - *Assigned to Me*
  - *Created by Me*
  - *All Open*
- **Pull Requests View**: Hierarchical categories including:
  - *Waiting for My Review*
  - *Assigned*
  - *Created*
  - *All Open*

*Performance Optimization*: Sidebar views avoid the rate-limited Search API (which limits users to 30 requests/minute). Instead, they fetch issues and pull requests using standard list requests with a high-capacity budget (5,000 requests/hour).

---

## 5. Rich Detail Panels

Clicking on an issue or pull request in the sidebar tree or board opens a dedicated editor webview panel:
- **Full Markdown Rendering**: Descriptions, comments, and comments timelines render rich markdown (headings, bullet points, checklists, images, code highlighting, tables) instead of raw plain text.
- **Threaded Timeline**: Displays comments, review logs, commits, merges, and status updates in a clean, vertical chronological thread.
- **CI/CD Status**: Pull request details fetch and report automated test checks and commit statuses (success, failure, pending).
- **PR Management**: Directly merge PRs (via merge commit, squash, or rebase), mark drafts as "Ready for Review", close or reopen items, and delete merged remote branches from a single dashboard.

---

## 6. GitHub Actions

A dedicated **GitHub Actions** sidebar view brings CI/CD monitoring and configuration into the editor, organized into three sections:

### A. Current Branch & Workflows
- **Run History**: Lists recent workflow runs for the branch you currently have checked out, and — under the **Workflows** section — grouped beneath each workflow file.
- **Drill-Down**: Expand any run to reveal its **jobs**, and expand a job to reveal its individual **steps** (e.g. `Set up job`, `Run npm ci`, `Run npm run build`). Each level shows a status icon: a green check for success, a red error for failure, a spinner for in-progress/queued, and a slashed circle for cancelled or skipped.
- **Open on GitHub**: A globe icon on a run or job opens it directly on github.com to inspect the complete logs.
- **Run Controls**: Re-run a completed workflow run, or cancel one that is still in progress, from inline actions on the tree.

### B. Environments, Secrets & Variables
Under the **Settings** section, RepoDeck manages Actions configuration end-to-end against the GitHub REST API:
- **Repository Secrets & Variables**: Add, update, and delete repository-level secrets and variables. Names are validated against GitHub's rules (letters, digits, underscores; the reserved `GITHUB_` prefix is rejected).
- **Environments**: Each environment is listed with its own **Secrets** and **Variables** sub-groups, supporting the same add/update/delete operations scoped to that environment.
- **Client-Side Encryption**: GitHub only accepts secret values that have been sealed against the target's public key. RepoDeck fetches that public key, encrypts the value locally using **libsodium** (`crypto_box_seal`), and uploads only the ciphertext — the plaintext secret never leaves your machine, and GitHub never returns existing secret values, so an "update" always replaces the value with a fresh entry.

> **Permissions**: Reading runs and managing secrets/variables/environments is covered by the token's `repo` scope. If a token lacks the necessary admin rights, RepoDeck surfaces a clear message rather than failing silently.
