---
name: interrogate
description: Review a concrete diff or implementation for correctness, spec compliance, and codebase fit. Report evidence-backed findings without automatically fixing code.
disable-model-invocation: true
license: MIT
metadata:
  opencode/autoinvoke: "false"
  source: "Lauren Tan, pstack: interrogate; Matt Pocock, skills: code-review."
---

# Interrogate

Review without automatically fixing code or posting externally. Preserve the two source methods as distinct modes:

- **Adversarial** (default): correctness, security, root causes, structural quality, and verification. Follow [the adversarial review](references/adversarial.md), including all four bundled rubric and judgment references.
- **Standards and spec**: documented repository standards and requirement compliance. Follow [the standards/spec review](references/standards-and-spec.md), including its full smell baseline and separate reports.
- **Both**: run both methods over the same scope. Report the adversarial verdict first, then the Standards and Spec reports. Do not merge or rerank findings across these methods or across the Standards and Spec axes. Cross-reference repeated evidence without hiding an axis's finding.

Use the mode the user names. A request specifically about standards/spec compliance selects that mode; a request for both methods or a full review selects Both; anything else, including a general review, uses Adversarial. State the selected mode before reviewing. These are procedures within this skill, not calls to other skills.

## Establish a shared scope

For a branch or committed-change review, resolve the intended fixed point and verify the comparison before reviewing. Ask if it is missing or ambiguous; do not assume a branch name. Use `git diff <fixed-point>...HEAD` and `git log <fixed-point>..HEAD --oneline`. Report an invalid reference or empty scope before launching reviewers.

For an explicit working-tree review, include staged and unstaged changes and relevant untracked files; do not substitute a HEAD-only diff that omits the requested work. Record the exact scope and use it for every selected mode. For an explicit file review, identify the files and surrounding callers needed to assess them. These explicit scopes replace the fixed-point requirement in the standards/spec procedure.

Identify intended behavior from the request, requirements, and relevant history. Read the actual specification where available; code may show behavior but cannot establish requirement compliance by itself. Give reviewers the scope, relevant context, and requirements, not your conclusions.

## Execution and evidence

Use independent contexts and model diversity when available and authorized. Sequential fallback preserves the rubrics and output contracts but is not an independent review; disclose the difference. Every adversarial reviewer receives the whole rubric. Standards and Spec remain separate reviews.

Verify reported execution paths and citations before returning the review. Run focused checks when useful and permitted and record what actually ran and what remains unverified. Do not change source, snapshots, or tests as part of the review. Zero findings is valid; state coverage limits. Never automatically invoke another skill.
