# Markiii Live — OBS Avatar Studio 🦍

A simple, single-page live avatar tool for [Markiii](https://x.com/MarkiiiWeb3). Real-time lip sync, periodic blink, plus Glasses and Green Screen overlays. One-click OBS Mode for clean window capture.

Built as a stripped-down sibling of [SolQuicks Live](https://github.com/BHALEYART/SolQuicks-OBS) and [BHB Live](https://bigheadbillionaires.com/live/) — same lip-sync algorithm, but no auto-expression engine. The Markiii base already has eyes built in, so the only animation needed is mouth and blink.

---

## Quick start

1. Drop these files into the repo root:
   - `index.html` — control panel + built-in OBS Mode
   - `obs.html` — optional pure-overlay page for OBS Browser Source
   - `style.css` — dark + lemon yellow theme used by `index.html`
2. Put all the `Markiii-*.png` files into an `assets/` subfolder.
3. Serve the folder over HTTPS or `localhost` (browsers block mic on `file://`).
   - Easiest: GitHub Pages, Vercel, or `npx serve .` from a terminal.
4. Open `index.html`, click **🎤 Start Mic**, allow mic access, start talking.

Final layout:

```
Markiii-OBS/
├── index.html
├── obs.html
├── style.css
├── README.md
└── assets/
    ├── Markiii-Base.png
    ├── Markiii-Blink.png
    ├── Markiii-Glasses.png
    ├── Markiii-Green.png
    ├── Markiii-Ahh.png
    ├── Markiii-Eee.png
    ├── Markiii-Ehh.png
    ├── Markiii-Mmm.png
    ├── Markiii-Rrr.png
    ├── Markiii-Smile.png
    ├── Markiii-Sss.png
    └── Markiii-Uhh.png
```

---

## How OBS capture works

### Option A — Window Capture (recommended)

1. Open `index.html`, dial in lip sync / blink / glasses / green.
2. Click **▶ Enter OBS Mode**. UI vanishes; just the avatar remains.
3. In OBS: **+ Source → Window Capture →** pick your browser window.
4. If Green Screen is on, add a **Chroma Key filter** (default green) in OBS.
5. To exit: hover the **top-left corner** for the faded "× Exit OBS Mode" button. Or press **Esc**.

### Option B — Browser Source (two windows)

1. Open `index.html` as the control panel.
2. Copy the OBS URL from the **OBS Setup** section.
3. In OBS: **+ Source → Browser Source →** paste URL, set Width × Height (e.g. 1000 × 1000), enable **Shutdown source when not visible**.
4. The Browser Source runs its own mic and listens to your control-panel settings via `BroadcastChannel`.

---

## Features

| Feature | What it does |
|---|---|
| **Lip Sync** | Four-tier mic volume → mouth file. Four styles: Default (Ehh), Casual (Uhh), Smiley (Smile), Hissy (Sss/Rrr). |
| **Auto Blink** | Toggle + interval slider (1–12s). Blinks fire every interval while the mic is live. Off by default. |
| **Glasses** | Overlays `Markiii-Glasses.png` on top of the character. |
| **Green Screen** | Draws `Markiii-Green.png` as the very top layer for OBS Chroma Key. |
| **OBS Mode** | Hides all UI, scales avatar to fill the window. Faded corner button exits or pauses mic. Esc also exits. |

---

## Layer order (top to bottom)

1. **Green Screen overlay** (`Markiii-Green.png`) — *if toggle on*
2. **Glasses overlay** (`Markiii-Glasses.png`) — *if toggle on*
3. **Blink overlay** (`Markiii-Blink.png`) — *while blinking*
4. **Mouth** (current lip-sync frame)
5. **Base** (`Markiii-Base.png`) — always

Green Screen sits at the very top so the Chroma Key cuts the background cleanly without nibbling at the glasses or blink frames.

---

## Lip sync mouth mapping

| Volume tier | Default (Ehh) | Casual (Uhh) | Smiley | Hissy (Sss) |
|---|---|---|---|---|
| Silent (< 0.018) | Mmm | Mmm | Smile | Mmm |
| Quiet (< 0.05)   | Eee | Eee | Eee   | Sss |
| Mid (< 0.12)     | Ehh | Uhh | Smile | Rrr |
| Loud (≥ 0.12)    | Ahh | Ahh | Ahh   | Ahh |

Thresholds are tuned for typical speaking volume. If your mic runs hot/quiet, tweak `getMouthTier` in `index.html` and `obs.html` (keep them in sync).

---

## Browser support

Works in any modern Chromium browser (Chrome, Edge, Brave, Arc) and Firefox. Safari works but `getUserMedia` requires a user gesture (the **Start Mic** button handles that). Mobile Safari/Chrome work too — OBS Mode will fill the screen.

---

## Credits

Built for [Markiii](https://x.com/MarkiiiWeb3) by [BHaleyArt](https://github.com/BHALEYART). Engine derived from BHB Live and SolQuicks Live.
