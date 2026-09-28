# AGENTS.md — layer-ripgrep

Standalone candy repo for the `ripgrep` layer. The entire candy lives in
`charly.yml` at the repo root: its package list, its ordered `plan:` of
build-time `check:` steps, and the embedded `skill:` entity that is projected
into the marketplace corpus as `/charly-tools:ripgrep`. There is no source tree
and no runtime service — only a package install and its verifiable assertions.

Canonical files:

- `charly.yml` — the `ripgrep:` candy entity and the `ripgrep-skill:` skill entity.
- `.github/workflows/deploy.yml` — the manifest gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:ripgrep` — the owning skill. What the candy installs, how it is
  consumed, and its `.gitignore`-aware `rg` behaviour. Load before editing or
  troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the same structural gate CI runs: the
  manifest must parse and validate at the pinned charly. The CI pin lives in
  `.github/workflows/deploy.yml`; keep the `version:` schema stamp within the
  pinned charly's supported range (do not migrate the stamp past the pin).
- `.github/workflows/deploy.yml` — builds the pinned charly from a CI-time
  checkout and runs `charly box validate`. This is the merge gate.
- There is no live bed: the candy is package-only, so the evidence is its
  `plan:` `check:` steps, which assert `/usr/bin/rg` exists, `rg --version`
  reports a parseable version, and search returns match / no-match correctly.

## Modify this repo

- Edit the `ripgrep:` candy entity AND the `ripgrep-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a
  package or behaviour change that is not mirrored in the skill leaves the
  corpus stale.
- Package changes go under `package:`. Behaviour claims go in `plan:` as
  observable `check:` steps.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
