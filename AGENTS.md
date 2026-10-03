# AGENTS.md

## Project

Name: `ws-tech-support`

Purpose:

`Standalone bilingual WaterSense/Yunnan technical support matrix served as a static HTML page.`

## Source of Truth

- Git history is authoritative.
- `PROJECT_STATE.md` describes the current working state.
- Active complex work may have a plan under `.ai/plans/`.
- Do not rely on old chat history when repository evidence is available.

## Engineering Rules

- Make the smallest correct change.
- Do not modify unrelated code.
- Preserve backward compatibility unless the task explicitly authorizes a breaking change.
- Do not silently change database schemas, public APIs, configuration formats, or persistent data.
- Add or update tests for bug fixes and behavior changes.
- Prefer existing project patterns over introducing new frameworks or abstractions.
- Never hide failing tests.
- Document uncertainty instead of guessing.

## Validation

Before declaring a task complete:

1. run relevant tests
2. run the broader test suite when practical
3. run lint/type/build checks used by this repository
4. inspect the resulting diff
5. verify acceptance criteria

Project-specific commands:

```bash
# No automated suite or build script exists.
# Inspect index.html and verify it in a browser.
```

## Checkpoint Protocol

A checkpoint is required when:

- a task completes
- a milestone completes
- a blocker is discovered
- a major design decision is made
- the agent is about to switch tasks
- the agent is about to end the session
- usage limits are approaching
- work must be handed to another agent/model

At a checkpoint:

1. update `PROJECT_STATE.md`
2. update the active task plan if one exists
3. record validation results
4. record unresolved issues
5. record one exact next action
6. commit code and state documentation together when appropriate

## Session Resume Protocol

At the beginning of a new engineering session:

1. read `AGENTS.md`
2. read `PROJECT_STATE.md`
3. read the active task plan if referenced
4. inspect `git status`
5. inspect current branch and HEAD
6. compare HEAD with the state recorded in `PROJECT_STATE.md`
7. inspect commits made after the recorded state commit if they differ
8. run minimum baseline validation when the project has been dormant or state is uncertain
9. continue from `Exact Next Action`

## Session Boundary

Continue the same session when the work is a direct continuation of the same task.

Start a fresh session when:

- the current task is complete
- switching to a different feature or bug
- the existing context contains multiple obsolete approaches
- the agent begins confusing old and current decisions
- excessive logs/history are reducing clarity
- a clean handoff would be cheaper and safer than retaining conversation context

## Handoff Standard

Before stopping meaningful work, leave the repository in a state where a new agent with no chat history can continue safely.

The handoff must answer:

- What was the objective?
- What changed?
- What passed?
- What failed?
- What remains unresolved?
- What should happen next?

## Repository-specific boundaries

- Preserve existing `.atoms/`, task/acceptance documents and nested repository instructions; reconcile stale claims against code.
- The global queue is maintained in the private [AI-Development-Control repository](https://github.com/akaghostwolf/AI-Development-Control); do not copy it into this project.
- No bootstrap step authorizes application PR integration, production deployment, persistent-data changes, credentials, paid services or private-data transmission.
- Use an isolated branch for future work, preserve concurrent edits, and verify the remote HEAD before pushing; never force-push over another agent.
- `State Commit` records the inspected code baseline before the checkpoint commit. Find the enclosing state checkpoint with `git log -1 -- PROJECT_STATE.md`; reconcile subsequent commits before resuming.
- A clean bootstrap clone does not imply that other local checkouts are clean; read Uncommitted Work.
