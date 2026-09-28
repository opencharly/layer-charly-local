# AGENTS.md — layer-charly-local

Standalone candy repo for the `charly-local` concept candy — it ships no install
content and owns the `local` family of `skill:` entities that document
`target: local` deploys and `kind: local` authoring. The entities live in
`charly.yml` at the repo root; `candy/plugin-marketplace` regenerates the
standalone opencharly/marketplace corpus from them.

Canonical files:

- `charly.yml` — the `charly-local:` concept candy entity plus two `skill:`
  entities: `local-deploy-skill` (`name: local-deploy`) and `local-spec-skill`
  (`name: local-spec`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-local:local-deploy` — the owning skill for the `target: local` deploy
  surface (the `host:` field, the `local:` substrate kind, the `user:`/`ssh_arg:`
  overrides, the install ledger, teardown, the install gates). Load before
  editing the `local-deploy-skill:` entity.
- `/charly-local:local-spec` — the `kind: local` template authoring reference
  (`local.yml`, the merge semantics). Load before editing the `local-spec-skill:`
  entity.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate.
- The candy is documentation-only: its `plan:` is a `true` no-op plus a `true`
  `check:` that the skill entities are present and generated into the
  marketplace. There is no live bed.

## Modify this repo

- The `skill:` entities are the projected usage source. Edit them here, never the
  generated `SKILL.md` in the marketplace corpus; a corpus regeneration
  (`charly marketplace generate`) projects them.
- When the local-deploy or `kind: local` schema changes, update the matching
  `skill:` entity in the same change so the corpus does not drift.
- Keep the concept candy's `plan:` no-op; it exists only to carry the entities.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
