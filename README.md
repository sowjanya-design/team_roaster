# Work Roster — Team Dashboard

A single-file, dependency-free web app for managing work across multiple ventures. Open `index.html` in any browser — no build step, no server required. State persists in the browser via `localStorage`.

## Features

- **Role switching** — act as Admin (full control) or as any staff member (sees only their own work and roster).
- **Venture filter** — filter the whole dashboard by venture, with live open-task counts.
- **Work Board** — three-column Kanban (To Do / In Progress / Done) with drag-and-drop status changes. Employees also get quick-move buttons.
- **Board tools (admin)** — search tasks, filter by assignee, and an "overdue only" toggle.
- **Tasks** — create, edit, delete; set venture, assignee, priority, due date, status, and notes. Overdue dates are flagged.
- **Team** — per-person cards with live workload bars and availability (Available / Busy / Off).
- **Roster** — weekly schedule grid; admins click cells to cycle Working → Off → Leave. Today's column is highlighted.
- **Stats strip** — open tasks, in progress, overdue, and team/completed counts that adapt to the current role.

## Usage

Just open `index.html` (or `team_roaster.html`) in a browser. All data is stored locally in your browser.

## Hosting

The app works as-is on any static host. On GitHub Pages, enable Pages for the repository and it will serve `index.html` at the root.
