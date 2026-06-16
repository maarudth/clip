# ◢ Clip — a browser video editor in a single HTML file

**Clip** is a complete video editor that runs entirely in your browser. No install, no
account, no backend, no upload. Your footage never leaves your machine — it's one
self‑contained `index.html` with **zero build step and zero dependencies**.

### ▶ [Try it live → maarudth.github.io/clip](https://maarudth.github.io/clip/)

![Clip video editor — timeline, keyframe zoom/pan, transitions and titles](demo.gif)

## Highlights

- **Multi‑clip timeline** — drag to reorder, drag edges to trim, zoom in/out, "Fit" to frame the whole edit. Thumbnails on every clip.
- **Cuts & splits** — split at the playhead (`S`), delete, or use the Cut tool to slice exactly where you click.
- **Transitions** — crossfade (dissolve) and dip‑to‑black between clips, with adjustable duration.
- **Fades** — per‑clip fade in / out from black.
- **Ken Burns zoom & pan** — animate scale and position with keyframes; markers are draggable along the timeline *and across clip boundaries*.
- **Speed** — 0.1× to 16× on a log slider (slow‑mo lengthens the clip); type an exact rate for anything in between.
- **Text & titles** — styled overlays (font, size, color, alignment, bold, backdrop box) with their own timeline track; drag to position right on the preview. Titles can run past the last clip for title‑on‑black cards.
- **Color cards** — solid‑color clips for intros/outros that take transitions and text like any clip.
- **Export** — frame‑accurate **H.264 MP4** (+ AAC audio) via the WebCodecs API, an animated **GIF** export, and a **WebM** fallback for older browsers.
- **Save / autosave** — projects (video bytes and all) live in your browser's IndexedDB; edits autosave and restore on next launch.
- **Built‑in help** — click the **?** in the toolbar for a full in‑app guide.

## Run it

It's just one file, but **how you open it matters** for video playback (see the box below). In order of preference:

- **[Use the hosted version](https://maarudth.github.io/clip/)** — nothing to download, works everywhere. **or**
- **Serve it locally over `http://localhost`** — best if you've cloned the repo. Pick whichever you have installed:

  ```bash
  # Option A — Python (ships with macOS/Linux; on Windows install from python.org)
  python -m http.server 8000
  #   …then open  http://localhost:8000/index.html

  # Option B — Node.js (no install needed, npx fetches it on the fly)
  npx serve
  #   …it prints a URL like http://localhost:3000 — open that
  ```

  Run the command **from inside the `video-editor` folder** (the one containing `index.html`).
  Leave that terminal window open while you work — it *is* the server. Close it to stop.

Then click **＋ Add video** (or drag video files onto the window) and start editing.

> ### ⚠️ Don't just double‑click `index.html`
> Opening the file directly gives it a `file://` address, which browsers treat as an
> isolated, locked‑down origin. Video **may play for a second and then freeze** while the
> timeline keeps moving (the decoder stalls), and you'll see a console error about
> *"`file:` URLs are treated as unique security origins."* Serving over `http://localhost`
> (or using the hosted version) fixes this completely — that's why the steps above are
> recommended. It's also required for browser extensions to attach to the page.
>
> **Heads‑up:** saved projects are stored *per address*. A project saved while running on
> `http://localhost:8000` won't appear on `http://localhost:3000` or on the hosted site —
> keep using the same URL, and always keep your original source files.

## Keyboard shortcuts

| Action | Key |
| --- | --- |
| Play / pause | `Space` |
| Split at playhead | `S` |
| Delete selected | `Del` |
| Step 1 frame | `←` / `→` |
| Step 1 second | `Shift` + `←` / `→` |
| Jump to start / end | `Home` / `End` |
| Undo / redo | `Ctrl`/`⌘` `+Z` / `+Shift+Z` |
| Save project | `Ctrl`/`⌘` `+S` |

## How it works

- **Preview** runs on a master clock driving native `<video>` playback for smooth, in‑sync audio.
- **Compositing** (transitions, keyframe zoom/pan, fades, text) is unified so the preview matches the export frame‑for‑frame.
- **Export** is deterministic: it seeks one frame at a time and encodes with the browser's `VideoEncoder` — it is *not* a screen recording, so the result is exact regardless of playback speed.
- Only the export muxers ([`mp4-muxer`](https://github.com/Vanilagy/mp4-muxer) and [`gifenc`](https://github.com/mattdesl/gifenc)) load from a CDN, and only at the moment you export. Everything else is offline.

## Browser support

MP4 export needs the **WebCodecs API** (Chrome / Edge, recent Safari). Where it's
unavailable, Clip automatically falls back to **WebM** via `MediaRecorder`. GIF export
works wherever ES modules and `<canvas>` do.

> **Note:** saved projects live in this browser on this machine. Clearing site data
> wipes them, so keep your source files.

## License

[MIT](LICENSE) — free to use, modify, and share.
