# Remember protocol

The format and rules for `.local/agents/`, which holds local working notes for
agents and is never committed. The skill reads this file on every invocation;
projects keep no copy. A fresh agent must be able to resume from the repository
plus one task's `state.md`. Notes are used only in sessions where the user
invokes the remember skill.

## Layout

```text
tasks/<TICKET>-<topic>/state.md      # entry point and the only status record
tasks/<TICKET>-<topic>/*.md          # supporting docs: spec, contracts, plan
tasks/<TICKET>-<topic>/artifacts/    # evidence and trimmed history
archive/<TICKET>-<topic>/            # finished tasks, moved by the user
```

Use the ticket as written plus a short kebab-case topic, or a topic alone. No index,
current-task pointer or cross-task memory.

## state.md

`state.md` is the task's entry point and the only place for status. Keep it under
about 80 lines, with these sections:

- **Goal:** the outcome and when it counts as done. Open with one plain sentence
  a person would use to describe the task; agents find tasks by it.
- **Context:** what a fresh agent needs before touching anything: key paths,
  environment and access, test accounts, ownership and constraints.
- **Decisions:** settled choices and rejected options worth remembering.
- **Status:** done, verified (with the check that proved it), pending, uncommitted
  work, branch and last commit.
- **Open issues:** known gaps, deferred problems, checks still needed.
- **Next:** the next piece of work, sized to finish and verify in one fresh
  session, starting with a concrete action on a file or symbol. Or "complete"
  plus any follow-up.

When more than one piece remains, list the rest in order in `plan.md`, each one
sized for a fresh session and verifiable on its own. Skip `plan.md` when one
piece covers the task. **Status** names the pieces done.

When the piece in **Next** is done and verified, save, then stop and suggest a fresh
session for the next piece. Don't start it in this session.

Overwrite in place and don't keep a diary. Link commits, repository files and
supporting docs instead of copying them.

`.local/agents/` isn't in Git, so nothing keeps an older `state.md`. Never delete
facts to fit the limit: move detail that no longer belongs in state, such as old
progress entries or long verification output, to a dated file in `artifacts/`
and link it.

## Supporting docs

Specs, contracts, plans and similar task documents go in the task folder next to
`state.md`, linked from it. They hold no status. Create one only when the content
outgrows a section of `state.md`.

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
