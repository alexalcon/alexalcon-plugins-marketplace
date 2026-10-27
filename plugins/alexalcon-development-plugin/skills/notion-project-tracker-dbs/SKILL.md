---
name: "notion-project-tracker-dbs"
description: "Create the three linked Notion tracker databases (Development Notes, Theory Notes, Code Generation Reports) for a software project, with per-phase views, from a project name, parent page and phase list."
---

# Notion Project Tracker Databases

Creates three inline, linked databases on a project page in Notion, cloned from the risk_gated_llm_trader tracker structure. Project-specific option lists are left empty so another skill or the developer fills them later. Phases come from the project plan. Every database gets an All view plus one view per phase (Development Notes also gets a Docs view), and property order in every view matches the reference project.

## Inputs

1. Project: the project name (used in database titles) and the Notion parent page (URL or ID) where the databases are created.
2. Phase list: the ordered phases of the project development plan, for example `Phase 0, Phase 1, Phase 2, Phase 3, Phase 4, Phase 5`, or labels with names such as `Phase 0: Foundations`. Use each label verbatim as an option name and a view name. Option names cannot contain commas; if one does, ask the user for a replacement.

If either input is missing, ask for it before creating anything.

## Tools

Notion MCP: `notion-fetch`, `notion-create-database`, `notion-update-data-source`, `notion-create-view`, `notion-update-view`. Load them with ToolSearch in one call if deferred.

## Database titles

Each title starts with an emoji that says what the database is for. Use these titles exactly:

- `💻 <project_name> — Development Notes` (development work: architecture, implementation, decisions, docs)
- `📚 <project_name> — Theory Notes` (concepts and theory studied for the project)
- `📄 <project_name> — Code Generation Reports` (reports on AI-generated code)

The emoji is part of the title text. Do not also set a database icon, because Notion would show the emoji twice.

## Phase option colors

Assign colors to phases in order, cycling if there are more than nine: blue, pink, yellow, purple, red, green, orange, brown, gray. Use the same phase options and colors in all three databases. Below, `<PHASES>` means the comma-separated list `'Phase 0':blue, 'Phase 1':pink, ...`.

## Property order

Notion shows schema properties alphabetically unless a view sets an order, so every view gets a `SHOW` directive with the order from the reference project's default views. Use these strings exactly.

`<DEV_ORDER>` (Development Notes; Project Phase takes the slot the removed Risk Impact had):

```
SHOW "Dev ID", "Legacy Dev ID", "Note", "Type", "System Area", "Decision", "Priority", "Project Phase", "Component", "Files and Media", "GitHub / Code URL", "Status", "Problem", "Context", "Acceptance Criteria", "Implementation Notes", "Summary", "Related Theory Notes", "Date Started", "Date Completed", "Last Reviewed", "Created Time", "Last Edited Time"
```

`<THEORY_ORDER>` (Theory Notes; Risk Link removed from its slot after Domain):

```
SHOW "Note ID", "Concept / Topic", "Difficulty", "Domain", "Project Relevance", "Related Component", "Project Phase", "Project Version", "Learning Status", "Project Application", "Why Needed", "Related Dev Notes", "Confidence 0-5", "Files and Media", "Date Started", "Date Completed", "Last Reviewed", "Created Time", "Last Edited Time"
```

`<CODEGEN_ORDER>` (Code Generation Reports):

```
SHOW "Report Title", "Project Version", "Project Phase", "Component", "Reading Status", "Files & media", "Date Started", "Date Completed", "Last Reviewed", "Created Time", "Last Edited Time"
```

The CREATE TABLE statements below list columns in the same order for readability.

## View order

Target tab order for each database:

- Development Notes: `All`, `Docs`, then one tab per phase in plan order.
- Theory Notes: `All`, then one tab per phase in plan order.
- Code Generation Reports: `All`, then one tab per phase in plan order.

View filters:

