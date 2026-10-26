# Agent notes

## Upstream tracking

`.upstream.json` maps each skill to the upstream files it adapts. Maintainers and
agents use it to skip upstream fetches that are already covered.

- `upstream[].commit` is the latest upstream commit touching `path` that has been
  reviewed. To check for changes, compare it with the current head commit for
  that path (for example `gh api "repos/<repo>/commits?path=<path>&per_page=1"`).
  If the SHA matches, there is nothing to review.
- `reviewed` is the date of the last review.
- After reviewing upstream changes, update `commit`, `commit_date` and `reviewed`,
  even if no local change was needed.
- Do not update them for local edits, and leave them alone if upstream cannot be
  reached.
- Entries with an empty `upstream` list have no repo to track. Read `note`.

## Writing

Prose in this repo follows `skills/unslop/SKILL.md`. Skill bodies that mirror upstream
text stay close to upstream, so change them only for a reason.

## Shared references

Some skills ship a copy of another file so they install alone. Edit the copies
together. This prints nothing when they match:

```bash
cmp skills/why/references/epistemics.md skills/blast-radius/references/epistemics.md
cmp skills/why/references/sources/code-archaeology.md skills/blast-radius/references/code-archaeology.md
awk 'f>=2; /^---$/{f++}' skills/unslop/SKILL.md | tail -n +2 | cmp - skills/blast-radius/references/writing.md
```
