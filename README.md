# Agent skills toolkit

A set of independent agent skills. Use any of them on its own. None requires another.
Each runs only when you invoke it by its command; the agent never loads one on its own.

| Skill | Use it to |
| --- | --- |
| [how](skills/how/SKILL.md) | Understand how existing code works now. |
| [why](skills/why/SKILL.md) | Investigate historical rationale without guessing intent. |
| [grilling](skills/grilling/SKILL.md) | Explore a design through question rounds and confirm shared understanding. |
| [restate](skills/restate/SKILL.md) | Have the agent restate your goals and problem to check it understood you. |
| [architecture-review](skills/architecture-review/SKILL.md) | Find modules worth deepening, compare candidates in an HTML report, and explore one. |
| [interrogate](skills/interrogate/SKILL.md) | Review a diff adversarially, against standards and spec, or both. Defaults to adversarial. |
| [blast-radius](skills/blast-radius/SKILL.md) | Find consumers and behavior at risk outside a change. |
| [principles](skills/principles/SKILL.md) | Apply six pstack principles and some general coding guidelines. |
| [bro](skills/bro/SKILL.md) | Make an explanation shorter and easier to understand. |
| [unslop](skills/unslop/SKILL.md) | Remove AI writing patterns from prose. |
| [remember](skills/remember/SKILL.md) | Load a saved task or save its state. |
| [innovate](skills/innovate/SKILL.md) | Propose one valuable new project idea without implementing it. |
| [caveman](skills/caveman/SKILL.md) | Talk in terse caveman speech when asked. |
| [grug](skills/grug/SKILL.md) | Challenge unnecessary complexity. Grug's voice stays in chat. |

## Install

```bash
npx skills add grugh/skills
```

This asks which skills and agents to install. Add `-g` for a global install. See the
[skills CLI documentation](https://github.com/vercel-labs/skills) for other options
and `npx skills update`.

To install manually, copy a skill folder, with everything in it, into your harness's or dotfiled skill directory. Common locations:

| Harness | Directory |
| --- | --- |
| [Codex](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) | `~/.agents/skills/` |
| [Claude Code](https://code.claude.com/docs/en/skills#choose-where-skills-load) | `~/.claude/skills/` |
| [OpenCode](https://opencode.ai/docs/skills/#place-files) | `~/.config/opencode/skills/` |
| [Pi](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/skills.md#locations) | `~/.pi/agent/skills/` |

## Optional task memory

The `remember` skill saves task state between sessions. Installing it does not
turn memory on. Invoke it in each session that should use memory:
`/remember` (Claude Code), `$remember` (Codex) or `/skill:remember` (Pi). Start
the message with the command; mentioning it mid-sentence does not load it.

The first save adds `/.local/agents/` to the project's `.gitignore` (unless a
rule inside the repository already covers it), installs `.local/agents/README.md`
if it is missing, and writes the task's `state.md`. It never overwrites an
existing protocol.

`state.md` is the only task record: goal, decisions, status, open issues and the
next step, kept short and overwritten in place. The skill never reads or writes
your repository docs. Those change only when you ask for a specific edit.

Once a task is loaded, the agent updates `state.md` after commits, finished
checks and decisions, and before handoff. Nothing runs in the background. These
are instructions to the agent, and `remember` on its own reconciles the state
before you stop.

There are no subcommands. Describe the task however you remember it:
`/remember the dcv validation thing`, or just `/remember` on the task's branch.
The agent finds the closest saved task and asks if several fit. The first call
in a session loads the task: it reads the state, compares it with the repository
and writes nothing. Later calls save. If nothing matches and you describe new
work, it creates a task. Existing memory files do not start a session
on their own. Saying "don't use memory" or "only read memory" overrides this for
the session.

The [protocol template](skills/remember/assets/project-state.md) ships with the
skill. To set up by hand, add `/.local/agents/` to `.gitignore` and copy the
template to `.local/agents/README.md`. Check that `git ls-files -- .local/agents/`
prints nothing; ignore rules do not untrack files Git already tracks.

## Credits

Most of these skills adapt other people's work. Thank you to:

- **[Lauren Tan (@poteto)](https://github.com/poteto)** for
  [pstack](https://github.com/cursor/plugins/tree/main/pstack), the source of
  `how`, `why`, `bro`, `unslop`, `blast-radius`, `interrogate`, `principles`
  and the session pickup ideas behind `remember`, and for the idea behind
  `restate`. MIT.
- **[Matt Pocock](https://github.com/mattpocock/skills)** for `grilling`,
  `architecture-review` (from `improve-codebase-architecture`), the `code-review`
  method in `interrogate` and the context persistence ideas in `remember`. MIT.
- **[Julius Brussee](https://github.com/JuliusBrussee/caveman)** for `caveman`. MIT.
- **[Carson Gross](https://grugbrain.dev)** for
  [The Grug Brained Developer](https://grugbrain.dev), which inspired `grug`.
  No text from the essay is copied.

## License

[MIT](LICENSE). The copyright lines match the upstream authors.
