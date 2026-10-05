# Macro Wheel — release distribution

**This repository holds releases, not source.** The source lives in
[`Astra123489/macrowheel`](https://github.com/Astra123489/macrowheel), which is private.

## What is here

| | |
|---|---|
| `manifest.json` | the update manifest — §25.15, §25.57 |
| Releases | the Windows installers, one per version |

**`manifest.json` is the file Macro Wheel reads to find out whether an update exists.** It is at a stable path
rather than attached to a release, and that is deliberate: a release-asset URL contains the tag
(`.../download/v6.3.0/manifest.json`), so an updater would have to know the version it is trying to discover.

The updater fetches:

```text
https://raw.githubusercontent.com/Astra123489/macrowheel-releases/main/manifest.json
```

## Why the split, and it is not only tidiness

**GitHub Releases is the distribution mechanism** (§25.53) and §25.64's cost model depends on it being free
static hosting — no server, no database, no backend. **But a private repository's manifest and release assets
cannot be read without authentication**, measured rather than assumed: an unauthenticated request for a private
repo's raw content returns **404** where a public one returns 200.

**And §25.68 forbids the obvious workaround:**

> Release publishing credentials and signing credentials must not be embedded in the application.

A token shipped in the binary would be extractable, so it would fail on its own terms as well as on the clause.
**The separation this repository creates is the one §25.68 describes** — *"the release process should separate:
source/build credentials, release credentials, signing credentials, customer application."*

## What is not here

**Nothing is built here and nothing is written by hand.** `updater/publish.py` in the source repository
generates `manifest.json`, creates the release, uploads the installer and commits the manifest — so a file in
this repository is always the output of a release rather than an edit somebody made.
