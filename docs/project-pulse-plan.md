# Project Pulse implementation plan

## Goal and current state

Build a small, polished static dashboard that lets Mona's contributors quickly
understand which projects are active, who owns them, their current status,
recent activity, priority or risk, and a short contributor-friendly summary.
The dashboard must open its UI from `app/index.html`, rather than exposing a
server directory listing.

The repository currently has an empty `app/` directory, Markdown documentation
under `docs/`, and `.vscode/tasks.json`. The requirements and orchestration
workflow are defined in `.github/project-pulse-brief.md` and the exercise
steps. Keep this implementation dependency-free: use HTML, CSS, JSON, and the
existing Python HTTP server launch workflow.

## Roles and file ownership

| Role | Responsibilities | Owned files |
| --- | --- | --- |
| Planner | Turn the brief into ordered phases, identify risks and dependencies, and define completion checks without implementing the app. | `docs/project-pulse-plan.md` |
| Designer | Define information hierarchy, card layout, spacing, typography, color and status treatment, accessibility expectations, responsive behavior, and any interaction guidance needed by contributors. | Design direction and review; no additional implementation file required |
| Coder | Implement the semantic HTML, visual system, project data, and launch configuration; connect data to rendered cards and perform technical validation. | `app/index.html`, `app/styles.css`, `app/project-data.json`, `.vscode/launch.json` |
| Orchestrator | Coordinate the specialists, resolve handoffs, protect file boundaries, integrate decisions, and run final launch validation. | Integration and review across the assigned files |

The Coder owns the implementation files to avoid conflicting edits. The
Designer supplies decisions and reviews the resulting UI rather than changing
the Coder's files unless explicitly assigned an implementation edit.

## Ordered implementation phases

### Phase 1: Confirm requirements and acceptance criteria

**Owner:** Planner, coordinated by Orchestrator

1. Read `.github/project-pulse-brief.md`, the exercise steps, the custom agent
   definitions, and the existing `docs/agent-team.md`.
2. Confirm that the product is a static dashboard and that no framework,
   package installation, build step, or backend is needed.
3. Treat the file list and validation gates below as the contract for the
   specialist handoff.

**Exit criteria:** the Designer and Coder have the same required file paths,
data shape, exact launch name, server command, URL, and UI content
requirements.

### Phase 2: Make design and implementation decisions

**Owners:** Designer and Coder, coordinated by Orchestrator

The Designer should specify:

- A clear page hierarchy: dashboard title and context first, followed by a
  readable grid of project cards.
- A card treatment that makes project name, owner, status, recent activity,
  priority, and summary easy to scan.
- A restrained visual system with consistent spacing, typography, contrast,
  borders or shadows, and distinct but understandable status/priority badges.
- Semantic landmarks, heading order, meaningful labels, keyboard-friendly
  controls if any interactions are introduced, visible focus states, and
  non-color-only status communication.
- A responsive layout that works from narrow mobile widths through wide
  desktop screens without horizontal scrolling or unreadable cards.
- Interaction guidance only where it improves contributor scanning; keep the
  first version simple and functional as a static page.

The Coder should concurrently draft the JSON contract and launch configuration,
including how `index.html` will fetch and render project records.

**Exit criteria:** the visual direction is specific enough to implement, and
the data/config drafts establish stable interfaces for the HTML.

### Phase 3: Implement the static dashboard

**Owner:** Coder

Create the following files:

- **`app/index.html`** - Build a semantic dashboard document with the exact
  visible/page title `Project Pulse`. Reference `styles.css` and
  `project-data.json`. Include accessible headings and labeled project
  metadata, then load the JSON and render one visible `.project-card` per
  project. Every card must show the project's status, recent activity,
  priority, owner, and contributor-friendly summary. Handle a failed data
  load visibly and explicitly rather than silently rendering an empty
  dashboard.