- All: no filter.
- Docs (Development Notes only): `FILTER "Type" = "Docs"`. Holds documentation and file records that are separate from any single phase.
- Phase view in Development Notes: `FILTER "Project Phase" = "<phase>"; FILTER "Type" != "Docs"` (resolves to "Project Phase contains <phase> AND Type does not contain Docs").
- Phase view in Theory Notes and Code Generation Reports: `FILTER "Project Phase" = "<phase>"`.

Every view also carries its database's order string (`<DEV_ORDER>`, `<THEORY_ORDER>` or `<CODEGEN_ORDER>`), appended with `;`.

## Rules learned from testing

- Empty option lists (`SELECT()`, `MULTI_SELECT()`) are accepted by the API. Use them as written.
- In `RELATION(...)`, pass the bare data source UUID (for example `'55bec4db-7a87-42c0-8884-8a027ad051a5'`). Passing the `collection://` prefix fails with "Cannot create relation".
- Create the three databases one at a time, in the order Development Notes, Theory Notes, Code Generation Reports. Never create them in parallel: each new database is appended to the page, so creation order is display order.
- Create views one at a time, waiting for each call to return before the next. Tabs are appended in the order the calls land, so parallel `notion-create-view` calls produce a scrambled tab order. There is no tool to move a tab afterwards.
- The views list returned by fetching a database is in tab order. If the order is wrong, fix it by position: use `notion-update-view` on the tab in each slot to set the right `name` and `configure` (`CLEAR FILTER; <filters>`), keeping its `SHOW` order.
- `SHOW` sets both the visible properties and their column order in a view. It works in `notion-create-view` and `notion-update-view`, combined with filters using `;`.
- Emoji in the `title` passed to `notion-create-database` (or to `notion-update-data-source`) is stored as part of the title.

## Procedure

### Step 1. Check the parent page

Fetch the parent page. Confirm it exists and get its page ID. If it already contains databases with the same titles, stop and ask whether to continue (this would create duplicates).

### Step 2. Create Development Notes

Title: `💻 <project_name> — Development Notes`. Parent: the project page.

```sql
CREATE TABLE (
  "Dev ID" RICH_TEXT,
  "Legacy Dev ID" RICH_TEXT,
  "Note" TITLE,
  "Type" MULTI_SELECT('Docs':gray),
  "System Area" SELECT(),
  "Decision" SELECT(),
  "Priority" SELECT('P0 Critical':red, 'P1 High':orange, 'P2 Medium':yellow, 'P3 Low':gray),
  "Project Phase" MULTI_SELECT(<PHASES>),
  "Component" MULTI_SELECT(),
  "Files and Media" FILES,
  "GitHub / Code URL" URL,
  "Status" SELECT('Backlog':gray, 'Designing':blue, 'Implementing':yellow, 'Testing':orange, 'Validated':green, 'Archived':gray),
  "Problem" RICH_TEXT,
  "Context" RICH_TEXT,
  "Acceptance Criteria" RICH_TEXT,
  "Implementation Notes" RICH_TEXT,
  "Summary" RICH_TEXT,
  "Date Started" DATE,
  "Date Completed" DATE,
  "Last Reviewed" DATE,
  "Created Time" CREATED_TIME,
  "Last Edited Time" LAST_EDITED_TIME
)
```

Rules for this database:
- Dev ID is plain text (RICH_TEXT). Never use UNIQUE_ID, FORMULA or any computed type for it.
- Type starts with only the `Docs` option.
- System Area, Component and Decision start with no options.
- There is no Risk Impact property.
- The relation to Theory Notes is added in Step 3 as the synced side of a two-way relation. Do not add it here.

Save the returned data source UUID (from `collection://<uuid>`) and the database ID (from the database URL).

### Step 3. Create Theory Notes

Wait for Step 2 to return. Title: `📚 <project_name> — Theory Notes`. Parent: the project page.

