# AGENTS.md — `sdlc`

Canonical rules for this repo. `CLAUDE.md` is a pointer to this file.

**Before any task, read [PRINCIPLES.md](./PRINCIPLES.md)** — a one-line index;
open a principle's file in [`principles/`](./principles/) when a task reaches
it. These apply to every task here:

- **Verify every assumption** before acting on it or reporting it as fact: run
  the command, read the file or the primary source
  ([§5](principles/05-evidence.md), [§12](principles/12-external-facts-decay.md)).
- **Run the thing you claim:** a workflow change is verified by running the
  workflow (§3 below), and a command by running that command
  ([§14](principles/14-run-the-thing.md)).
- **Fix the root cause:** never skip, suppress or route around a failing check
  ([§8](principles/08-fix-it.md)).
- **Decide and keep going;** ask only when the answer changes what gets built,
  one decision at a time ([§4](principles/04-bias-for-action.md)).
- **Done means reviewed:** every change here gets the one-round agentic review
  of [§18](principles/18-adversarial-review.md) (§3 below).

## 1 · What this repo is

`sdlc` holds **agentic quality-assurance and review workflows** that any project
can be put through. A workflow here is a Markdown document of instructions for
an AI agent — what to examine, in what order, what counts as pass or fail, what
to write, and whom to hand off to. The agent does the work; there is no program.

| Workflow | What it does |
|---|---|
| [`workflows/unit-tests/`](./workflows/unit-tests/README.md) | `unit-tests-review.md` — an isolated adversarial checklist of every unit-test file against six gates, written up as an HTML report and a `findings.json`. `unit-tests-update.md` — the updater: hands the code to a fresh reviewer, fixes every finding, commits, reports, and hands off to a new reviewer, until no medium or high finding remains. |
| [`workflows/code-review/`](./workflows/code-review/README.md) | `code-review-review.md` — an isolated adversarial checklist of every source file against six quality gates (static analysis, coverage, design, security, documentation, reliability), as an HTML report and a `findings.json`. `code-review-update.md` — the updater: the same loop for source code, until a fresh review passes every gate. |

## 2 · How a workflow is written

- **One folder per workflow family** under `workflows/`, with a README that says
  how to run it.
- **A workflow document is self-contained.** Everything an agent needs to do the
  job is in it — inputs, ground rules, the criteria, the procedure, the exact
  shape of what it writes. It may use files beside it in its own folder (the
  shared report head) and may tell the agent to start a sibling workflow in a
  fresh agent; nothing else.
- **Plain instructions, not code.** If a step seems to need a program, it is
  usually a command the agent runs (the project's own test runner, `git`,
  `open`). Keep it that way: a program placed between agents that hand off to
  each other is one more layer that can silently drop or mangle what passes
  through it, such as a large report that has to land on disk intact.
- **Any language, any framework.** Name tools as examples, never as a whitelist,
  and tell the agent to say so when a capability does not exist.
- **Isolation is a hand-off rule.** A workflow that must judge independently is
  started as a new agent with only its inputs — never the starter's findings or
  reasoning.
- **The target's own rules are read in full.** A workflow that judges or changes
  code tells the agent to read the target's AGENTS.md or CONTRIBUTING and its
  principles — the index *and every file it links* — and to pass those paths to
  every sub-agent it starts. A session may begin with a one-line index only, as
  this repo's does, and a finding needs the whole rule. The bar is the target's
  rules, never this repo's; a target with none is judged by the gates alone, and
  the report says so.
- **Reports** are one self-contained HTML file each (inline CSS and SVG, no
  scripts, no external requests), built from the folder's `report-head.html`,
  plus a JSON file with a fixed shape for the next agent to read.

## 3 · Verifying a change

There is no build and no unit-test suite: the product is instructions. A change
to a workflow is verified by **running it for real** on a small target and
reading what it produced — the report, the JSON, the commits — against what the
document says should happen (PRINCIPLES §14). Record the run you used in the
commit message.

Every change — to a workflow, the guidance or the docs — also gets the one-round
review of PRINCIPLES §18: a fresh agent given only its inputs — the diff, §18's
charter, and the paths of AGENTS.md, PRINCIPLES.md and every `principles/` file,
to read in full — never the author's reasoning. Its `Review:` trailer goes in
the completing commit.

Before every commit, check that the tree, untracked files included, carries
nothing private or machine-specific — a private network address, an SSH URL, a
home-directory path. It prints nothing and exits 1 when clean:

```sh
git grep --untracked -n -I -E '192[.]168[.]|ssh:[/][/]|/Users[/]|~[/]' -- . ; echo "exit $? (1 = clean)"
```

## 4 · Scope of the principles

[PRINCIPLES.md](./PRINCIPLES.md) is the index — one line per principle,
imported into every session, so it stays short. [principles/](./principles/)
holds each principle's text, opened when a task reaches it.

**This repo stands alone:** nothing here syncs with, or points to, a path
outside it, and a principle changes here by a commit here. A principle's file
governs over its line in the index, and a change to a principle's text updates
that line in the same change. §N always
means this repo's numbering, even where a parent directory loads another
`PRINCIPLES.md` that numbers differently. Check that the index links every
principle file and nothing else:

```sh
diff <(ls principles/*.md | sort) <(grep -o '](principles/[^)]*)' PRINCIPLES.md | sed 's/^](//; s/)$//' | sort) && echo "index complete"
```

All principles apply. Scope notes:

- **§1** — no deploy path. Publishing this repo is outward-facing and needs the
  owner's consent for that action. Debt D1 in [DEBT.md](./DEBT.md), which
  tracked private details in the git history, is paid.
- **§2** — origin is the NAS (`git remote get-url origin`).
- **§4** — there is no decision queue here yet: ask in the conversation, one
  self-contained decision at a time.
- **§17, §19, §20** — there is no `docs/` or plans folder yet. Until there is,
  the docs are README.md and each workflow family's README, kept true under
  §15, and the §20 debt register is [DEBT.md](./DEBT.md).
- **§22** — there is no CDK infrastructure here.
- **The workflows act on other repositories.** `unit-tests-update` and
  `code-review-update` edit and commit in the target repo (never push) and stop
  on a dirty tree while committing is on. Treat a change to that behaviour as
  outward-facing.
