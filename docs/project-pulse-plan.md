# Project Pulse Dashboard Implementation Plan

## Goal

Build a lightweight static dashboard that helps contributors quickly see active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The first view should be a polished, accessible Project Pulse UI with responsive project cards, not a server directory listing.

## Responsibilities

- **Planner:** Define phases, file ownership, dependencies, edge cases, and validation before implementation.
- **Designer:** Set the dashboard information hierarchy, card layout, typography, spacing, status and priority treatments, responsive behavior, and accessibility guidance. Provide design direction without editing implementation files, so file ownership stays clear.
- **Coder:** Implement the static app and launch support in the assigned files, follow the Designer's direction, handle data-loading and launch errors clearly, and validate the integrated experience.
- **Orchestrator:** Delegate the design and implementation phases, keep the file scopes explicit, sequence dependent work, and verify the completed dashboard.

## Ordered implementation steps and file assignments

1. **Set the design direction — Designer**
   - No file edits.
   - Define an accessible, responsive dashboard structure and a visual treatment for cards, status badges, and priority/risk.
   - Specify how name, owner, status, recent activity, priority, and a concise project summary should be prioritized and read.

2. **Prepare independent app support — Coder**
   - `app/project-data.json`: Create a valid top-level `projects` array with multiple representative projects. Each object includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short contributor-friendly summary as well.
   - `.vscode/launch.json`: Add strict JSON configuration named **Run Project Pulse Dashboard**. Run `python3 -m http.server 5500` with the working directory set to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.
   - These files have no overlap with the Designer's advisory work and can be prepared in parallel with step 1.

3. **Implement the dashboard UI — Coder**
   - `app/index.html`: Build the accessible page structure, use the exact title **Project Pulse**, reference `styles.css` and `project-data.json`, and render visible `.project-card` elements with the required project fields.
   - `app/styles.css`: Implement the Designer-approved visual direction, including `.dashboard` and `.project-card` styles, readable spacing, status/priority distinctions, responsive layout, rounded corners, and subtle shadows.
   - This step depends on the Designer's direction from step 1 and the data shape established in step 2. Keep markup and CSS coordinated through the shared class names.

4. **Integrate and validate — Coder, coordinated by Orchestrator**
   - Confirm data is loaded and rendered, all files agree on the schema and selectors, and launch opens the dashboard page.
   - Resolve any integration or accessibility issues within the four assigned implementation files.

## Dependencies and parallel work decisions

- Step 1 and step 2 can run in parallel: the Designer provides guidance without editing files, while the Coder prepares the data fixture and launch configuration.
- Step 3 must wait for the design handoff and the data contract. The HTML renderer depends on the `projects` array and its field names; CSS depends on the markup classes and visual decisions.
- Step 4 must follow implementation so it can validate the integrated result.
- Do not have Designer and Coder edit `app/index.html` or `app/styles.css` concurrently. If design feedback requires changes after implementation begins, pause the Coder, incorporate the feedback, and then resume.
- The launch configuration can be authored independently of the page, but final launch validation depends on `app/index.html` existing and the server being able to serve it from `app/`.

## Edge cases and implementation notes

- Handle an empty or unavailable project list with a readable message rather than a blank dashboard; surface data-loading errors visibly.
- Keep status and priority understandable through text as well as color, with sufficient contrast and visible keyboard focus.
- Ensure cards reflow at narrow viewport widths without horizontal scrolling and preserve a logical reading order.
- Use a local static server because loading JSON with `fetch` from a `file://` URL may be blocked by browser security rules.
- The repository brief defines the required fields and launch behavior. Exact project examples and a visual theme are open to the Designer/Coder, provided the dashboard remains clear and contributor-friendly.

## Validation expectations

- Parse both JSON files: `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
- Check that each project has `name`, `owner`, `status`, `recentActivity`, and `priority`, and that the top-level key is `projects`.
- Inspect the page to confirm it references both `styles.css` and `project-data.json`, renders project cards, and displays status, recent activity, and priority values.
- Check `.dashboard`, `.project-card`, responsive rules, readable contrast, text labels for status/priority, and keyboard focus behavior.
- Start the **Run Project Pulse Dashboard** launch configuration and confirm it serves from `app/`, opens `/index.html`, and shows the dashboard rather than a directory listing. Stop the server after the smoke test.
- Run the repository's `scripts/validate-exercise.sh` when available to catch exercise-level checks, in addition to the app-specific checks above.
