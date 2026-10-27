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
| [bro](skills/bro/SKILL.md) | Make an explanation shorter and easier to understand. |
| [unslop](skills/unslop/SKILL.md) | Remove AI writing patterns from prose. |
| [technical-writing](skills/technical-writing/SKILL.md) | Write or review docs, PR descriptions and commit messages to a technical-writing standard. |
| [translate-bg](skills/translate-bg/SKILL.md) | Translate English technical text into Bulgarian as a developer would write it. |
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
turn memory on. Start a message with `/remember` (Claude Code), `$remember`
(Codex) or `/skill:remember` (Pi), followed by the task as you would describe
it, such as `/remember the dcv validation thing`. A bare `/remember` on the
task's branch also works.

The first call in a session loads the task and writes nothing unless you ask it
to save. Later calls save. If nothing matches and you describe new work, it
creates a task. Once a task is loaded, the agent updates its notes after
commits, finished checks and decisions. Nothing runs in the background.

Notes live in `.local/agents/`, which ignores itself through its own
`.gitignore`. The skill never touches your ignore files or repository docs.
Check that `git ls-files -- .local/agents/` prints nothing, because ignore rules
do not untrack files Git already tracks. The
[protocol](skills/remember/assets/project-state.md) describes the layout.

## Credits

Most of these skills adapt other people's work. Thank you to:

- **[Lauren Tan (@poteto)](https://github.com/poteto)** for
  [pstack](https://github.com/cursor/plugins/tree/main/pstack), the source of
  `how`, `why`, `bro`, `unslop`, `technical-writing`, `blast-radius`,
  `interrogate`, `restate` (from `in-your-own-words`) and the session pickup
  ideas behind `remember`. MIT.
- **[Matt Pocock](https://github.com/mattpocock/skills)** for `grilling`,
  `architecture-review` (from `improve-codebase-architecture`), the `code-review`
  method in `interrogate` and the context persistence ideas in `remember`. MIT.
- **[Julius Brussee](https://github.com/JuliusBrussee/caveman)** for `caveman`. MIT.
- **[Carson Gross](https://grugbrain.dev)** for
  [The Grug Brained Developer](https://grugbrain.dev), which inspired `grug`.
  No text from the essay is copied.

## License

[MIT](LICENSE). The copyright lines match the upstream authors.
