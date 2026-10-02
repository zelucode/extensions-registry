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

## Two ways to host a package

An entry is only a pointer: `downloadUrl` (and `sigUrl`) can be **any public http(s) URL**, so both of these work side by side in the same `index.json` and need no registry change:

1. **The author's own repo/release** (third-party authors, and the original worked example `acme-pdf-tools`):
   `https://github.com/<owner>/<repo>/releases/download/v1.2.0/pdf-tools.aosext`
2. **A shared "packaged" repo** that holds only built, signed packages, never source (the DeskStride first-party setup).
   The source lives in a private repo; `tools/publish.py` there builds and signs on a trusted machine and writes
   `<id>/<id>-<version>.dsext` + `.sig` into the public packaged repo, plus the entry here:
   `https://raw.githubusercontent.com/zelucode/deskstride-extensions-packaged/main/<id>/<id>-<version>.dsext`

Either way the install is verified the same way: the app checks the `sha256` pinned in the entry and, when present, the
`sigUrl` signature. Entries written by the publish tooling also carry `permissions`, mirrored from the manifest.
