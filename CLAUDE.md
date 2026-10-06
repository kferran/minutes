# minutes

A Claude Code plugin with two skills: `meeting-notes` (`skills/meeting-notes/scripts/minutes.py`) and `confined-fetch` (`skills/confined-fetch/scripts/confined_fetch.sh`). Design: `docs/superpowers/specs/`.

## How work is done here

- Superpowers flow: brainstorm → spec (independent Opus review; re-review after a large revision) → plan drafted and proven in a scratch clone (exact patches, red then green, a fresh-clone proof) → execute (native by default) → one whole-branch Opus review → one test-first fix pass → live acceptance → outcomes doc → PR. The owner merges; ask before opening a PR.
- Process docs: `docs/superpowers/specs/`, `docs/superpowers/plans/` (plans, outcomes), `docs/superpowers/spikes/` (probes, acceptance records).
- Work on a feature branch, never on `main`.
- Every decision made on the owner's behalf is a ruling with its cost if wrong; deferred minors become GitHub issues when the owner asks.

## Rules

- **Prerequisites stay low:** `confined-fetch` uses bash 3.2+, jq 1.6+ and `claude` only (no `mapfile`, `declare -A`, `${x,,}`, GNU `timeout`, `date -d`, `sed -i`); `meeting-notes` uses Python 3.9+ standard library only.
- **Tests are hermetic:** synthetic fixtures only (this repo is public: no real names, emails, file IDs or meeting text), a stub `claude`, no network. Run everything with `tests/run.sh`; read the verdict from its exit code, never through a pipe.
- **bats:** no mid-test `!`, no `&&` assertion chains, no wall-clock timing assertions; no `run -N` (bats 1.8).
- **Scratch:** `.scratch/` replaces `/tmp` for clones, worktrees and temp files (it ignores itself). The one exception is `confined_fetch.sh`'s session directory, which must stay under `/tmp`, outside any project.
- **Commands:** one tool per Bash call, literal paths, Edit/Write for file changes, `git commit -F <file>`.
- **Live runs** of the real `claude` or a connector happen only in a plan's acceptance task, with the owner's go-ahead.
- Never commit a hostname, user path, remote secret or personal data.