- **`app/styles.css`** - Implement the Designer's visual system and responsive
  layout. Include `.dashboard` for the main dashboard layout and
  `.project-card` for each card. Provide polished card styling with
  `border-radius`, `box-shadow`, clear spacing, readable typography, badge
  treatments, sufficient contrast, visible focus states, and responsive
  breakpoints or fluid layout behavior.
- **`app/project-data.json`** - Write valid JSON with a top-level `projects`
  array containing multiple useful sample projects. Each item must include
  `name`, `owner`, `status`, `recentActivity`, `priority`, and a
  contributor-friendly summary field. Keep values concrete and suitable for
  display; use consistent types and vocabulary for status and priority.
- **`.vscode/launch.json`** - Write strict JSON with no comments. Add a
  configuration named `Run Project Pulse Dashboard` that serves `app/` using
  `python3 -m http.server 5500` and uses `serverReadyAction` to open
  `http://localhost:%s/index.html`. The working directory must ensure the
  server root is `app/`, and the browser URL must target `index.html`.

**Exit criteria:** all four assigned files exist, the data is wired to the
page, project cards are visible from the data, and the launch configuration
targets the dashboard UI.

### Phase 4: Integrate and validate

**Owner:** Orchestrator with Coder and Designer review

1. Compare the implementation against the brief, the Designer's decisions,
   and every acceptance check in this plan.
2. Resolve integration issues in the order of data shape, rendering,
   styling/accessibility, then launch behavior.
3. Start the configured server and confirm the browser-facing response is
   `index.html` and displays Project Pulse, not a directory listing.
4. Record any known limitation or follow-up in the Orchestrator's final
   handoff, without expanding this static-dashboard scope.

## Dependencies and sequencing

- Repository requirements and exercise steps must be reviewed before
  specialist work begins.
- Designer's layout and accessibility decisions must be available before the
  final HTML/CSS integration.
- The data shape must be agreed before HTML rendering is finalized, so every
  required field has a stable display location.
- All four implementation files must be present before launch validation.
- Designer UI direction can run in parallel with the Coder drafting the JSON
  schema and launch configuration; these tasks do not edit the same files.
- After those decisions, HTML/CSS/data integration and final launch testing
  are sequential. Rendering must be integrated before the visual review is
  meaningful, and the complete file set must exist before starting the server.

## Validation expectations

### Content and data

- Parse `app/project-data.json` with a standard JSON parser.
- Confirm the parsed value has a top-level `projects` array with multiple
  records.
- Confirm every record has `name`, `owner`, `status`, `recentActivity`,
  `priority`, and a contributor-friendly summary.
- Confirm each record produces a visible card with the exact
  `project-card` class and displays status, recent activity, and priority.

### HTML, styling, and accessibility

- Confirm `app/index.html` contains the exact title `Project Pulse` and
  references both `styles.css` and `project-data.json`.
- Inspect the rendered page for semantic structure, heading order, readable
  labels, keyboard focus visibility, adequate contrast, and status/priority
  communication that does not rely on color alone.
- Inspect narrow and wide viewport layouts for readable cards, usable spacing,
  no horizontal overflow, and a coherent responsive grid.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, plus the
  intended badge, rounded-card, shadow, and responsive rules.

### Launch behavior

- Parse `.vscode/launch.json` as strict JSON and confirm it contains no
  comments or syntax errors.
- Confirm the configuration name is exactly `Run Project Pulse Dashboard`.
- Confirm it runs `python3 -m http.server 5500` with `app/` as the served
  directory.
- Start the server using the launch configuration and request
  `http://localhost:5500/index.html`.
- Confirm the response is the Project Pulse dashboard and not an app
  directory listing; stop the server after the check.

## Completion definition

The plan is complete when the Designer's hierarchy and accessibility
decisions are reflected in a responsive polished dashboard, the Coder's four
files satisfy the data and launch contracts, and the Orchestrator has passed
the content, rendering, accessibility, responsive, JSON, and launch checks
above.
