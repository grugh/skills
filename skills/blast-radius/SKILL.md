---
name: blast-radius
description: Trace what outside a proposed or implemented change could break, especially shared contracts, schemas, lifecycle, serialization, concurrency, and reused helpers.
disable-model-invocation: true
license: MIT
metadata:
  opencode/autoinvoke: "false"
  source: "Lauren Tan, pstack: blast-radius."
---

# Blast radius

Find what a change breaks somewhere else, before it ships. Use for "blast radius of X", "what could this break", or reviewing a small diff you don't trust yet.

This investigation covers what a change breaks elsewhere. No other skill is required.

Listing the callers is not the job. The agent can grep those in a second. The job is the breakage grep won't show you.

## Don't trust your own writeup

A blast-radius writeup that sounds right is worthless. It reads as convincing whether or not it's true. So don't hand back the writeup. Find the one or two facts the whole thing depends on and prove them by running code.

### How sure are you

For each fact the change's safety depends on, get it as far down this list as is cheap, and say where it stopped.

1. You said so. Worthless on its own.
2. You pointed at the line. A real `file:line`, or the library's own source.
3. You showed the bad case can't happen. You walked the failure step by step and it doesn't reach.
4. You ran it. A script or test that calls the real code and fails loud if you're wrong.
5. You reproduced it in the running app.

Step 4 is usually one small script that imports the same library the app ships and calls the exact function you're worried about.

## Steps

1. Read the change. The diff, the symbols it adds, changes, and deletes, and what it now does differently, including the part the diff doesn't spell out. Anchor the change in paths, symbols, relevant commits, and PR discussion. Use blame, `git log --follow -p`, and commit messages to find its history; retrieve substantive PR bodies and reviews through an available hosting tool. Follow the bundled [code archaeology procedure](references/code-archaeology.md); report inaccessible history or consumers.
2. Find the one fact it's safe because of. Most changes that look risky are safe because of a single fact, like "this call only drops already-dead cache entries and does nothing else". Find that fact. If it holds, most risky cases are cleared at once. Spend your time here, not on a long list of maybes.
3. Look where grep stops. Read the source of the library you call, and check its pinned version and any local patch. Work out when things run: microtasks, unmount and teardown, Solid versus React. Follow what a symbol search misses: the JSON an API returns, a DB column, a wire format, another language reading the same bytes, a feature flag, code three hops downstream.
4. Be honest about each risk. Give it a real chance of happening and a real cost if it does. Keep the risks you confirmed. List the ones you checked and cleared separately. Use the bundled [evidence framework](references/epistemics.md): distinguish direct evidence, supported conclusions, inference, speculation, and unknowns. Cite a real `file:line`, a search that finds nothing is still an answer, and never make up a caller or an API.
5. Prove the one fact. Write a script or test that runs the real code, run it, and paste what happened.
6. For a big or wide change, when delegation is available and authorized, ask independent reviewers the same question and merge their evidence. Use model diversity when supported. Otherwise perform the investigation locally and disclose the lack of independent review; do not invent reviewer consensus.

## What to hand back

- **What it does.** What changed, including the part that isn't obvious.
- **The one fact it's safe because of.** State it, say which step you got it to, and show the proof. If you couldn't prove it, write unproven.
- **Risks.** Each names how it breaks, the `file:line`, how likely and how bad, and how to check. Paste the proof for the ones that matter.
- **Cleared.** What you checked and why it's fine.
- **Before you merge.** The cheapest test or repro that catches the real bug, including the script you wrote.

Apply the bundled [prose editing instructions](references/writing.md), cite real code, and strip anything private before any separately authorized public output.

**Reply:** the writeup above, with the one safety fact either proven or marked unproven.

Run probes against the actual code and pinned dependencies, using temporary files or isolated test resources when needed. Do not change application source, fixtures, snapshots, or external data to make a probe pass. A no-write request also forbids creating probe files: use an allowed in-memory invocation or mark the fact unproven. For proposed changes, distinguish current-code evidence from predictions. Do not automatically invoke another skill or publish the report.
