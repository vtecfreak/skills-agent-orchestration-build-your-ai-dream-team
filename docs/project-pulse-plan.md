# Project Pulse dashboard implementation plan

## Summary

Build Mona's lightweight, static Project Pulse dashboard described in
`.github/project-pulse-brief.md`. The dashboard will load project records from
JSON and present them as responsive, accessible project cards showing each
project's name, owner, status, recent activity, and priority. Use the existing
custom agents for planning, design, implementation, and orchestration. No
application framework or build step is required.

## Ordered implementation steps and file assignments

### 1. Establish the UI and data contracts

- **Designer responsibility:** Define a concise visual and interaction contract:
  page hierarchy, responsive card layout, accessible status and priority
  treatments, loading/error/empty states, and the CSS hooks the markup must use.
  Implement the dashboard styling in `app/styles.css`.
- **Coder responsibility:** Implement `app/project-data.json` with a top-level
  `projects` array. Include representative records with `name`, `owner`,
  `status`, `recentActivity`, and `priority` values.
- **File assignments:** Designer owns `app/styles.css`; Coder owns
  `app/project-data.json`.
- **Dependencies:** The data shape follows the brief. The Designer's CSS hook
  contract is an input to the HTML implementation in step 2.

### 2. Implement the page and runnable launch configuration

- **Coder responsibility:** Create `app/index.html` with semantic page
  structure, a clear Project Pulse heading, and client-side rendering that
  fetches `project-data.json` and creates project cards using the Designer's
  agreed class hooks. Link `app/styles.css`. Provide visible loading, empty,
  and fetch/parse error states so failures are not mistaken for a successful
  dashboard.
- **Coder responsibility:** Create `.vscode/launch.json` with a configuration
  named **Run Project Pulse Dashboard**. Serve the static app with the VS Code
  built-in Node terminal launch type running Python's built-in HTTP server,
  set its working directory to `${workspaceFolder}/app`, and configure
  `serverReadyAction` to open `http://localhost:<port>/index.html`. Keep the
  file strict JSON without comments.
- **File assignments:** Coder owns `app/index.html` and `.vscode/launch.json`;
  Designer does not edit these files.
- **Dependencies:** This step depends on the Designer's agreed markup/CSS hook
  contract and the JSON schema established in step 1. The launch target depends
  on `app/index.html` existing and the server running from `app/`.

### 3. Integrate and validate

- **Orchestrator responsibility:** Check that the markup, data, CSS hooks, and
  launch target agree; resolve integration issues with the assigned owner.
- **File assignments:** Make only targeted corrections in the files above,
  retaining their assigned ownership where possible.
- **Dependencies:** Steps 1 and 2 must be complete.

## Dependencies and parallel work

The Designer and Coder can work in parallel during step 1 because they own
separate files. Before step 2, the Designer must communicate the class-hook and
layout contract, and the Coder must finalize the JSON field/value expectations.
The Coder's HTML work then depends on both contracts. The Coder may draft the
launch configuration alongside the HTML once the app directory and launch
behavior are agreed, but final launch validation depends on the page and static
server being available. Integration and runtime verification happen
sequentially after all implementation files are ready.

## Edge cases and implementation guidance

- Render every required field and treat status/priority labels as text, not
  trusted HTML.
- Handle an empty project list, a missing or malformed `projects` array, and
  failed JSON fetches with understandable visible states.
- Ensure cards remain readable at narrow viewport widths and status/priority
  meaning is conveyed by text and not color alone.
- Use semantic headings and landmarks, keyboard-friendly native elements, and
  sufficient contrast.
- Fetching local JSON requires an HTTP server; opening the HTML directly as a
  `file://` URL is not the expected launch path.
- Use a stable local port and ensure the launch configuration opens the app
  page rather than a directory listing. Avoid adding dependencies or build
  tooling for this static app.

## Validation expectations

- Parse `app/project-data.json` with `python3 -m json.tool`; verify the top-level
  `projects` array and required fields in its records.
- Parse `.vscode/launch.json` with `python3 -m json.tool`; confirm the exact
  **Run Project Pulse Dashboard** name, strict JSON, working directory
  `${workspaceFolder}/app`, server command/port, and browser URL ending in
  `/index.html`.
- Inspect the HTML/CSS for linked stylesheet and data paths, matching
  `.dashboard` and `.project-card` hooks, responsive rules, accessible semantic
  structure, and visible loading/empty/error states.
- Start the dashboard using the launch configuration. Confirm the page opens
  at `/index.html`, project cards show all required data, and no console or
  network errors occur. Also verify the empty/error states if practical.

## Open questions

- The brief does not prescribe project names, record count, status vocabulary,
  priority scale, or a color palette. Use a small representative sample and
  consistent, readable labels unless Mona provides specific values.
- The brief does not prescribe a browser or operating system; use the Codespace
  and VS Code launch behavior as the primary runtime target.
