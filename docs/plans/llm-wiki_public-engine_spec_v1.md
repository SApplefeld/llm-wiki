# The engine, its template, its two scripts and its three runbooks publish to a public repository on a tag, so this repository can go private

Status: Ready
Commit Model: Branch-and-PR
Created: 2026-10-02

## Dispatch Authorization

The ARCHITECT persona wrote this plan on 2026-10-02 on the operator's ruling of that day on the ARCHITECT's channel: "llm-wiki should also move private. It sets up as an installable process, not a plugin. However, it would be nice to have a cleaned copy that also just has install and setup actions which can be public." He wrote this repository's remote address on the channel the same day. The plan is the fourth repository's instance of the public distribution design the kit repository's `claude-kit_public-marketplace_spec_v1.md` carries, with one difference: the public copy is a repository of its own rather than a folder in the plugins marketplace, since the engine is not a plugin. It has one precondition, on dispatch: the kit's plan has merged, so the two publish scripts exist to copy. The coordinator queues it for whichever worker holds this repository. While this repository is public, no commit, pull request, Chapter or brief on this plan spells any word on the banned list or the path of a file that leaks one.

## Goal

This repository carries a GitHub Actions workflow that runs on a pushed tag matching `publish-*` and on a manual run, assembles the engine from a committed allowlist, the template tree, the two scripts, the structure test, the README, the license and the three documents an installer reads, fails on any forbidden path and on any word from a list held as a repository secret, and pushes one commit replacing the whole tree of the public repository `SApplefeld/llm-wiki-engine` over a deploy key. No plan, backlog, archive, kaizen note or pilot record ships. The README says how a release is cut. A clean machine clones the public repository and instantiates a knowledge base from it by the shipped walkthrough, and the operator then takes this repository private.

## Intent

The frame, in the operator's words of 2026-10-02: "llm-wiki should also move private. It sets up as an installable process, not a plugin. However, it would be nice to have a cleaned copy that also just has install and setup actions which can be public."

What done needs to do. The job, from the kit's design, pushing to the public repository's root rather than a plugin folder: the job empties the clone's tree except `.git` and copies the snapshot in, so a file removed from the allowlist disappears from the public copy at the next publish. The allowlist carries what an installer runs and reads: `new-kb.ps1`, `register-task.ps1`, `template/` whole, `tests/Test-KbStructure.ps1`, `README.md`, `LICENSE`, `.gitignore`, and under `docs/` the instantiation walkthrough, the automation runbook and the architecture reference the two runbooks cite. The README gains a release section.

What done does not need to do. It does not ship `docs/plans/`, `docs/backlog.md`, `docs/archive/` or `docs/README.md`, which is the library index and names the pilot. It does not rewrite the three shipped documents, whose links to one another hold in the snapshot. It does not change how a knowledge base is instantiated.

Alternatives refused. Shipping the two runbooks without the architecture reference: refused, since both cite it by relative link and a public walkthrough with a dead link reads as broken. Making the public copy a folder in `SApplefeld/plugins`: refused on the operator's words, since the engine is not a plugin and a marketplace entry for it would install nothing. A denylist: refused on the 2026-09-28 decision.

Rulings after the spec shipped: none yet.

Provenance: written by the ARCHITECT persona, session 57239bb8, on 2026-10-02, from a fresh clone at f75a25e.

## Approach

**The job and the scripts.** `tools/publish/assemble.mjs` and `tools/publish/leak-gate.mjs` are the kit plan's two scripts, copied byte for byte from the kit repository's `tools/publish/` at the commit its plan landed, with a header line naming that origin. The assembler honors a literal path in the allowlist ahead of its forbidden set, which admits the three documents under `docs/` one by one and nothing else there. `tools/publish/allowlist.txt` is: `README.md`, `LICENSE`, `.gitignore`, `new-kb.ps1`, `register-task.ps1`, `template/**`, `tests/Test-KbStructure.ps1`, `docs/architecture.md`, `docs/instantiation.md`, `docs/runbook_automation.md`. The tree at f75a25e holds 29 tracked files, 25 of them CRLF, and the allowlist resolves 24 of them: everything but `docs/README.md`, `docs/backlog.md`, `docs/archive/README.md`, `docs/plans/README.md` and `docs/plans/llm-wiki_spec_v1.md`. `template/` ships whole, its dotfiles and `.gitkeep` placeholders included, since `new-kb.ps1` copies the entire tree and a missing placeholder drops a directory from every new knowledge base.

