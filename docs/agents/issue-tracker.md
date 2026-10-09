# Issue tracker: Local Markdown

Issues and specs for this repo live as Markdown files in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`.
- The spec is `.scratch/<feature-slug>/spec.md`.
- Implementation issues are one file per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`.
- Triage state is recorded as a `Status:` line near the top of each issue file. Use the role strings in `docs/agents/triage-labels.md`.
- Each triaged issue has one `Category:` value: `bug` or `enhancement`.
- `Closed: yes` marks a closed spec or issue. `Closed: no`, or an absent field, means open. Keep the triage `Status:` when closing.
- Append comments and conversation history under a `## Comments` heading at the bottom of the file.

## When a skill says "publish to the issue tracker"

Create a spec or issue file at the corresponding path above, creating its directory if needed.

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. If given only an issue number, resolve it within the relevant feature directory.

## Close work and resolve blockers

When work is complete, record the resolution under `## Comments` and set `Closed: yes`. For implementation work, verify the acceptance criteria before closing.

Implementation tickets record dependencies in a `Blocked by:` line, using ticket numbers or paths. A ticket is unblocked when every listed blocker has `Closed: yes`; `None` means it can start immediately.

## Wayfinding operations

Used by `/wayfinder`. The map is a file with one child file per ticket.

- **Map**: `.scratch/<effort>/map.md`, holding the Notes / Decisions-so-far / Fog body.
- **Child ticket**: `.scratch/<effort>/issues/<NN>-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records `research`, `prototype`, `grilling`, or `task`. Wayfinding uses its own `Status:` values: `open`, `claimed`, and `resolved`.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `.scratch/<effort>/issues/` for tickets that are `open` and unblocked; first by number wins.
- **Claim**: set `Status: claimed` and save before starting work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved` and `Closed: yes`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
