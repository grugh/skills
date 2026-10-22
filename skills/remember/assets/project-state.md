# Remember protocol

The format and rules for `.local/agents/`, which holds local working notes for
agents and is never committed. The skill reads this file on every invocation;
projects keep no copy. A fresh agent must be able to resume from the repository
plus one task's `state.md` and the files it links. Notes are used only in sessions where the user
invokes the remember skill.

## Layout

```text
tasks/<TICKET>-<topic>/state.md       # entry point: status and links
tasks/<TICKET>-<topic>/context.md     # environment, access, paths, accounts, fixtures
tasks/<TICKET>-<topic>/decisions.md   # settled choices and rejected options, with why
tasks/<TICKET>-<topic>/plan.md        # remaining pieces in order
tasks/<TICKET>-<topic>/<topic>.md     # any other topic: spec, contracts, QA, tests, handoffs
tasks/<TICKET>-<topic>/artifacts/     # evidence: logs, outputs, screenshots, probes
archive/<TICKET>-<topic>/             # finished tasks, moved by the user
```

Use the ticket as written plus a short kebab-case topic, or a topic alone. No index,
current-task pointer or cross-task memory.

## state.md

`state.md` is the task's entry point and the only place for status. It holds what
changes from session to session, in these sections:

- **Goal:** the outcome and when it counts as done. Open with one plain sentence
  a person would use to describe the task; agents find tasks by it.
- **Status:** finished pieces, one line each with the commit; verified (with the
  check that proved it); pending and uncommitted work; branch and last commit.
- **Open issues:** known gaps, deferred problems, checks still needed.
- **Next:** the next piece of work, sized to finish and verify in one fresh
  session, starting with a concrete action on a file or symbol. Or "complete"
  plus any follow-up.
- **Files:** one line per topic file, saying what it holds.

Environment details, decisions, plans and other reference material never go in
`state.md`; it links to them.

When more than one piece remains, list the rest in order in `plan.md`, each one
sized for a fresh session and verifiable on its own. Skip `plan.md` when one
piece covers the task. **Status** names the pieces done.

When the piece in **Next** is done and verified, save, then stop and suggest a fresh
session for the next piece. Don't start it in this session.

On each save, rewrite `state.md` to match the present. Remove what is no longer
true or what Git history already records, and move anything that is still
reference into its topic file. Don't keep a diary.

## Topic files

Reference material goes in topic files next to `state.md`: what a fresh agent
needs before touching anything, and what stays true across sessions. Create a
file the first time its topic has content, not when something grows. The names
in the layout are the usual ones, not a fixed list. Keep one topic per file;
when a new kind of content comes up, start a file instead of appending to an
unrelated one.

Topic files hold no status. Link commits and repository files instead of
copying them.

Edit every file in place. Never make dated or backup copies of `state.md` or a
topic file.

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
