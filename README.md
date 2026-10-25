# Aoma Video Squeeze

A free, private, in-browser video compressor & converter — part of the **Aoma AI Agency Free Tools Desk**.

**What it does:** shrinks and converts videos (MP4 / WebM) entirely on the visitor's device. No upload, no server processing, no signup, no watermark. The file never leaves the user's machine — that's the product's core promise and it's architectural, not a policy: there is no upload code path in this app.

## Aoma presets (our original layer)

| Preset | What it does |
|---|---|
| 📤 Send Anywhere | Re-encodes to fit under 16 MB (WhatsApp / email / social upload limits). Bitrate is computed from duration and **never exceeds the source bitrate** — a small file never comes out bigger. |
| 📱 Reels · TikTok · Shorts | Converts to vertical 1080×1920 (scale + pad), CRF 26. |
| 🎞️ Same Look, Smaller File | CRF 28 H.264 at original dimensions (capped at 1920 wide). |
| 🌐 Convert to WebM | VP9 (CRF 34) + Opus, width capped at 1280. Note: VP9 in a browser is slow — fine for short clips. |

## What's ours vs. what's upstream

- **Ours (AARMAJ LLC):** `index.html` — the interface, the preset logic, the copy, the branding. Written for this product.
- **Upstream (unmodified):** everything under `vendor/` is the [ffmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm) build of [FFmpeg](https://ffmpeg.org). See [UPSTREAM.md](UPSTREAM.md) for exact versions, licenses, and source links. We credit it on the page itself, in the footer — loudly, on purpose.

## Run locally

Any static server works:

```bash
cd aoma-video-squeeze
python3 -m http.server 8000
# open http://localhost:8000
```

No build step. The engine (`vendor/core/ffmpeg-core.wasm`, ~31 MB) is self-hosted in this repo so the tool has zero third-party runtime dependencies and works offline after first load.

## Deploy

Static hosting via GitHub Pages from this repo. **Live: https://aarmajus.github.io/aoma-video-squeeze/** (page 200, engine wasm served as `application/wasm`; a custom aomaai.com tools-desk URL can point here later).

## Verification (2026-10-25)

Driven end-to-end in headless Chromium at iPhone size (390×844) before first deploy:

- Send Anywhere preset: 0.3 MB test clip → smaller output, correct % readout, downloadable blob ✔
- Vertical preset: scale+pad filter chain executes clean ✔
- WebM preset: 0.3 MB → 0.2 MB output ✔ (VP9 in single-thread WASM took ~2 minutes for a 3-second clip — works, but set expectations: short clips only)
- No horizontal overflow at 390 px; zero console errors ✔
- Metadata-failure fallback: if a browser can't read the file's duration, the UI still proceeds ✔

## Limits (honest)

- Browser memory caps very large files; the UI advises ~200 MB or less.
- Single-threaded WASM build: slower than a desktop app. Multi-threaded builds need cross-origin-isolation headers; revisit if the host config allows.
- VP9/WebM encoding is the slowest preset.
