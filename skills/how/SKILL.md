---
name: how
description: Explain how an existing subsystem, feature, or code path works now by tracing repository evidence. Use for understanding current behavior, not historical intent.
disable-model-invocation: true
license: MIT
metadata:
  opencode/autoinvoke: "false"
  source: "Lauren Tan, pstack: how."
---

# How

Explore the codebase to answer "how does X work?" questions. Produce architectural explanations at the level of a senior engineer onboarding onto a subsystem, enough to build a working mental model, not so much that it reads like annotated source code.

## Step 1. Assess Complexity

If the scope is ambiguous, state your interpretation and explore. The user can redirect.

- **Simple** (a single module, a small utility, a narrow question such as "how does function X work"): no explorers. One explainer explores and explains in a single pass. Go to Step 2b.
- **Complex** (a subsystem spanning multiple files or services, a cross-cutting feature, a full architectural overview): spawn parallel explorers first, then hand off to the explainer. Go to Step 2a.

When in doubt, take the simple path.

## Step 2a. Explore (complex questions only)

Decompose the question into 2 to 4 exploration angles, each a distinct slice of the subsystem. When delegation is available and authorized, run independent explorers concurrently using the harness's supported tools and models. Give each read access without write authority. Otherwise investigate the same angles sequentially and keep their findings distinct before synthesis; disclose that the exploration was not independent.

Each explorer gets the prompt in `references/explorer-prompt.md` with its angle filled in. Then go to Step 3.

## Step 2b. Direct Explain (simple questions)

Explore and explain in one pass in the current context. No subagent is needed for a simple question.

Build its prompt from `references/explainer-prompt.md` without the explorer-findings section. Go to Step 4.

## Step 3. Synthesize (complex questions only)

Once all exploration angles are complete, synthesize their findings into one explanation. Use a separate explainer when delegation is available and authorized; otherwise perform a distinct synthesis pass yourself. Verify contradictions against the code.

Build its prompt from `references/explainer-prompt.md` with every explorer's findings filled in.

## Step 4. Present

Present the explainer's output to the user. Light edits for clarity or context from the conversation are fine. Do not substantially rewrite it.

## Output Format

The explanation uses the sections defined in `references/explainer-prompt.md`, dropping any that do not apply: Overview, Key Concepts, How It Works, Where Things Live, Gotchas.

Do not edit project files or automatically invoke another skill.
