# charly-local

The `charly-local` family — the `target: local` deploy and `kind: local`
authoring skills.

The `charly-local` candy is a **concept candy**: it ships no install content and
owns the `local` family of `skill:` entities that document applying candies
directly to a Linux filesystem. It currently carries two entities:

- `local-deploy` — `target: local` deployments: the Ansible-style `host:`
  destination field (literal `local` for direct shell, anything else through
  `ssh(1)`), the `local:` substrate kind and its `from:` template reference, the
  `user:`/`ssh_arg:` overrides, the managed `ssh_config` fragment, the install
  ledger, ReverseOp teardown, and the `--with-services` / `--allow-repo-changes`
  / `--allow-root-tasks` gates.
- `local-spec` — authoring `kind: local` templates, `local.yml` files, inline
  `kind: local` nodes, and the merge semantics between a template and the deploy
  that deploys it.

The `charly-cachyos` workstation-profile skill of the same family is owned by
the sibling `opencharly/distro-cachyos` repo. `candy/plugin-marketplace`
regenerates the standalone
[opencharly/marketplace](https://github.com/opencharly/marketplace) corpus from
these entities, so the skills are authored here and projected there.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-local` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Owns | 2 `skill:` entities: `local-deploy`, `local-spec` |
| Projected to | `marketplace/local/skills/` |
| Service / port | none |

## How to use it

This repo is consumed as a **skill source**, not as an image layer. Edit the
`skill:` entities in `charly.yml`; the marketplace regeneration projects them
into `/charly-local:*` pages. To reference the repo directly, compose it in a
box. A box is a `candy:` node that carries the box's `base:` image and a nested
`candy:` list of layer refs (the nested `candy:` is the composition list; the
outer `candy:` is the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora          # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-charly-local:v2026.265.1911'
```

`target: local` deploys do not consume an image at all: they apply a `kind: local`
template's candy stack to a filesystem (this machine or an SSH host).

## Layout

- `charly.yml` — the `charly-local:` concept candy entity plus two `skill:`
  entities (`local-deploy`, `local-spec`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skills: `/charly-local:local-deploy`, `/charly-local:local-spec`
- Authoring reference: `/charly-image:layer`
- Go file map: `/charly-internals:local-infra`
- Workstation profile: `opencharly/distro-cachyos` (`/charly-local:charly-cachyos`)
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