```sql
CREATE TABLE (
  "Note ID" RICH_TEXT,
  "Concept / Topic" TITLE,
  "Difficulty" SELECT('Beginner':green, 'Intermediate':yellow, 'Advanced':orange, 'Research-Level':red),
  "Domain" SELECT(),
  "Project Relevance" SELECT('Core':red, 'Supporting':yellow, 'Optional':gray, 'Later':blue),
  "Related Component" MULTI_SELECT(),
  "Project Phase" MULTI_SELECT(<PHASES>),
  "Project Version" SELECT('V1':blue),
  "Learning Status" SELECT('To Learn':orange, 'Learning':yellow, 'Summarized':blue, 'Applied':green, 'Mastered':purple, 'Archived':gray),
  "Project Application" RICH_TEXT,
  "Why Needed" RICH_TEXT,
  "Related Dev Notes" RELATION('<dev_notes_data_source_uuid>', DUAL 'Related Theory Notes'),
  "Confidence 0-5" NUMBER,
  "Files and Media" FILES,
  "Date Started" DATE,
  "Date Completed" DATE,
  "Last Reviewed" DATE,
  "Created Time" CREATED_TIME,
  "Last Edited Time" LAST_EDITED_TIME
)
```

Rules: Domain and Related Component start with no options. There is no Risk Link property. The DUAL relation creates `Related Theory Notes` on Development Notes automatically.

### Step 4. Create Code Generation Reports

Wait for Step 3 to return. Title: `📄 <project_name> — Code Generation Reports`. Parent: the project page.

```sql
CREATE TABLE (
  "Report Title" TITLE,
  "Project Version" SELECT('V1':blue),
  "Project Phase" MULTI_SELECT(<PHASES>),
  "Component" MULTI_SELECT(),
  "Reading Status" STATUS('Not started':default:to_do, 'Learning':blue:in_progress, 'In progress':blue:in_progress, 'Done':green:complete),
  "Files & media" FILES,
  "Date Started" DATE,
  "Date Completed" DATE,
  "Last Reviewed" DATE,
  "Created Time" CREATED_TIME,
  "Last Edited Time" LAST_EDITED_TIME
)
```

Rules: Component starts with no options. Reading Status default is `Not started`.

### Step 5. Make all three inline

New databases are created full-page. For each data source, call `notion-update-data-source` with `is_inline: true` (these three calls can run in parallel). Do not set database icons; the emoji is already in each title.

### Step 6. Build the views

Do this after Step 3, so the synced `Related Theory Notes` property exists. Fetch each database to get its default view ID (named "Default view"). All views are tables, created with both `database_id` and `data_source_id` (view tabs, not linked views).

For each database:

1. With `notion-update-view` on the default view, set `name` to `All` and `configure` to the database's order string. No filter. This stays the first tab.
2. Development Notes only: create `Docs` next, with `FILTER "Type" = "Docs"; <DEV_ORDER>`.
3. Create one phase view per phase, in plan order, with the phase filter for that database plus its order string (see View order).

Create every view with its own `notion-create-view` call and wait for it to return before the next. The three databases' default-view renames in substep 1 can run in parallel; nothing else in this step can.

### Step 7. Verify

Fetch the parent page and the three databases and check:
- Page order is Development Notes, Theory Notes, Code Generation Reports, all inline.
- Each title matches Database titles exactly, emoji included.
- Dev ID type is text, and Risk Impact and Risk Link are absent.
- Type has only `Docs`; System Area, Component, Decision, Domain, Related Component and the Code Generation Reports Component have no options.
- Project Phase options match the phase list in all three databases.
- The two-way relation exists on both sides (`Related Theory Notes` and `Related Dev Notes`).
- Each database's views list matches the tab order in View order exactly, with the filters listed there.
- Every view's `displayProperties` matches its order string exactly.

If the tab order is wrong, fix it by position as described in Rules learned from testing, then fetch again to confirm. If a title is missing its emoji, set it with `notion-update-data-source` and `title`. Fix any other mismatch before reporting.

## Output

Report in two or three sentences: the three databases created (with links), the views created, and anything that failed. Do not create sample records.