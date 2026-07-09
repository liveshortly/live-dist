# live-dist

Prebuilt release binaries for [`live`](https://liveshortly.com). No source here —
only release assets.

## Install

```sh
curl -fsSL https://liveshortly.com/i.sh | bash
```

Detects your platform, downloads the matching binary, verifies it against its
published `.sha256`, and puts `live` on your PATH. No Go toolchain.

Pin a version with `bash -s -- --version v1.0.1`.

There is a Homebrew tap, but the tap repo is not public yet, so `brew tap
liveshortly/tap` only works if you have access to it.

## Assets

Every release carries `darwin`/`linux` × `amd64`/`arm64`:

| File | What |
|---|---|
| `live-vX.Y.Z-<os>-<arch>.gz` | gzipped static binary |
| `live-vX.Y.Z-<os>-<arch>.sha256` | SHA-256 of the **uncompressed** binary |
| `latest.txt` | current version pointer |

Mirrored at <https://liveshortly.com/install/>, which is also where `latest.txt`
is read from — that avoids the GitHub API rate limit.

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

`live` is developed in a private repo. Release assets on a private repo need an
auth token to download, which would break `curl … | sh` for everyone outside the
org. So the binaries are published here, in public, carrying no source.
