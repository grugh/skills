# Conventions

A small set of coding-agent skills. Each works on its own. The skill list and
install steps are in [README.md](../README.md).

- **Explicit.** Skills run only when you invoke them, never because the agent
  decides one fits. Each sets `disable-model-invocation: true`,
  `opencode/autoinvoke: "false"` and, in `agents/openai.yaml`,
  `allow_implicit_invocation: false`. Start the message with the command
  (`/how`, `$how`); a mention mid-sentence does not load the skill.
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

- The first save adds a project ignore rule, installs the protocol if it is
  missing, and writes the state. Existing protocols are never overwritten.
- Ordinary work does not turn memory on. Each session starts with an explicit
  invocation. The first one on an existing task only reads; later ones save.
- `state.md` is the only task record. Repository docs are shared knowledge and
  change only on a specific request. Task status never goes into tracked files.
- "Don't use memory" means no reads or writes. "Only read memory" allows reads.
  Neither deletes files or leaves a marker.
- Nothing runs in the background. The agent follows instructions, so updates
  are not guaranteed.

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
