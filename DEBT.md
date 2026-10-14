# DEBT.md — known debt

The register PRINCIPLES §20 requires, until this repo has `docs/plans/debt.md`.
Each entry says what the debt is and where, why it was accepted, what it costs
while it stays, what paying it off looks like, and its date. An entry stays
**open** until the commit that pays it marks it **paid**.

## D1 — the git history carried private details — paid, 2026-10-14

- **What and where:** every commit in this repo's prior local history carried
  the author's e-mail address. Some of those commits also carried a private
  network address and SSH user for the backup remote, and machine-specific
  home-directory paths, in files and in commit messages. An earlier commit in
  that history held one such address as tracked file content before a later
  commit generalized it; by the time this register was added, the tree no
  longer held any of these, and AGENTS.md §3's scan has run before every
  commit since.
- **Why accepted:** history rolls forward only (PRINCIPLES §1). Removing them
  meant publishing from a fresh root or rewriting history, and both were the
  owner's decision. The repo was not being published yet.
- **Cost while it stayed:** the repo could not be published with its history.
- **Paid by:** the owner chose a fresh root commit of the clean tree, replacing
  the prior local history on `main`, authored and committed as
  `SimplyAutonomous.AI, LLC <admin@simplyautonomous.ai>`. The scan below exits
  1 (clean) against `main` — the branch that gets published — and every
  identity it lists is one the owner approved. It scopes to `main`, not
  `--all`: origin is a private backup remote (§2), never a publication
  target, so a local ref that still mirrors its unrewritten history (or a
  local recovery branch kept during the rewrite) is not itself a leak and is
  not what this check is verifying.

  ```sh
  L=$(mktemp) && git log main -p > "$L" && grep -n -E '192[.]168[.]|ssh:[/][/]|/Users[/]|~[/]' "$L"; echo "history: exit $? (1 = clean)"
  git log main --format='%an <%ae> | %cn <%ce>' | sort -u
  ```

## D2 — the update workflows' helper and arbiter hand-off is not yet run — open, 2026-10-13

- **What and where:** a ground rule and preflight step added to
  [code-review-update.md](./workflows/code-review/code-review-update.md), and
  a ground rule added to
  [unit-tests-update.md](./workflows/unit-tests/unit-tests-update.md), now
  tell the updater to read the target's rules in full and to pass their paths
  to helpers and arbiters. Each workflow was run once on a small scratch
  JavaScript package, with `maxRounds 1`:
  - **code-review-update:** the updater read AGENTS.md, PRINCIPLES.md and all
    15 principle files. Round 1 fixed 12 of 12 findings, citing principles 3,
    9, 11 and 12. The round-2 reviewer's `findings.json` lists every rule file
    under `rulesRead`. Its verdict was FAIL, with 2 medium and 2 low findings
    open.
  - **unit-tests-update:** the updater read the same files. Round 1 fixed 5 of
    5 findings. Its one production change, the removal of a catch-log-rethrow,
    cites principle 12. The round-2 review cites principles 9, 11 and 12. Its
    verdict was FAIL, with 1 medium finding open.

  Neither updater started a helper or an arbiter: each made every fix itself,
  and no finding was disputed. So the instruction to pass the rule paths to
  helpers and arbiters has never run.
- **Why accepted:** in both runs, only the targets' size decided whether a
  helper or arbiter was started, and neither target needed one.
- **Cost while it stays:** a helper or arbiter could judge or fix against the
  one-line index only.
- **Paying it off:** run one update workflow on a target that makes it start a
  helper or an arbiter, such as several independent files, or a finding the
  updater disputes with evidence. Record that each one's hand-off carried the
  rule paths.

## D3 — the update workflows do not say what to do when a reviewer fails mid-run — open, 2026-10-13

- **What and where:** step 2 of
  [code-review-update.md](./workflows/code-review/code-review-update.md) says
  that if the reviewer's `findings.json` is missing, malformed or for another
  round, "say so and stop". The [unit-test updater](./workflows/unit-tests/unit-tests-update.md)
  has the same kind of check. In the D2 code-review run, the round-2 reviewer
  died on a transient network error before writing its findings. The updater
  treated that as transient and resumed the same reviewer once, which the
  workflow does not allow.
- **Why accepted:** it was found while verifying D2, and changing what a
  workflow does needs its own run (AGENTS.md §3).
- **Cost while it stays:** one updater stops on a transient failure while
  another works around it. That is inconsistent behaviour (PRINCIPLES §9).
- **Paying it off:** the update workflows state one rule for a reviewer that
  fails before writing its findings. For example: stop and report, or resume
  the same reviewer once with only "finish into the same runDir" and record
  that in the update report. The rule is then verified by a run.
