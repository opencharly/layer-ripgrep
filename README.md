# ripgrep

Fast recursive text search for OpenCharly images.

The `ripgrep` candy installs upstream [ripgrep](https://github.com/BurntSushi/ripgrep),
providing the `rg` binary at `/usr/bin/rg` — a recursive search tool that honours
`.gitignore` by default and is dramatically faster than a naïve `grep -r`. The
layer is package-only: no service, no daemon, no runtime dependency, no
configuration. It composes into any image whose distro ships a `ripgrep` package.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `ripgrep` |
| Package | `ripgrep` (distro system package) |
| Binary | `/usr/bin/rg` |
| Service / port | none |
| Environment | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora                 # any distro with a ripgrep package
    candy:
      - '@github.com/opencharly/layer-ripgrep:v2026.235.1653'
```

Then, inside the built image:

```bash
rg "Pattern" path/
rg --version
```

ripgrep returns exit `0` when at least one match is found and exit `1` when the
pattern is absent — usable directly in shell conditionals.

## Layout

- `charly.yml` — the candy manifest: the `ripgrep` package list, an ordered
  `plan:` of build-time `check:` steps, and the embedded `skill:` entity.
- `.github/workflows/deploy.yml` — builds the pinned charly and runs
  `charly box validate` on the manifest (the merge gate).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-tools:ripgrep`
- Bundled by: `/charly-coder:dev-tools`, `/charly-openclaw:openclaw-full`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
