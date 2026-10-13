# Interrogate

Use independent reviewers, one per configured model when supported, to adversarially review code changes. The adversarial signal comes from model diversity, not assigned personas. The deliverable is a synthesized verdict. Do NOT auto-apply changes.

## Step 1, Scope

Identify what to review from context:

- If the user points at specific files or a diff, use that.
- If on a feature branch, use the resolved comparison from the entrypoint, `git diff <fixed-point>...HEAD`, for the full changeset.
- If the user's message references recent work, gather the relevant files.

Package the diff or file contents with any surrounding context files the reviewers need to understand the code.

## Step 2, Intent

Before spawning reviewers, state the intent in one clear paragraph from the user's message, commit messages, the PR description if one exists, and the code. If you're unsure about the intent, ask the user before proceeding.

## Step 3, Spawn Reviewers

When delegation is available and authorized, launch independent reviewers using the harness's supported tools. Use distinct models when supported and configured; otherwise use fresh contexts with the available model. Every reviewer gets the same prompt and complete rubric, not an assigned lens or persona. Do not assume specific model identifiers or modify model configuration.

If delegation is unavailable, perform one full-rubric review locally and a separate lead-judgment pass. Clearly report that there was no independent or multi-model review; do not fabricate reviewers, agreement counts, or consensus.

Read [reviewer-prompt.md](reviewer-prompt.md) and fill in the same template for every reviewer with the stated intent, the diff or file contents, the rubric from [rubric.md](rubric.md), and the code-quality lens from [code-quality-review.md](code-quality-review.md).

## Step 4, Synthesize

As results come back, build a unified picture. Parse every reviewer's findings. Merge findings that describe the same issue differently, and note which models raised each one. Findings two or more models raised independently are the highest signal. Read a lone model's finding, but weight it accordingly. Note disagreements. If one model flags something and another explicitly says the opposite, that is context for the verdict.

## Step 5, Lead Judgment

You are the lead reviewer, a pragmatic senior engineer, not a neutral aggregator. Read [lead-judgment.md](lead-judgment.md) for the full framework. Put every finding in one of four buckets, with the model(s) that raised it and a one-line rationale.

- **Act on**. Real issues affecting correctness, security, or maintainability given the actual goals. They would block a real PR.
- **Consider**. Legitimate, but you're not sure they outweigh the cost of addressing them now. Worth the user's attention.
- **Noted**. Technically valid but not actionable. Context-dependent, premature optimization, or low-impact at this stage.
- **Dismissed**. Wrong, nitpicky, or missing context, with a brief reason.

## Output Format

### Intent
> [The stated intent paragraph from Step 2]

### Reviewers
- Reviewer [label]: [model name], [N findings] (one bullet per reviewer)

### Act On
[Each: description, which models raised it, why it matters.]

### Consider
[Each: description, which models raised it, the tradeoff.]

### Noted
[Brief list.]

### Dismissed
[Each with a brief rationale.]

### Agreement Map
[Where models agreed and diverged, and what that pattern tells us.]

For a local-only review, identify the single pass and its limits under Reviewers; mark the Agreement Map unavailable instead of simulating independent agreement.
