# Automation OS Extensions Registry

A GitHub/Obsidian-style index of third-party [Automation OS](https://github.com/) `.aosext` extensions -- this repo holds pointers (one JSON file per extension) to code hosted elsewhere, never the code itself.

Self-hosted (not on GitHub) at this project owner's own Gitea instance for now. Point Automation OS's Settings -> Extensions -> Registry URL at this repo's `index.json` raw URL to browse/install from it.

- Full design: `docs/roadmap/RND_EXTENSION_REGISTRY.md` in the main `automation-os` repo.
- To submit or update an entry: see [CONTRIBUTING.md](CONTRIBUTING.md).
- `index.json` is a generated build artifact (`aos_ext_cli.py build-index .`) -- never hand-edit it, edit files under `extensions/` instead.

## Layout

```
schema/extension-entry.schema.json   # what a valid extensions/<id>.json must look like
extensions/                          # one <id>.json per listed extension
index.json                           # generated aggregate -- do not hand-edit
```
