--- Begin file content (docs/project-pulse-plan.md) ---
# Project Pulse — Implementation Plan

Version: 1.0  
Author: Planner sub-agent  
Purpose: A practical, phase-by-phase roadmap to implement the "Project Pulse" dashboard web app, with clear file ownership and acceptance criteria so Designer and Coder can work with minimal friction.

---

Table of contents
- High-level overview and goals
- File assignments (explicit)
- Designer vs Coder responsibilities (distinct deliverables)
- Phase-by-phase plan (numbered phases)
- Parallel vs sequential work decisions
- External & internal dependencies and setup steps
- Validation expectations (unit tests, E2E/smoke, manual QA)
- Edge cases and error states
- Deliverables and final acceptance checklist summary
- Rough total time estimate
- Open questions

---

High-level overview and goals
- Project Pulse is a lightweight, static single-page dashboard that visualizes current projects and their key metrics (status, health, owner, progress, recent activity).
- Primary goals:
  - Deliver a responsive, accessible dashboard that reads a static data file (app/project-data.json).
  - Simple, clear visuals (cards / summary tiles / bar or sparkline) that let stakeholders quickly see project health.
  - A minimal local developer experience with a debug launch configuration (.vscode/launch.json).
  - Clear design handoff and separation of Designer and Coder responsibilities so parallel work is efficient.

Success criteria (summary)
- The app opens in a dev browser, loads project-data.json, and renders the dashboard with correct counts and at least one visual metric per project.
- Responsive breakpoints are tested; critical accessibility checks pass (keyboard navigation, semantic HTML, contrast).
- Launch configuration allows starting a local dev server and opening the app from VS Code.

---

Explicit file assignments (these exact file paths must be owned and updated as listed)
Note: Responsibilities specify Owner(s) (Designer, Coder). Files and tasks below are exhaustive and precise.

1) app/index.html
- Purpose: The single-page application shell. Contains semantic HTML structure, placeholders or containers for dashboard components, minimal JS bootstrap to fetch app/project-data.json and mount UI.
- Owner(s): Designer + Coder (joint)
- Designer tasks:
  - Provide final HTML wireframe/annotated template showing semantic sections (header, main, nav/filters, cards grid, footer).
  - Provide accessibility notes for the structure (landmarks, ARIA where necessary).
  - Supply visual tokens: color palette, spacing scale, typography tokens (font-size scale), and example content for cards in a short "content spec" document or annotated HTML snippet.
- Coder tasks:
  - Implement the HTML shell consistent with the Designer’s wireframe and content spec.
  - Implement a lightweight initialization script (inline or referenced) that fetches project-data.json, validates it, and renders JSON-driven UI placeholders.
  - Hook semantic ids/classes used by app/styles.css and add small JS hooks for interactivity (filter toggles, sort).
- Acceptance of changes to this file requires both Designer sign-off (layout/semantics) and Coder sign-off (data binding works).

2) app/styles.css
- Purpose: All visual styling and responsive rules for the dashboard — layout, color usage, typography, spacing, breakpoints, and simple visual treatments for project cards and metrics.
- Owner(s): Designer (primary) — Coder may make minor edits only for integration purposes.
- Designer tasks:
  - Create a full stylesheet implementing the visual design tokens and component styles consistent with the index.html structure.
  - Provide comments in the CSS describing token usage and responsive breakpoint rationale.
  - Include accessibility-focused styles (focus states, reduced-motion, high-contrast fallback).
- Coder tasks:
  - Integrate the stylesheet into index.html and make minimal, well-documented adjustments only if needed for JavaScript-driven classes/state toggles.
- Deliverable: A single well-commented app/styles.css that renders the approved visuals when loaded by index.html.

3) app/project-data.json
- Purpose: Static canonical data source for the dashboard. A small JSON array of project objects (id, name, owner, status, progressPercent, lastUpdated, tags, healthScore, optional metrics).
- Owner(s): Coder
- Coder tasks:
  - Define a small, strict JSON schema (in comments or separate doc) and provide a populated example file with 6–10 diverse projects demonstrating edge conditions: 0% and 100% progress, missing optional fields, older dates, special characters in text.
  - Keep file size modest (< 100KB). Use realistic but synthetic data.
  - Implement simple schema validation in the app bootstrap to produce helpful errors if file format changes.
