# Adversarial review

Adversaries try to break the change. A finding survives only when a run proves it. Report the findings. Do not apply fixes.

## Step 1, Intent

State the intent in one paragraph, from the user's message, the commits, the PR description if one exists, and the code. Ask the user when the intent is unclear. Adversaries test the change against this intent, not against their own idea of what the code should do.

## Step 2, Fan out

When delegation is available and authorized, launch two or more adversaries in parallel with the harness's supported tools. Start each with a fresh context window. Use distinct models when the harness supports them and they are configured. Do not assume model identifiers or change model configuration.

Each adversary needs a place to run code outside the user's working tree. Use a worktree or scratch copy when the harness provides one, and copy the scope's uncommitted changes into it. Otherwise give each adversary its own scratch directory.

Give each adversary the scope, the intent, [where-to-look.md](where-to-look.md), and this brief, with `{REPRO_DIR}` filled in:

> Break this change. Prove each finding with a run against the real code, such as a failing test, a repro script, or command output that shows the wrong behavior. For a design or plan, run a prototype or probe the current tree. Keep repros in {REPRO_DIR} and leave the user's working tree as you found it. Report each finding with a title, the exact command, the output it printed, and the output the intent requires. Leave out anything you did not run. That covers style, naming, structure, and risks that need a future change to matter. No findings is a valid result.

If delegation is unavailable, follow the brief yourself as a single adversary. Report that the review was not independent.

## Step 3, Verify

Rerun each finding's command yourself. Keep a finding when the output shows the failure and the failure breaks the intent. Drop the rest, including findings that come without a command. Merge findings that share a root cause.

Keep every verified finding. Adversaries find different failures, so do not rank adversaries against each other or weight a finding by how many of them raised it.

If the harness does not allow running commands, say so. List each finding that needs a run under Unverified, with the command that would prove it. Never present an unverified finding as verified.

## Output

- **Intent**: the paragraph from Step 1.
- **Adversaries**: how many ran, on which models, and whether they ran independently.
- **Findings**: for each verified finding, the command, its output, how it breaks the intent, and the adversaries that found it. Say "no verified findings" when none survive.
- **Dropped**: for each adversary, the count of dropped findings and why.
- **Unverified**: only when runs were not possible, as described in Step 3.
