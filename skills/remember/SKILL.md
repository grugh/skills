---
name: remember
description: Remember a task across sessions in project-local state. Explicitly invoke to load a task or save its state; the first save sets up the project.
disable-model-invocation: true
license: MIT
metadata:
  opencode/autoinvoke: "false"
  source: "Lauren Tan, pstack: session pickup and recall; Matt Pocock, skills: context persistence."
---

# Remember

There are no subcommands. Find the task, then pick the action from the session:

- **Load:** the task isn't loaded in this session yet and a match exists. Read it
  and report. Write nothing, even if the request mentions saving; say that the
  next invocation saves.
- **Save:** the task was already loaded or created in this session. Write
  `state.md`.
- **New:** nothing matches and the user describes work worth tracking. Set up if
  needed and create the task. If nothing matches and the request is vague, ask.

Free text after the command steers the result ("just summarize", "is it stale?").
If the user says not to use memory, or to only read it, obey for the session.

Read [the protocol](assets/project-state.md) on every invocation. It lives only in
this skill, so a project never holds a stale copy.

`state.md` is the only status record. Remember never reads or writes the
repository's docs; they change only when the user asks for a specific edit.
Progress, pending work and next steps never go into tracked files.

## Setup (every write)

1. Use the Git worktree root. Keep all writes inside `.local/agents/`; don't follow
   symlinks out of it.
2. If `git ls-files -- .local/agents/` lists anything, report it and stop. Never
   untrack files.
3. Run `git check-ignore -v .local/agents/README.md`. If it fails,
   append `/.local/agents/` to `.gitignore`.
4. If `.local/agents/README.md` is missing, create it with the text below. Never
   change an existing one; the skill doesn't read it.

   ```markdown
   # Agent task state

   Local task notes written by the remember skill. Never committed.
   The format lives in the skill: invoke remember to load or save a task.
   ```

## Find the task

Match loosely, the way the user would describe it. List `tasks/` and `archive/`,
and skim each candidate's **Goal**. Weigh the request's words, the current branch,
recent commits and this session's work. A bare invocation with one active task,
or one matching the branch, means that task.

Use a clear best match. If two or more are plausible, list them with their goals
and ask. Continue an archived task in place.

## Load

1. Read `state.md`. Compare its saved commit with `HEAD`, and check `git status`
   and the branch.
2. Report the goal, status, open issues and next step, and call out anything stale.
   If the task is in an older format, say that the next save converts it.
   Saved notes are context, not permission to run the next step.

## Save

1. Check the branch, `git status` and the relevant diff, and correct stale notes.
2. Write `state.md` in the protocol's current format, overwriting in place. Convert
   an older task the same way: keep its old files, link them from `state.md`, and
   move detail that doesn't fit to `artifacts/` rather than dropping it. A new task
   goes in `tasks/<name>/state.md`: the ticket as written, if there is one, plus a
   short kebab-case topic. Never invent a ticket.

After loading or saving, keep `state.md` current at milestones for the rest of
the session, as the protocol describes. If the user says "the docs" and could
mean either `state.md` or the repository's docs, ask once. Never commit, archive
or invoke another skill automatically.

Return the path, a one-line status, the next action and any unverified gap.
