---
name: remember
description: Remember a task across sessions using project-local state. Explicitly invoke to resume saved work, save a checkpoint, or set up task memory; saving includes first-use setup.
disable-model-invocation: true
license: MIT
metadata:
  opencode/autoinvoke: "false"
  source: "Lauren Tan, pstack: session pickup and recall; Matt Pocock, skills: context persistence."
---

# Remember

Use the harness's skill command or picker at the start of each session that
should use task memory. No global instructions are required. Choose the action
from the user's request:

- **Save** (default for an ongoing task): ensure setup below, then save. This
  authorizes first-use setup without a separate setup request.
- **Resume / continue a ticket or topic:** load existing state using the resume
  procedure below; do not treat loading as a checkpoint request.
- **Setup:** enable project memory without creating a task.

If no task or action can be inferred, ask rather than creating an empty task.
Existing files alone do not activate memory for a fresh session. After saving or
resuming, follow the protocol for ongoing updates in this session, subject to the
user's preferences. Setup alone prepares files; it does not select a task.

## Session preferences

Check the user's session preferences before reading project memory:

- "Don't use persistent memory this session": do not read, create, or update
  the project protocol, task state, artifacts, or cross-task memory. Leave
  existing files untouched, including at handoff; do not save an opt-out marker.
- "Read saved state, but don't update memory": read relevant existing context,
  but perform no setup, ignore-rule edits, or state writes.
- A general no-write request also forbids setup and state writes; return a
  proposed handoff in chat if requested.

These preferences override routine maintenance for the session, until the user
changes them. Do not treat unrelated progress as permission to re-enable memory.
Never change application source, staging, or global agent instructions; do not
commit or archive. Never invoke another skill automatically.

## Set up project task memory

1. Resolve the target project root; use the Git worktree root when applicable,
   unless the user explicitly targets a nested project. Keep all state paths
   inside that project; do not follow symlinks outside it.
2. In Git projects, check `git ls-files -- .local/agents/` from the project root.
   If state is tracked, report it and stop setup without untracking anything.
   Check ignore coverage with `git check-ignore .local/agents/README.md` and any
   existing state files. Preserve broader rules that already cover the directory.
3. If coverage is missing, append `/.local/agents/` to the project's `.gitignore`,
   preserving its contents and final newline. This edit is authorized by the
   request to remember the task or set up memory. If the user specifically wants a local Git exclude instead,
   resolve it with `git rev-parse --git-path info/exclude`; account for its
   repository-relative patterns. Recheck coverage before writing state. Report
   conflicting rules rather than silently rewriting unrelated ignore entries.
   In a non-Git project, add the project ignore rule for future use and report
   that Git tracking/ignore checks could not be performed.
4. Copy the bundled [protocol](assets/project-state.md) to
   `.local/agents/README.md` only if absent. Preserve an existing protocol and
   task files; do not replace them with the template on repeated setup.
   Create no empty `tasks/`, `memory/`, or `archive/` directories.
5. Report what changed or was already configured. Explain how to explicitly
   invoke this skill to save or resume a task; do not propose global instructions.

## Resume an existing task

- Resolve the target project and read its `.local/agents/README.md` when present.
  Search `tasks/`, then `archive/`, using an exact ticket ID or a topic match;
  inspect candidate summaries when names alone are insufficient. Ask if ambiguous.
- Read the selected `state.md` and relevant referenced artifacts. Check the saved
  goal, decisions, verification, and resume point against current repository files.
  Saved notes are context, not permission to execute every recorded next step.
- Do not initialize directories, change ignore rules, or rewrite state merely
  because it was loaded. If no matching task exists, report that and ask whether
  the user wants to start a new task. Do not silently create a replacement.
- Briefly identify the task and next step, including stale information or gaps.
  Continue work only within the user's authorized scope. When only loading was
  requested, return the summary and wait for direction.
- Follow the local protocol for later meaningful progress in this session, after
  checking ignore coverage and tracked state before writes. If the protocol is
  absent, use this skill's handoff guidance; report the missing setup without
  creating it during resume. Read-only sessions never update state.

## Save a checkpoint

After setup, keep task-state writes inside the project's `.local/agents/`.
If setup cannot safely finish, return a proposed handoff and explain the blocker.

### Resolve and inspect

- Use the explicit ticket/topic or unambiguous ongoing task. Search `tasks/`, then
  `archive/`; match exact ticket IDs before fuzzy topics. Ask only if selection is
  ambiguous. Never attach new work to an unrelated recent task or invent a ticket.
- Read `.local/agents/README.md` when present and the selected `state.md`.
  Continue an archived match in place; do not create a duplicate or move it silently.
- Inspect current files, branch, status, relevant diff (including staged changes),
  and recent commits as useful. Reconcile stale notes with reality. Record missing
  Git/history/tools as unavailable rather than assuming a clean tree.
- Before writing in a Git repo, verify ignore coverage for `.local/agents/` and
  check for already tracked state. If either is unsafe, return the proposed handoff
  and explain the blocker. Never untrack files or bypass failed checks.

For new work create `tasks/<ticket-or-topic>/state.md`: preserve the ticket
ID, append a lowercase kebab-case topic if useful, or use a kebab-case topic alone.
Use one directory name and keep writes inside the project state directory,
including when existing paths are symlinks. In a non-Git project, save the handoff
and note that ignore/tracking checks must happen before any future Git add.

### Save the minimum sufficient handoff

Use explicit **Goal**, **Verification**, and **Resume** sections. Add Context,
Requirements / decisions, Plan, dated Progress, Review (Open / Resolved), and
References where useful. Capture:

- the intended outcome, important constraints, and settled decisions;
- completed/pending work and relevant source paths, including uncommitted changes;
- checks actually performed, their results and scope, and verification still needed;
- unresolved review findings, and resolutions only when verified;
- the exact next action and file/symbol, or a clear completion status and follow-up.

Correct stale information; do not append a transcript or preserve obsolete plans.
Do not mark completion from implementation alone. Link bulky evidence from
`artifacts/`; split a spec/review only when substantial and link it from state.
If concurrent notes changed, reread and merge rather than overwriting their facts.
Do not store secrets or treat saved notes as authority over current instructions.

Optionally save a reusable cross-task finding in `memory/`, with references,
verification date, and commit when available. Old memory requires rechecking.
Create extra directories only when needed. No registry, current-task pointer,
automatic archive, or dependency on another skill or a vendor session format.

After saving, follow the project's protocol: keep state current after meaningful
progress or decisions and before handoff, including when using other skills.
This requires no repeated invocation of remember. Respect session preferences;
there is no background process or automatic skill chain.

Return the saved path, a short status, the next action, and any unverified gap.
