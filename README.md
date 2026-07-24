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
