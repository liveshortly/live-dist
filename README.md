# live-dist

Prebuilt release binaries for [`live`](https://github.com/liveshortly/live). No source here —
only release assets.

## Install

```sh
curl -fsSL https://raw.githubusercontent.com/liveshortly/live-dist/main/install.sh | bash
```

Detects your platform, downloads the matching binary, verifies it against its
published `.sha256`, and puts `live` on your PATH. No Go toolchain.

Pin a version with `bash -s -- --version v1.0.1`.

After installing, follow the [self-hosted setup instructions](https://github.com/liveshortly/live#choose-your-server).
The installer does not sign you in. The Pocket daemon uses a key; the full web
platform uses your operator's Google OAuth setup. Homebrew is available with
`brew tap liveshortly/tap` and `brew install liveshortly/tap/live` once the tap is public.

## Assets

Every release carries `darwin`/`linux` × `amd64`/`arm64`:

| File | What |
|---|---|
| `live-vX.Y.Z-<os>-<arch>.gz` | gzipped static binary |
| `live-vX.Y.Z-<os>-<arch>.sha256` | SHA-256 of the **uncompressed** binary |
| `latest.txt` | current version pointer |

The latest-version pointer is served at
`https://github.com/liveshortly/live-dist/releases/latest/download/latest.txt`.
No liveshortly.com mirror is needed. `LIVE_INSTALL_BASE` can select an
operator-owned flat mirror when required.

## Verify by hand

```sh
V=v1.0.1; P=darwin-arm64
B=https://github.com/liveshortly/live-dist/releases/download/$V
curl -fsSLO "$B/live-$V-$P.gz"
curl -fsSL  "$B/live-$V-$P.sha256"

gzip -dc "live-$V-$P.gz" > live
shasum -a 256 live          # must equal the .sha256 above
chmod +x live && ./live version
```

The `.sha256` covers the **extracted** binary, not the `.gz`. If you are writing
a package manifest that checksums the downloaded file — Homebrew does — hash the
`.gz` yourself.

## Why this repo exists

This repository keeps the existing release URLs stable while the source moves to
open-source distribution. Source, issues, and contribution instructions live in
[liveshortly/live](https://github.com/liveshortly/live). Installers and updates use
GitHub directly; no managed LiveShortly website is required.
