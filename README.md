Photo Compiler

A browser-based tool that compiles a sequence of photos into a single MP4 video — no server, no upload, everything runs locally in your browser.

Features
Drag-and-drop or multi-select upload — drop or pick several JPG/PNG files at once; they're read in the order added.
Per-photo duration control — every photo in the filmstrip has its own − / + extend buttons to set how long it stays on screen.
Default duration + apply-to-all — set a default for new uploads, or reset every photo already loaded to one duration in a single click.
Reorder and remove — move photos up/down in the sequence or delete any of them from the filmstrip.
Live preview — play back the sequence at its real timing before exporting, with a scrubbable progress track.
Loading bar — a status bar at the bottom of the page shows when photos are still being read (Reading photos… 3/8) versus ready to render, and later shows encoding progress the same way. Export is disabled until loading finishes.
Lossless MP4 (H.264) export — each frame is captured as a lossless PNG and encoded with libx264 at crf 0 in yuv444p (no chroma subsampling), so no color — from the photos to the letterbox bars — is degraded by the export. Runs entirely in-browser via ffmpeg.wasm.
Automatic fallback — if the lossless encoder can't load (e.g. no network, or a browser wasm memory limit), export automatically falls back to a high-bitrate MP4 (or WebM as a last resort) instead of failing outright, and tells you which happened.
Hosting it on GitHub Pages

This is a single static HTML file — no build step, no dependencies to install.

Create a new repository and add photo-compiler.html to it.
Optional: rename it to index.html if you want it to load directly at your Pages root URL.
Go to your repo's Settings → Pages.
Under Build and deployment, set Source to your default branch (e.g. main) and folder / (root).
Save. GitHub will give you a URL like:
   https://<your-username>.github.io/<repo-name>/
Open that URL — the lossless encoder needs a real http(s):// origin to load, so it will not work correctly if you just double-click the file locally (see below).
Running it locally (without GitHub)

The lossless encoder (ffmpeg.wasm) is loaded as an ES module from a CDN, and browsers block that kind of cross-origin module fetch from a file:// page. To test locally before pushing to GitHub, serve the folder instead of opening the file directly:

bash
# from the folder containing photo-compiler.html
python -m http.server 8000

Then open http://localhost:8000/photo-compiler.html in your browser.

Usage
Drop photos onto the dropzone, or click it to browse — you can select multiple files at once.
Wait for the status bar at the bottom to say "Ready" before rendering (it'll tell you if it's still reading files).
Adjust each photo's duration individually in the filmstrip, or set a default and apply it to all.
Reorder or remove photos as needed using the arrows and ✕ button on each card.
Press play to preview the sequence at its real timing.
Click Export video (.mp4). The status bar will show Preparing frame X/Y… then Encoding H.264 MP4… NN%. When it finishes, the file downloads automatically as compiled-photos.mp4.
Technical notes
Output frame size is fixed at 1920×1080; photos are letterboxed (never cropped or stretched) to fit, preserving full original detail.
Output is a constant 30fps — still photos are simply held/duplicated for their duration, which costs nothing in quality since it's the same untouched pixels each time.
yuv444p + crf 0 is the losslessest H.264/MP4 combination available, but note the trade-off: files are large, encoding is slower than a compressed export, and some players/older devices have weaker support for 4:4:4 H.264 than for the more common 4:2:0. VLC and most desktop editors (Premiere, DaVinci, Resolve) handle it fine.
Nothing is uploaded anywhere — photos, previews, and the final export all stay on your device; the "CDN" dependency is only for loading the ffmpeg.wasm encoder library itself.
Browser support

Works best in current versions of Chrome, Edge, and Safari. Firefox can preview and play back sequences fine; if the lossless encoder can't load in your environment for any reason, export automatically falls back to the browser's built-in recorder (MP4 where supported, WebM otherwise).