**The workflow, `.github/workflows/publish.yml`.** The kit's workflow with these differences: the clone is `git@github.com:SApplefeld/llm-wiki-engine.git`; after the clone the job removes every entry at the clone's root except `.git` and copies the snapshot to the root; the commit title is `Publish llm-wiki-engine from llm-wiki at <tag> (<short sha>).`; there is no seed step, since the README ships. The secrets are `PUBLISH_DEPLOY_KEY`, a key with write access on the public repository, and `PUBLISH_BANNED_WORDS`, both installed by the operator in this repository.

**The README.** `README.md` gains a release section after "How to verify a KB's structure": tag trunk `publish-<YYYYMMDD>` or any `publish-` tag, push it, read the run, what the job refuses, and that the public copy is the engine as an installer needs it and nothing else.

**The tests.** `tools/publish/assemble.test.mjs` and `tools/publish/leak-gate.test.mjs` pin the kit plan's cases under `node --test tools/publish/*.test.mjs`, and a third case pins the literal-path allow admitting `docs/instantiation.md` and refusing `docs/backlog.md`. This repository's own gate stays `tests/Test-KbStructure.ps1`, which is unchanged.

**The sweep.** The gate over the assembled snapshot, run locally with a temporary list the operator hands the worker on its thread. A hit in a shipped document is rewritten to a generic word before the section closes and recorded by path and line.

## Sections of Work

### 1. The publish job, the allowlist and the release section

Model: sonnet

Acceptance:
- `node tools/publish/assemble.mjs --allowlist tools/publish/allowlist.txt --out <tmp>` on this checkout copies 24 files, listed in the Chapter, with `<tmp>/docs/` holding exactly `architecture.md`, `instantiation.md` and `runbook_automation.md`, and `<tmp>/template/` holding every file `git ls-files template` lists.
- `cmp` of the two scripts against the kit's copies reads identical below the origin header line, recorded with the kit commit copied from.
- `node --test tools/publish/*.test.mjs` passes with the three cases, the planted-word case red against a gate stub before the copy.
- `node tools/publish/leak-gate.mjs --root <tmp> --words-file <a temporary list from the operator>` passes over the assembled snapshot, the run in the Chapter with no word.
- `pwsh -NoProfile -File tests/Test-KbStructure.ps1` run against `<tmp>/template` reads the same result as against `template/`, both in the Chapter.
- `README.md` carries the release section.

Files in scope: `tools/publish/assemble.mjs`, `tools/publish/leak-gate.mjs`, `tools/publish/allowlist.txt`, `tools/publish/assemble.test.mjs`, `tools/publish/leak-gate.test.mjs`, `.github/workflows/publish.yml`, `README.md`, and any shipped document the sweep rewrites.
Tests: the literal-path allow admitting three documents and no fourth, since `docs/` holds the plan and the backlog beside them; the template shipping whole with its dotfiles, since the instantiation script copies the entire tree.

## Out of Scope

- The engine's own behavior and the knowledge bases instantiated from it.
- `docs/README.md`, the backlog, the archive and the plans, which never ship.
- The three plugins' publish jobs, in their own repositories' plans.

## Assumptions

- assumed 2026-10-02 (default, pending the operator's word): the public repository is `SApplefeld/llm-wiki-engine`; reversal: another name, which changes one line in the workflow.
- assumed 2026-10-02 (default): `docs/architecture.md` ships with the two runbooks, since both cite it; reversal: drop it from the allowlist and rewrite two links, if the operator reads it as more than setup.
- assumed 2026-10-02 (source: the kit's public marketplace plan): the kit's assembler honors a literal path ahead of its forbidden set; reversal: this repository's copy carrying that one difference, named in its header.
- assumed 2026-10-02 (default): the blind read and the plan review are skipped, since the design is the kit plan's and this spec is one section instancing it.

## Operator Verification

- Before the first publish: create `SApplefeld/llm-wiki-engine`, public, default branch `main`, empty. Add a deploy key with write access there and its private half as `PUBLISH_DEPLOY_KEY` in this repository, with `PUBLISH_BANNED_WORDS` beside it. Confirm Actions is enabled here. Tell the worker any word the sweep must find, on its thread.
- First publish: tag trunk and read the run green and the public tree matching the allowlist.
- Clean-machine check: clone the public repository, run `new-kb.ps1` per the shipped walkthrough, and read the structure test pass on the new knowledge base.
- The flip: after the clean-machine check, take this repository private. The act is irreversible from the public side.

## Open Questions

- The public repository's name, assumed above until the operator writes it.

## Related

- `claude-kit_public-marketplace_spec_v1.md` in the kit repository: the design this plan instances.

## Chapters
