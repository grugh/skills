# Conventions

A small set of coding-agent skills. Each works on its own. The skill list and
install steps are in [README.md](../README.md).

- **Independent.** A skill may recommend another skill but never invokes it.
  There is no required order and no shared runtime.
- **Portable.** No skill requires a vendor API, session format or model name.
  `how`, `why` and `interrogate` keep their upstream methods and fall back to
  sequential work when the harness cannot delegate. `grilling` is upstream text
  and expects subagents for fact-finding. Your harness and its permissions decide
  whether they run.
- **Focused.** Analysis and review skills report findings. They change code only
  when you ask. Match the depth of the work to the problem.
- **Evidence-based.** Separate what the code shows, what follows from it and what
  is unknown. Prefer an existing solution and simple ownership over a new layer.
- **Voices stay in chat.** `caveman` and `grug` change how the agent talks. They
  never touch code, comments or saved files.

## Task memory

Task memory lives in the ignored folder `.local/agents/`. The
[remember skill](../skills/remember/SKILL.md) saves task handoffs and does the
first-use setup. It bundles its own
[protocol template](../skills/remember/assets/project-state.md), so it installs
without the rest of this repo.

- Invoking `remember` to save a task installs the protocol if it is missing,
  adds the ignore rule, and writes the state. `setup` does only the first two.
- Existing protocols and task files are never overwritten.
- Ordinary work in a project without memory does not turn memory on.
- Every session starts with an explicit save or resume. Resume reads existing
  state and does no setup or checkpoint. A loaded task's protocol then guides
  updates for that session.
- "Don't use persistent memory" means no reads or writes. "Read but don't
  update" allows reads only. Neither deletes files or leaves a marker.
- Nothing runs in the background. The agent follows instructions, so updates
  are not guaranteed.

A task starts with one `state.md`, enough for a fresh agent to resume from the
repo files. There is no task registry, no required template sections and no
automatic archiving.

## Attribution

Most skills adapt other people's work. [README.md](../README.md#credits) credits
the authors. Each skill records its origin in `metadata.source` and ships its own
`LICENSE` so it can be installed alone.

`bro`, `unslop` and `grilling` use the upstream instructions with local
metadata. The other skills are adapted or written locally. `restate` follows
an idea from Lauren Tan. `grug` takes ideas from The Grug Brained Developer by
Carson Gross and copies none of its text.

Skills are adapted by hand, not synced automatically. `.upstream.json` records
the last upstream commit reviewed for each skill, and
[AGENTS.md](../AGENTS.md) explains how to check it for new changes.
