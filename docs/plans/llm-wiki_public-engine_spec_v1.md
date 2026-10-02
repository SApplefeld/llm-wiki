# The engine, its template, its two scripts and its three runbooks publish to the public marketplace as the wiki entry on a tag, so this repository can go private

Status: Ready
Commit Model: Branch-and-PR
Created: 2026-10-02

## Dispatch Authorization

The ARCHITECT persona wrote this plan on 2026-10-02 on the operator's ruling of that day on the ARCHITECT's channel: "llm-wiki should also move private. It sets up as an installable process, not a plugin. However, it would be nice to have a cleaned copy that also just has install and setup actions which can be public." He wrote this repository's remote address on the channel the same day. On the same channel later that day he ruled the public copy's shape: "Just call the public copy wiki@applefeld" and "Just like you did for personas@applefeld". The plan is the fourth repository's instance of the public distribution design the kit repository's `claude-kit_public-marketplace_spec_v1.md` carries: the public copy is the folder `plugins/wiki/` in `SApplefeld/plugins`, a plugin by manifest alone, so an installer reaches the engine with one install command as with every other piece of the fleet. It has one precondition, on dispatch: the kit's plan has merged, so the two publish scripts exist to copy. The coordinator queues it for whichever worker holds this repository. While this repository is public, no commit, pull request, Chapter or brief on this plan spells any word on the banned list or the path of a file that leaks one.

## Goal

This repository carries a GitHub Actions workflow that runs on a pushed tag matching `publish-*` and on a manual run, assembles the engine from a committed allowlist, the template tree, the two scripts, the structure test, the README, the license and the three documents an installer reads, fails on any forbidden path and on any word from a list held as a repository secret, and pushes one commit replacing `plugins/wiki/` in `SApplefeld/plugins` over a deploy key. The folder carries a plugin manifest naming `wiki`, so the public catalog's `wiki` entry installs it as `wiki@applefeld`. No plan, backlog, archive, kaizen note or pilot record ships. The README says how a release is cut and how an installer reaches the engine. A clean machine installs `wiki@applefeld` and instantiates a knowledge base from the installed folder by the shipped walkthrough, and the operator then takes this repository private.

## Intent

The frame, in the operator's words of 2026-10-02: "llm-wiki should also move private. It sets up as an installable process, not a plugin. However, it would be nice to have a cleaned copy that also just has install and setup actions which can be public."

What done needs to do. The job, from the kit's design, replacing the `plugins/wiki/` folder as each sibling replaces its own. A plugin manifest, so the catalog entry installs. The allowlist carries the manifest and what an installer runs and reads: `new-kb.ps1`, `register-task.ps1`, `template/` whole, `tests/Test-KbStructure.ps1`, `README.md`, `LICENSE`, `.gitignore`, and under `docs/` the instantiation walkthrough, the automation runbook and the architecture reference the two runbooks cite. The README gains a release section and an install paragraph naming the installed folder the setup script runs from.

What done does not need to do. It does not ship `docs/plans/`, `docs/backlog.md`, `docs/archive/` or `docs/README.md`, which is the library index and names the pilot. It does not rewrite the three shipped documents, whose links to one another hold in the snapshot. It does not change how a knowledge base is instantiated, and it adds no skill, hook or command to the plugin: the manifest only places the files.

Alternatives refused. Shipping the two runbooks without the architecture reference: refused, since both cite it by relative link and a public walkthrough with a dead link reads as broken. A public repository of its own: refused on the operator's ruling, since one marketplace serves the whole fleet and a second remote is a second thing to find. A skill wrapping the setup script: refused as not asked for, and a later plan's if wanted. A denylist: refused on the 2026-09-28 decision.

Rulings after the spec shipped: 2026-10-02, on the ARCHITECT's channel, "Just call the public copy wiki@applefeld" and "Just like you did for personas@applefeld", which moved the public copy from its own repository to the marketplace's fourth entry.

Provenance: written by the ARCHITECT persona, session 57239bb8, on 2026-10-02, from a fresh clone at f75a25e.

## Approach

**The job and the scripts.** `tools/publish/assemble.mjs` and `tools/publish/leak-gate.mjs` are the kit plan's two scripts, copied byte for byte from the kit repository's `tools/publish/` at the commit its plan landed, with a header line naming that origin. The assembler honors a literal path in the allowlist ahead of its forbidden set, which admits the three documents under `docs/` one by one and nothing else there. `tools/publish/allowlist.txt` is: `.claude-plugin/plugin.json`, `README.md`, `LICENSE`, `.gitignore`, `new-kb.ps1`, `register-task.ps1`, `template/**`, `tests/Test-KbStructure.ps1`, `docs/architecture.md`, `docs/instantiation.md`, `docs/runbook_automation.md`. The tree at f75a25e holds 29 tracked files, 25 of them CRLF, and the allowlist resolves 24 of them plus the new manifest, 25 in all: everything but `docs/README.md`, `docs/backlog.md`, `docs/archive/README.md`, `docs/plans/README.md` and `docs/plans/llm-wiki_spec_v1.md`. `template/` ships whole, its dotfiles and `.gitkeep` placeholders included, since `new-kb.ps1` copies the entire tree and a missing placeholder drops a directory from every new knowledge base.

