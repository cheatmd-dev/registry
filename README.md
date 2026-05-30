# CheatMD Registry

The **registry** is the canonical list of installable [CheatMD](https://github.com/cheatmd-dev/cheatmd)
cheat packs. The `cheatmd` CLI fetches [`registry.yaml`](registry.yaml) during
first-run setup so users can install a starter pack of cheats in one step.

The CLI reads the raw manifest at:

```
https://raw.githubusercontent.com/cheatmd-dev/registry/main/registry.yaml
```

Users can point at a private/self-hosted registry by setting `registry_url` in
their `~/.config/cheatmd/cheatmd.yaml`.

## Manifest schema

```yaml
version: 1
packs:
  - name: git            # short id; install subdir under the cheats dir (required)
    repo: https://github.com/cheatmd-dev/cheats-git   # clonable repo URL (required)
    description: Everyday Git commands                 # one-line summary
    official: false                                    # true if maintained by cheatmd
```

## How installation works

For each selected pack, the CLI:

1. Shallow-clones `repo` (`git clone --depth 1`) when `git` is on `PATH`.
2. Falls back to downloading the GitHub tarball over HTTPS when git is missing.
3. Copies only Markdown (`.md`) files into `<cheats-dir>/<name>/`, preserving
   directory structure (and stripping `subdir` when set).

Each cheat pack is itself a normal repository of CheatMD Markdown files.

## Submitting a pack

Open a pull request adding an entry to `registry.yaml`. Keep `description`
short, set `official: false`, and make sure the target repo contains valid
CheatMD Markdown cheats.