- Designer tasks:
  - Provide any copy/content corrections (project naming conventions, owner display names) for the sample content.
- Acceptance: JSON parses cleanly in modern browsers, conforms to schema, and every example rendered in the UI maps correctly to displayed fields.

4) .vscode/launch.json
- Purpose: VS Code debug/launch configuration that starts or attaches to a local dev server and opens app/index.html in the default browser or a debugging browser tab.
- Owner(s): Coder
- Coder tasks:
  - Provide a launch.json configuration that either:
    - Launches a small Node-based local static server (http-server, live-server, or a simple npm script) and opens the page, or
    - Attaches a browser debug session (e.g., using the "pwa-chrome" type) to the running server.
  - Document expected npm scripts in package.json (if used), or include instructions in README.md for running the server.
  - Ensure the launch configuration works cross-platform (Windows/macOS/Linux) where possible and includes fallback steps in comments if an extension is required.
- Acceptance: Running the VS Code launch configuration opens the Project Pulse home page and attaches the debugger (if requested) without additional manual steps beyond "Install npm deps" and "Run".

5) docs/project-pulse-plan.md
- Purpose: This plan — created and owned by the Planner sub-agent. It is not to be edited by Designer or Coder except for factual corrections.
- Owner(s): Planner (author), Reviewer(s): Designer, Coder
- Tasks: Use it as the authoritative plan and checkpoint list for implementation.

---

Designer vs Coder — distinct responsibilities and deliverables

Designer (deliverables)
- Visual design system: color palette, typography scale, spacing scale, responsive breakpoints.
- Annotated HTML wireframe (for index.html structure).
- Fully implemented app/styles.css (production-ready, commented).
- Image/icon assets or references (SVGs or icon fonts) or instructions to use system icons.
- Accessibility guidance (focus styles, color contrast targets, ARIA requirements).
- Visual acceptance sign-off checklist.

Coder (deliverables)
- Implemented app/index.html scaffold that binds to data and contains semantic markup.
- app/project-data.json sample file and an inline or separate schema definition.
- JavaScript to fetch, validate, and render the data into the DOM (can be vanilla JS).
- .vscode/launch.json to start / attach to a dev server for debugging.
- Automated validation artifacts: minimal unit tests for the data schema or data parsing; simple E2E smoke test(s) to assert the dashboard renders.
- README or dev notes describing how to run the dev server, tests, and launch config verification steps.

Designer and Coder collaboration points
- Wireframe -> HTML mapping (Designer produces, Coder implements).
- CSS token names and class naming conventions (agreed before heavy coding).
- JSON field naming (Coder must choose schema; Designer needs to know labels they will surface).
- Accessibility checks and final visual sign-off.

---

Phase-by-phase plan (numbered phases)
Each phase lists: tasks, assigned specialist(s), files touched, dependencies, time estimate (order-of-magnitude), deliverables, and acceptance checklist.

Notes on estimates:
- Estimates are order-of-magnitude, listed in hours (h) and days (d) where helpful for planning.
- "Quick" means < 4h, "Small" ~4–8h, "Medium" ~1–2d, "Large" ~3–5d.
- Adjust based on team velocity and local working context.

Phase 1 — Kickoff & discovery
- Tasks:
  - Align scope, success criteria, and sample dataset needs in a short kickoff meeting or document.
  - Designer provides a 1–2 page brief describing the intended visual language and major UI components.
  - Coder assesses repository and identifies tooling choices (dev server, test runner).
- Assigned: Planner (facilitate), Designer, Coder
- Files touched: docs/project-pulse-plan.md (review), README.md (update dev run notes if needed)
- Dependencies: none
- Time estimate: Small — 2–4h
- Deliverables:
  - Confirmed scope and acceptance checklist.
  - Draft design brief and agreed JSON fields list.
- Acceptance checklist:
  - All participants agree on data fields, page sections, and minimal tech stack.
  - JSON field list confirmed.