**The manifest, `.claude-plugin/plugin.json`.** New, at this repository's root, as the persona plugin's is at its root: `name` is `wiki`, with a one-line `description` and no `version`, so installs follow commits, and no hooks, skills or commands. `claude plugin validate .` passes on this checkout. It is a plugin by manifest alone: installing `wiki@applefeld` places the engine under the plugin cache at `<plugin cache>/applefeld/wiki/<sha>/`, and the setup script runs from there.

**The workflow, `.github/workflows/publish.yml`.** The kit's workflow with `wiki` for the plugin, `plugins/wiki` for the folder, `llm-wiki` for the repository name in the commit title, and no seed step. The secrets are `PUBLISH_DEPLOY_KEY`, the deploy key with write access on `SApplefeld/plugins`, and `PUBLISH_BANNED_WORDS`, both installed by the operator in this repository.

**The README.** `README.md` gains an install paragraph near its top: add the public marketplace, `claude plugin marketplace add SApplefeld/plugins`, install `wiki@applefeld`, and run `new-kb.ps1` from the installed folder, with the clone path still described for a developer. It gains a release section after "How to verify a KB's structure": tag trunk `publish-<YYYYMMDD>` or any `publish-` tag, push it, read the run, what the job refuses, and that the public folder is the engine as an installer needs it and nothing else.

**The tests.** `tools/publish/assemble.test.mjs` and `tools/publish/leak-gate.test.mjs` pin the kit plan's cases under `node --test tools/publish/*.test.mjs`, and a third case pins the literal-path allow admitting `docs/instantiation.md` and refusing `docs/backlog.md`. This repository's own gate stays `tests/Test-KbStructure.ps1`, which is unchanged.

**The sweep.** The gate over the assembled snapshot, run locally with a temporary list the operator hands the worker on its thread. A hit in a shipped document is rewritten to a generic word before the section closes and recorded by path and line.

## Sections of Work

### 1. The publish job, the allowlist and the release section

Model: sonnet

Acceptance:
- `node tools/publish/assemble.mjs --allowlist tools/publish/allowlist.txt --out <tmp>` on this checkout copies 25 files, listed in the Chapter, with `<tmp>/.claude-plugin/plugin.json` present, `<tmp>/docs/` holding exactly `architecture.md`, `instantiation.md` and `runbook_automation.md`, and `<tmp>/template/` holding every file `git ls-files template` lists.
- `cmp` of the two scripts against the kit's copies reads identical below the origin header line, recorded with the kit commit copied from.
- `node --test tools/publish/*.test.mjs` passes with the three cases, the planted-word case red against a gate stub before the copy.
- `node tools/publish/leak-gate.mjs --root <tmp> --words-file <a temporary list from the operator>` passes over the assembled snapshot, the run in the Chapter with no word.
- `pwsh -NoProfile -File tests/Test-KbStructure.ps1` run against `<tmp>/template` reads the same result as against `template/`, both in the Chapter.
- `claude plugin validate .` passes on this checkout and on `<tmp>`.
- `README.md` carries the install paragraph and the release section.

Files in scope: `.claude-plugin/plugin.json`, `tools/publish/assemble.mjs`, `tools/publish/leak-gate.mjs`, `tools/publish/allowlist.txt`, `tools/publish/assemble.test.mjs`, `tools/publish/leak-gate.test.mjs`, `.github/workflows/publish.yml`, `README.md`, and any shipped document the sweep rewrites.
Tests: the literal-path allow admitting three documents and no fourth, since `docs/` holds the plan and the backlog beside them; the template shipping whole with its dotfiles, since the instantiation script copies the entire tree.

## Out of Scope

- The engine's own behavior and the knowledge bases instantiated from it.
- `docs/README.md`, the backlog, the archive and the plans, which never ship.
- The three plugins' publish jobs, in their own repositories' plans.

## Assumptions

- assumed 2026-10-02 (default): the manifest carries a name and a description only, since a plugin with no hooks, skills or commands still installs and places its files; reversal: a validate refusal on this build, which the worker records and answers by adding whatever field the validator names.
- assumed 2026-10-02 (default): `docs/architecture.md` ships with the two runbooks, since both cite it; reversal: drop it from the allowlist and rewrite two links, if the operator reads it as more than setup.
- assumed 2026-10-02 (source: the kit's public marketplace plan): the kit's assembler honors a literal path ahead of its forbidden set; reversal: this repository's copy carrying that one difference, named in its header.
- assumed 2026-10-02 (default): the blind read and the plan review are skipped, since the design is the kit plan's and this spec is one section instancing it.

## Operator Verification

- Before the first publish: add the fleet's deploy key as `PUBLISH_DEPLOY_KEY` in this repository, with `PUBLISH_BANNED_WORDS` beside it, per the kit plan's steps. Confirm Actions is enabled here. Tell the worker any word the sweep must find, on its thread.
- First publish: after the kit, persona and relay tags, tag trunk here and read the run green and `plugins/wiki/` in the public repository matching the allowlist.
- Clean-machine check: `claude plugin install wiki@applefeld`, run `new-kb.ps1` from the installed folder per the shipped walkthrough, and read the structure test pass on the new knowledge base.
- The flip: after the clean-machine check, take this repository private. The act is irreversible from the public side.

## Open Questions

- None.

## Related

- `claude-kit_public-marketplace_spec_v1.md` in the kit repository: the design this plan instances.

## Chapters
