# Agent skills toolkit

A set of independent agent skills. Use any of them on its own. None requires another.

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
| [remember](skills/remember/SKILL.md) | Set up task memory, save a handoff, or resume saved work. |
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
`/remember` (Claude Code), `$remember` (Codex) or `/skill:remember` (Pi).

The skill adds `.local/agents/` to your ignore rules, installs
`.local/agents/README.md` if it is missing, and writes the task's `state.md`.
It never overwrites existing files. Add `setup` (for example `/remember setup`)
to do the setup and save nothing.

Once a task is loaded, the agent updates its state after meaningful progress and
before handoff, including while it uses other skills. Nothing runs in the
background. These are instructions to the agent, and `remember` on its own
reconciles the state before you stop.

To resume in a new session, run `/remember resume FEATURE-42` or
`/remember resume invoice export`. Resume reads the saved state and checks it
against the repository. It writes nothing and creates nothing. Say what you want
done next, or ask for a summary only. Existing memory files do not start a
session on their own.

Two phrases override this:

- "Don't use persistent memory this session": no reads or writes, and existing
  files stay untouched.
- "Read the saved state, but don't update memory this session": the agent reads
  existing state but does no setup and writes nothing.

The [protocol template](skills/remember/assets/project-state.md) ships with the
skill. To set up by hand, ignore `/.local/agents/` and copy the template to
`.local/agents/README.md`.

In a Git project, check that `git check-ignore .local/agents/README.md` prints
the path and `git ls-files -- .local/agents/` prints nothing. Ignore rules do not
untrack files Git already tracks.

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
