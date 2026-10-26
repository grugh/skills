# Conventions

A small set of coding-agent skills. Each works on its own. The skill list and
install steps are in [README.md](../README.md).

- **Explicit.** Skills run only when you invoke them, never because the agent
  decides one fits. Each sets `disable-model-invocation: true`, which Claude
  Code and OpenCode read, and `allow_implicit_invocation: false` in
  `agents/openai.yaml` for Codex. Start the message with the command
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

The [remember skill](../skills/remember/SKILL.md) keeps task state in the ignored
folder `.local/agents/`. Its [protocol](../skills/remember/assets/project-state.md)
lives only in the skill, so it installs without the rest of this repo and projects
never hold a stale copy. [README.md](../README.md#optional-task-memory) describes
how it behaves.

## Attribution

Most skills adapt other people's work. [README.md](../README.md#credits) credits
the authors. Each skill records its origin in `metadata.source` and ships its own
`LICENSE` so it can be installed alone.

`bro`, `unslop`, `grilling` and `restate` use the upstream instructions with
local metadata. The other skills are adapted or written locally. `grug` takes
ideas from The Grug Brained Developer by Carson Gross and copies none of its
text.

Skills are adapted by hand, not synced automatically. `.upstream.json` records
the last upstream commit reviewed for each skill, and
[AGENTS.md](../AGENTS.md) explains how to check it for new changes.
