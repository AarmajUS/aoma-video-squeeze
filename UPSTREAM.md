# Upstream provenance — read this before reusing this repo

Everything under `vendor/` is **unmodified upstream open-source software**, vendored (copied in) so the tool runs with zero third-party runtime dependencies. It is NOT Aoma/AARMAJ code and we do not claim it.

| Component | Version | License | Source |
|---|---|---|---|
| @ffmpeg/ffmpeg (JS wrapper) | 0.12.15 | MIT | https://github.com/ffmpegwasm/ffmpeg.wasm |
| @ffmpeg/util | 0.12.2 | MIT | https://github.com/ffmpegwasm/ffmpeg.wasm |
| @ffmpeg/core (FFmpeg compiled to WebAssembly) | 0.12.10 | **GPL-2.0-or-later** | Build scripts: https://github.com/ffmpegwasm/ffmpeg-core · FFmpeg source: https://ffmpeg.org |

## GPL note (why this file exists)

The FFmpeg core build includes GPL-licensed components (e.g. libx264). Our use:

- The core binaries are **unmodified** upstream builds, shipped as separate files (`vendor/core/ffmpeg-core.js`, `vendor/core/ffmpeg-core.wasm`) and loaded by our page through the engine's public API.
- The corresponding source is publicly available at the upstream links above, which is where any recipient can obtain it — that availability is stated here and credited in the app footer.
- Aoma's own code is `index.html` (interface + preset logic). It does not contain FFmpeg code.

Downloaded from the npm registry on 2026-10-25:
`@ffmpeg/ffmpeg/-/ffmpeg-0.12.15.tgz`, `@ffmpeg/util/-/util-0.12.2.tgz`, `@ffmpeg/core/-/core-0.12.10.tgz`.

## The house rule this repo follows

Credit loudly, own the layer. Every Aoma tool built on open source carries a file like this one: exact upstream versions, exact licenses, and a bright line between the upstream engine and the original Aoma work on top of it.
