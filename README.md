# live-dist

Prebuilt release binaries for [`live`](https://liveshortly.com) — **no source here**, only release assets.

## Install

```sh
curl -fsSL https://liveshortly.com/i.sh | bash
```

The installer detects your platform, downloads the matching binary from
[Releases](https://github.com/resapce/live-dist/releases), verifies it against
its published `.sha256`, and installs it. No Go toolchain required.

## Assets

Each release carries, for `darwin`/`linux` × `amd64`/`arm64`:

| File | What |
|---|---|
| `live-vX.Y.Z-<os>-<arch>.gz` | gzipped static binary |
| `live-vX.Y.Z-<os>-<arch>.sha256` | SHA-256 of the *uncompressed* binary |
| `latest.txt` | the current version pointer |

Mirrored at <https://liveshortly.com/install/>.

## Verify by hand

```sh
gzip -dc live-v1.0.1-darwin-arm64.gz > live
shasum -a 256 live            # must equal live-v1.0.1-darwin-arm64.sha256
chmod +x live && ./live version
```
