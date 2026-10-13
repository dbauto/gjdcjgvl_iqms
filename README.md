# iQMS web

The iQMS front end: an ISO 9001 quality management system for Philippine businesses.
React, TypeScript, Tailwind CSS v4 and shadcn/ui (Radix). Every page is clickable with sample
data and no backend yet. Adding, editing and deleting work while the app is open, and reset on reload.

## Run it

```bash
npm install
npm run dev        # http://localhost:5173
npm run build      # production build in dist/
npm run typecheck  # TypeScript check
```

Node 20 or newer.

## Where to start

| Page | Path |
| --- | --- |
| Sign in, password reset, Cloudflare check | `/sign-in` |
| Microsoft 365 sign-in outcomes (prototype) | `/sign-in/microsoft` |
| Request access | `/request-access` |
| Overview | `/` |
| QMS: Organization & Scope, Policies, Processes, Objectives & KPIs | `/qms/...` |
| Documents register, reviews, a record | `/documents`, `/documents/reviews`, `/documents/OPS-WI-014` |
| Risks & Opportunities | `/risks` |
| Evidence | `/evidence` |
| Audits, a report, the checklist workspace, templates | `/audits`, `/audits/IA-2026-06`, `/audits/IA-2026-05/checklist`, `/audits/templates` |
| Management Review | `/management-review` |
| Corrective Action, a nonconformity | `/corrective-action`, `/corrective-action/NC-2026-005` |
| Settings (client admin) | `/settings` |
| Platform admin console (your team) | `/platform` |

**See it as different people.** The menu under your name has *Prototype: view as*. Switch between
Maria Santos (Quality manager), Aaron Lim (Administrator), Joy Aquino (Process owner),
Kim Dela Cruz (Auditor) and Elena Cruz (Guest auditor). The menu, buttons and Settings pages change
with each role. Remove this switcher when real sign-in is connected.

## How the code is organised

```
src/
  index.css            design tokens (colours, radii, type) for Tailwind v4 and shadcn
  App.tsx              all routes
  lib/store.tsx        in-memory data store, permissions (can), sidebar state
  data/                sample data and types
  components/ui/       shadcn-style primitives (button, badge, dialog, sheet, dropdown-menu, tabs, ...)
  components/app/      iQMS building blocks
    app-shell.tsx      sidebar + top bar + mobile menu
    sidebar.tsx        the client's menu, collapsible to icons
    data-table.tsx     register table: 10 rows in view, scrolls inside, 10/50/100 rows per page
    page.tsx           page header, figure cards (style B), lifecycle track, view chips, key facts
    status.tsx         stage and status badges
    record-actions.tsx row menu and ISO-aware delete / withdraw / archive
    form.tsx           field layout, chip choice, people picker (several people)
  pages/               one folder per menu area, plus auth, settings and platform
```

## Rules the UI follows

- **Menu wording** matches the client’s menu exactly, in the same order.
- **One primary button per page.** Everything else is outline, ghost or a link.
- **Status colours mean status only:** green done, indigo in progress, amber due soon, red overdue, grey draft.
- **Removing records follows ISO 9001:** drafts are deleted, records in a workflow are withdrawn,
  published or closed records are archived. See `components/app/record-actions.tsx`.
- **Classification:** Public, Internal, Confidential, Highly confidential. Confidential records keep a
  link and a description only, never the file, and show a warning about SharePoint sharing.
- **Several people per step:** reviews and approvals (everyone, any one, one after another), tasks
  (one accountable plus helpers), audit reports (editors and readers).
- **Permissions:** each role has a level per menu area (None, View, Add and edit, Approve, Manage).
  `can(area, level)` in `lib/store.tsx` hides menu items and buttons. **The backend must enforce the
  same rules**; the UI only reflects them.

## Connecting the backend

Replace `lib/store.tsx` with API calls (TanStack Query works well) and keep the same function
names in the pages.

## Not real yet

- Prices, invoices, hosting region, subprocessors and data protection officer details are placeholders.
- The Microsoft screens are a prototype of the outcomes. Microsoft shows its own sign-in page.
- Exports, emails and file uploads only show a confirmation.

## Libraries

shadcn/ui component patterns (new-york style), Radix UI primitives, lucide icons, sonner toasts,
React Router, Tailwind CSS v4, class-variance-authority, tailwind-merge. All open source (MIT or ISC).
