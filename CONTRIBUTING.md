# Contributing an extension entry

This repo is an **index**, not a code host. Adding your extension here
does not upload your code anywhere -- it lives in your own repo/release;
this registry only points at it.

## Submission flow (single-maintainer, light review -- Obsidian's model)

1. Publish your `.aosext` as a downloadable file (e.g. a Gitea/GitHub
   Release asset on your own repo). If it's signed (recommended --
   `extension-cli`'s `keygen`/`sign` commands), publish the detached
   `.sig` file alongside it.
2. Compute its SHA-256: `aos_ext_cli.py sha256 your-extension.aosext`.
3. Add one file at `extensions/<your-id>.json`, matching
   `schema/extension-entry.schema.json`. `id` must match the `id` in your
   extension's own `manifest.json`.
4. Open a PR (or, for now, hand this to the maintainer directly).
5. The maintainer runs `aos_ext_cli.py lint`/`scan` against your package
   and a lightweight manual read for anything obviously malicious or
   undisclosed (network calls, telemetry) -- not a full audit. First-claim
   on `id`: a new id is accepted on a first-come basis; updating an
   existing id should come from whoever's `authorProfile` is already on
   file (or the maintainer).
6. On merge, the maintainer runs `aos_ext_cli.py build-index .` to
   regenerate `index.json` and pushes that too -- `index.json` is a build
   artifact, never hand-edited.

## Updating a listed extension

Same flow: edit your `extensions/<id>.json` (bump `latestVersion`,
`downloadUrl`, `sha256`, `sigUrl`), rebuild the index, push. There is no
separate "publish" step beyond that -- the app reads `index.json` directly.

## Revoking a listing

Set `"revoked": true` and a human-readable `"revokedReason"` on the
affected entry, rebuild the index, push. Installed copies are never
auto-removed -- users see a warning badge and can uninstall in one click.

## What review does *not* guarantee

Passing `lint`/`scan` and a maintainer's read means "shaped correctly and
free of the crudest red flags," not a security audit. See
`aos_ext_cli.py`'s own `scan` documentation for what it is and isn't.
