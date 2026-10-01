# Project-local agent state

Local working notes for agents. Never committed. A fresh agent must be able to resume
from the repository plus one task's `state.md`. Notes are used only in sessions
where the user invokes the remember skill.

## Layout

```text
tasks/<TICKET>-<topic>/state.md      # one file per active task
tasks/<TICKET>-<topic>/artifacts/    # only bulky evidence: logs, outputs, snapshots
archive/<TICKET>-<topic>/            # finished tasks, moved by the user
```

Use the ticket as written plus a short kebab-case topic, or a topic alone. No index,
current-task pointer or cross-task memory.

## state.md

`state.md` is the only task record. Keep it under about 80 lines, with these sections:

- **Goal:** the outcome and when it counts as done. Open with one plain sentence
  a person would use to describe the task; agents find tasks by it.
- **Decisions:** settled choices and rejected options worth remembering.
- **Status:** done, verified (with the check that proved it), pending, uncommitted
  work, branch and last commit.
- **Open issues:** known gaps, deferred problems, checks still needed.
- **Next:** one concrete action with the file or symbol, or "complete" plus any
  follow-up.

Overwrite in place. Don't keep a diary; Git history is the record. Link commits,
repository files and artifacts instead of copying them.

## Repository docs

The repository's docs are shared knowledge, changed only when the user asks for a
specific edit. Don't read them to sync state, don't copy state into them, and never
put progress, pending work or next steps in any tracked file.

## When to update

After a commit, a finished check, a decision, and before handoff. Compare the
notes with `git status` and the branch before writing.

## Rules

- No secrets or sensitive payloads.
- Notes never override the user's current instructions.
- If another writer changed `state.md`, merge their facts instead of replacing them.

## Archive

The user moves finished tasks to `archive/`. Before that, set **Next** to "complete".
