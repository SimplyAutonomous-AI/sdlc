# sdlc

<img src="./assets/logo.png" alt="sdlc robot mascot" width="160" align="right">

Agentic quality-assurance and review workflows, written once and pointed at any
project. Each workflow is a set of instructions for an AI agent — Claude Code
CLI or any agent tool that can read files, run commands and start a fresh
sub-agent. There is no program to install.

| Workflow | Use it to |
|---|---|
| [`workflows/unit-tests/unit-tests-review.md`](./workflows/unit-tests/README.md) | Get an isolated, adversarial, read-only verdict on a package's unit tests: a pass/fail checklist of every test file over six gates, as a self-contained HTML report. |
| [`workflows/unit-tests/unit-tests-update.md`](./workflows/unit-tests/README.md) | Bring a package's unit tests up to that bar: a fresh reviewer → fix every finding → commit → a new fresh reviewer, until no medium or high finding remains. |
| [`workflows/code-review/code-review-review.md`](./workflows/code-review/README.md) | Get the same kind of verdict on a package's source code: every source file over six quality gates — static analysis, coverage, design, security, documentation, reliability. |
| [`workflows/code-review/code-review-update.md`](./workflows/code-review/README.md) | Bring the source code up to that bar, in the same review → fix → commit loop, until a fresh review passes every gate. |

Run one by asking the agent, from any directory:

> Follow `<path-to-sdlc>/workflows/unit-tests/unit-tests-update.md`
> with target `/path/to/package`

Each family's README — [unit tests](./workflows/unit-tests/README.md),
[code review](./workflows/code-review/README.md) — has the gates, the inputs and
the output layout. A target's own rules are part of the bar: the workflows read
its AGENTS.md or CONTRIBUTING and its principles in full.
[AGENTS.md](./AGENTS.md) has the rules for changing this repo, and
[PRINCIPLES.md](./PRINCIPLES.md) the principles behind them.

Copyright © 2026 SimplyAutonomous.AI, LLC. Licensed under the PolyForm
Noncommercial License 1.0.0 or the PolyForm Small Business License 1.0.0, at
your choice; their texts in [LICENSE](./LICENSE) govern what each permits.
Both require passing on the licence terms and every `Required Notice:` line;
the second carries one condition under either licence: a redistributor who
offers a warranty or indemnity does so on their own behalf and indemnifies
SimplyAutonomous.AI, LLC for it. A use neither permits, such as
commercial use by a company larger than the Small Business licence's line
(which counts affiliated organizations), requires a separate licence agreement
signed with SimplyAutonomous.AI, LLC (admin@simplyautonomous.ai). All rights
not granted are reserved.