Phase 2 — Design wireframe and tokens
- Tasks:
  - Designer builds HTML wireframe mock (annotated), creates color/type/spacing tokens, and drafts app/styles.css baseline (variables and component scaffolding).
  - Provide expected class names for integration points (e.g., .project-card, .status-pill, #projects-grid).
- Assigned: Designer
- Files touched: app/index.html (designer mock snippet—may be a separate file or annotated HTML snippet delivered to the Coder), app/styles.css (initial draft)
- Dependencies: Phase 1
- Time estimate: Medium — 1–2d
- Deliverables:
  - Annotated HTML wireframe and tokens.
  - Draft app/styles.css implementing tokens and baseline layout rules.
- Acceptance checklist:
  - Wireframe maps to components in the acceptance checklist.
  - CSS includes tokens and breakpoints, and comments for integration.

Phase 3 — Data schema and sample data
- Tasks:
  - Coder designs a small JSON schema for project objects, creates app/project-data.json with sample data covering edge states.
  - Implement basic JSON validation rules and a README snippet describing fields.
- Assigned: Coder
- Files touched: app/project-data.json, README.md (dev instructions, optional)
- Dependencies: Phase 1
- Time estimate: Quick — 3–6h
- Deliverables:
  - app/project-data.json (validated sample with diverse cases).
  - Schema documentation/comments.
- Acceptance checklist:
  - JSON parses in browser and in Node (JSON.parse).
  - Sample covers required edge conditions (zero projects, missing optional fields, special characters).

Phase 4 — Development scaffold & dev server
- Tasks:
  - Coder creates or documents a simple dev server workflow (npm scripts using http-server/live-server OR instructions for using a VS Code static server extension).
  - Create .vscode/launch.json that runs/attaches to the dev server and opens index.html.
  - Add dev notes in README.md to run server and verify launch.json.
- Assigned: Coder
- Files touched: .vscode/launch.json, README.md (or docs/developer-run.md)
- Dependencies: Phase 3
- Time estimate: Small — 4–8h
- Deliverables:
  - Working launch.json that starts server and opens app.
  - npm script examples or documented alternative.
- Acceptance checklist:
  - Launching from VS Code opens the app and, optionally, attaches debugger.
  - Steps to run the server recorded in README.

Phase 5 — Implement index.html + minimal JS runtime
- Tasks:
  - Coder implements the semantic index.html shell per Designer wireframe.
  - Implement JS to fetch app/project-data.json, do validation, render placeholder UI (cards grid), and basic interactivity (sort/filter).
  - Use semantic markup with accessible patterns (landmarks, roles).
- Assigned: Coder (+ Designer review of layout)
- Files touched: app/index.html, app/styles.css (minor integrations), app/project-data.json
- Dependencies: Phase 2, Phase 3, Phase 4
- Time estimate: Medium — 1–2d
- Deliverables:
  - Functional HTML page that renders content from project-data.json and responds to basic UI actions.
- Acceptance checklist:
  - The page loads with no console errors.
  - Sample data renders expected fields.
  - Filter/sort interactions work and are keyboard accessible.

Phase 6 — Styling polish & responsive work
- Tasks:
  - Designer finalizes app/styles.css and reviews implementation on breakpoints (desktop/tablet/mobile).
  - Coder integrates any interactive state classes used by JS and resolves any layout issues.
- Assigned: Designer (primary), Coder (integration)
- Files touched: app/styles.css, app/index.html
- Dependencies: Phase 5
- Time estimate: Medium — 1–2d
- Deliverables:
  - Finalized styles matching the design tokens, responsive behavior implemented.
- Acceptance checklist:
  - Visual sign-off from Designer on desktop and mobile breakpoints.
  - No layout shift or overflow on typical viewports.

Phase 7 — Accessibility, edge-case handling & error states
- Tasks:
  - Implement keyboard navigation for interactive controls.
  - Provide visible focus styles and reduced-motion support.
  - Implement friendly error messaging and fallback UI for when project-data.json fails to load or is malformed.
  - Coder to add JSON validation and user-facing fallbacks.
- Assigned: Designer (accessibility guidance), Coder (implementation)
- Files touched: app/index.html, app/styles.css, app/project-data.json (edge-case entries to test)
- Dependencies: Phases 3–6
- Time estimate: Small — 4–8h
- Deliverables:
  - Accessible and robust UI with clear error handling and documented fallback behavior.
- Acceptance checklist:
  - Keyboard-only navigation exercises pass.
  - Screen reader basics (landmark reading) verified.
  - App displays a friendly error when JSON fails to load or is invalid.

Phase 8 — Automated validation & smoke tests
- Tasks:
  - Add a minimal unit test for JSON parsing/validation (Node + jest or node runner).
  - Add a simple E2E/smoke test that runs a dev server, opens the page in headless browser, and asserts that at least one project-card exists and key counts are visible.
  - Document test run commands.
- Assigned: Coder
- Files touched: new tests (tests/...), package.json (scripts), README.md
- Dependencies: Phases 3–6
- Time estimate: Small — 6–12h
- Deliverables:
  - Unit test(s) and at least one E2E smoke test with documented run steps.
- Acceptance checklist:
  - Tests run locally and pass on a dev machine with Node/npm installed.
  - Smoke test fails gracefully if server isn't running and gives clear instructions.

Phase 9 — QA, polish, handoff & documentation
- Tasks:
  - Designer performs final visual QA and collates any copy updates.
  - Coder fixes bugs identified by QA and finalizes README + runbook (how to run, debug, extend).
  - Prepare final delivery package: final app files, tests, and launch.json.
- Assigned: Designer & Coder
- Files touched: app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json, README.md
- Dependencies: All prior phases
- Time estimate: Medium — 1–2d
- Deliverables:
  - Finalized app and docs ready for staging or archive.
- Acceptance checklist:
  - All tests pass.
  - Designer sign-off on visuals and accessibility.
  - Coder sign-off on dev workflow and launch.json.

Phase 10 — Final acceptance & retrospective
- Tasks:
  - Stakeholder review, acceptance testing, short retrospective and list of follow-ups.
- Assigned: Stakeholders, Designer, Coder
- Files touched: docs/project-pulse-plan.md (close out), README.md (versions)
- Dependencies: All prior phases
- Time estimate: Quick — 2–4h
- Deliverables:
  - Formal acceptance notes and list of follow-up improvements.
- Acceptance checklist:
  - Stakeholders confirm the app meets the success criteria listed above.

---

Decisions about parallel vs sequential work (with rationale)
- Parallel work recommended:
  - Designer (styles & tokens) can work in parallel with Coder building the data schema and dev server. Rationale: style definitions and JSON schema are largely independent; integration points are class names and field names which should be agreed early in Phase 1.
  - Designer can deliver the CSS while the Coder builds the scaffold and JSON sample — this reduces idle time.
- Sequential work required:
  - Coder’s rendering logic must wait for the agreed JSON schema (Phase 3) and wireframe (Phase 2) to be stable.
  - Final styling polish (Phase 6) is sequential after the functional rendering is working (Phase 5).
- Recommended workflow:
  - Start Phase 2 & Phase 3 immediately after Kickoff; Phase 4 and 5 can start as soon as the initial CSS tokens and JSON schema are available.

---

External and internal dependencies and setup steps
Recommended toolchain (minimal, pragmatic)
- Node.js (LTS recommended, e.g., >= 16.x or 18.x)
- npm (bundled with Node) or yarn
- A small static server for local dev:
  - Options: http-server, live-server, serve, or VS Code "Live Server" extension.
  - Recommended: Add a dev dependency script like "start": "http-server ./app -o" or "start": "live-server ./app --port=8080".
- Testing:
  - Unit tests: jest or mocha (if JS logic complex).
  - E2E tests (optional): Playwright or Puppeteer for a single smoke test.
- VS Code:
  - launch.json to configure pwa-chrome or to run an npm script then attach debugger.
  - Ensure team members have the "Debugger for Chrome" functionality available (built-in in modern VS Code via pwa-chrome); document if any extension is required.
- Steps to set up locally (developer instructions):
  1. Install Node.js (LTS).
  2. Clone repository.
  3. cd repo root; run npm install (if package.json is provided).
  4. Run npm run start (or use the provided VS Code launch.json to start the server).
  5. Open http://localhost:PORT/ (or let launch.json open it).
  6. Run tests with npm test (if present).

Notes about .vscode/launch.json verification
- The launch configuration should:
  - Option A: Run the npm start script before launching the browser (preLaunchTask or runtimeExecutable).
  - Option B: Attach to an existing running server with "request": "attach" and "port" configured appropriately.
  - Include a fallback comment: "If launch fails, run npm start in terminal and then use 'Attach to Chrome' configuration."
- To verify:
  1. From VS Code, select the Project Pulse launch target and run it.
  2. Confirm VS Code either launches the server or attaches to a running server.
  3. Confirm the browser tab opens to index.html and the debugger is optionally attached (breakpoints on the app init code should pause if breakpoints set).
  4. Document OS-specific notes if any issues.

---

Validation expectations (how to verify correctness)

Unit tests
- Purpose: Validate JSON schema parsing, required field presence, and basic data transformations.
- Examples:
  - test/jsonSchema.parse: parse app/project-data.json and assert required fields exist.
  - test/dataTransforms: transform lastUpdated to Date and assert expected ISO parsing on at least one sample entry.
- Automation: One or two Jest tests running in Node.

E2E / Smoke tests
- Purpose: Confirm that the web app renders and key elements exist.
- Minimal smoke test:
  - Start dev server programmatically.
  - Open headless Chromium (Playwright/Puppeteer).
  - Navigate to http://localhost:PORT/.
  - Assert that the projects grid contains at least one project-card and the top summary count equals the expected value from project-data.json.
- Acceptance: Smoke test passes in CI or locally in a reproducible way; if unavailable, document manual steps.

Manual QA checks
- Visual:
  - Desktop/tablet/mobile breakpoints behave correctly with no overlapping content.
  - Focus outline visible and usable for keyboard navigation.
  - No severe color contrast failures (contrast ratio >= 4.5:1 for body text is preferred).
- Functional:
  - Filters and sorts work.
  - Error states show user-friendly messages.
  - Launch config opens the page and debugger attaches (if requested).
- Accessibility:
  - Basic screen reader walkthrough of the main page reads the major landmarks and project headings reasonably.
  - Reduced motion respects system-level preferences.

Acceptance criteria (final)
- The app loads and renders data from app/project-data.json with no console errors.
- Responsive layout passes Designer sign-off at key breakpoints.
- Accessibility checks (keyboard navigation, focus states) pass the checklist.
- Unit and smoke tests (if present) run and pass locally.
- .vscode/launch.json opens the app from VS Code as documented.

How to verify .vscode/launch.json
- Run launch target in VS Code; confirm server startup and tab open.
- Set a breakpoint in the initialization JS; run the launch to confirm the debugger halts.
- If launch.json invokes an npm script, verify npm dependencies are installed and script runs as expected.

---

Edge cases and error states to consider
- project-data.json missing or 404:
  - Show a friendly banner and a retry button; log error to console with troubleshooting instructions.
- JSON malformed:
  - Show an explicit parse error message in the UI with a helpful link to expected schema.
- Empty projects array:
  - Show a clear empty state with suggested actions (e.g., "No active projects — check filters or add sample data").
- Very large number of projects:
  - Performance: implement virtualization or pagination if > 200 items (note: for MVP, document limits and fallback).
- Missing optional fields (owner, tags):
  - Render with sensible defaults (e.g., "Owner: Unassigned").
- Non-ISO date strings:
  - Validate and normalize dates; display "Unknown date" if unparsable.
- Special characters & right-to-left text:
  - Ensure correct encoding and CSS supports overflow and wrapping.
- Slow network:
  - Show loading skeletons; timeouts with retry.
- Browser compatibility:
  - Support modern evergreen browsers. Document unsupported browsers and minimal polyfills if necessary.
- Security:
  - Avoid injecting raw HTML from project data (escape user content).
  - For local static site, ensure no XSS via files.

---

Work that can run in parallel
- Designer finalizes tokens/styles while Coder:
  - Creates project-data.json and dev server configuration.
  - Builds initial rendering scaffold (index.html with placeholder content).
- Writing unit tests (Coder) can begin once data schema is stable, before finish of all UI polish.
- Accessibility guidance from Designer can be a running document used while Coder implements features.

Work that must run sequentially
- Final integration (JS rendering hooking into exact CSS class names and markup) is sequential — CSS and HTML wireframe must be stable.
- Launch.json validation requires the dev server configuration to be in place.

---

Deliverables and final acceptance checklist summary
Deliverables (by role)
- Designer:
  - Annotated HTML wireframe
  - app/styles.css (final)
  - Visual tokens and assets
  - Accessibility sign-off
- Coder:
  - app/index.html (functional, semantic)
  - app/project-data.json (schema + sample data)
  - .vscode/launch.json (dev/workflow)
  - Minimal tests (unit + smoke)
  - README.md dev/run/test instructions
- Planner:
  - docs/project-pulse-plan.md (this document)

Final acceptance checklist (all must pass)
- App launches from VS Code using .vscode/launch.json (or instructions produce same result).
- Dashboard loads and renders data from app/project-data.json with no console errors.
- Responsive layout passes Designer sign-off.
- Accessibility: keyboard navigation and focus states pass the checklist.
- Unit test(s) and at least one smoke E2E test pass.
- Error and empty states correctly handled and documented.
- README contains clear run/test/debug instructions.

---

Rough total time estimate (order-of-magnitude)
- Phase totals (rounded):
  - Phase 1: 0.5 day
  - Phase 2: 1–2 days
  - Phase 3: 0.5 day
  - Phase 4: 0.5–1 day
  - Phase 5: 1–2 days
  - Phase 6: 1–2 days
  - Phase 7: 0.5–1 day
  - Phase 8: 0.5–1.5 days
  - Phase 9: 1–2 days
  - Phase 10: 0.25 day
- Total: ~7–12 working days (rough) or ~56–96 hours depending on depth of polish and test automation level.

Lower end = quick MVP with minimal tests and a single smoke test. Upper end = polished, fully accessible, and tested deliverable.

---

Open questions (for quick resolution during kickoff)
1. What is the canonical scope of metrics to show per project? (e.g., healthScore, progressPercent, lastUpdated, owner, status).
2. Should the app be purely static (no build step) or can we add a small build step (Webpack/Vite) later? MVP assumes static files and minimal tooling.
3. Are there branding constraints (fonts, logo) that must be respected or supplied?
4. Is there a target browser baseline to support (Chrome/Edge only vs Safari/Firefox/IE11)?
5. Will this be integrated into a larger site later (affecting routing/base href expectations)?
6. Should we include a simple CI check for the smoke test? If so, which CI provider is expected?

---

Notes and implementation guidance (non-prescriptive)
- Keep the implementation minimal: prefer vanilla JS for fetching and DOM rendering for the MVP unless the project already uses a framework.
- Keep CSS modular and tokenized so it is easy to convert to a component system later.
- Use feature-detection and progressive enhancement: ensure the page renders useful info with JS disabled (where feasible) or at least shows a clear requirement message.

---

Appendix: Quick JSON schema suggestion (for Coder use — high level)
- Project object keys (required): id (string), name (string), status (enum: active, paused, complete), progressPercent (number 0–100), healthScore (number 0–100), lastUpdated (ISO8601 string)
- Optional keys: owner (string), tags (array of strings), description (string), metrics (object)
- Provide sample entries in app/project-data.json covering:
  - A project with missing owner
  - A project with 0% and 100% progress
  - A project with very old lastUpdated date
  - Special characters or long names

---

If anything above is unclear or you want the plan adjusted to a different target level (MVP vs production-ready), tell me which focus you want (Minimal, Balanced, or Production) and I will re-scope time estimates and tests accordingly.

--- End file content (docs/project-pulse-plan.md) ---

{"path":"docs/project-pulse-plan.md","bytes":8800,"summary":"Project Pulse plan: clearscope, file ownership, Designer/Coder deliverables, and a phased roadmap. Includes tasks, dependencies, tests, edge cases, and VS Code launch guidance. Estimated 7–12 days to implement with checkpoints and acceptance criteria."}