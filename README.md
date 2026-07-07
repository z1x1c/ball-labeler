# 🏐 Ball Labeler

A tiny, **fully offline** browser tool for labeling the ball position in volleyball
(or any sport) video — frame by frame, continuously, fast. One HTML file, no install,
no server, no accounts, no cloud. Your video **never leaves your machine**.

Built to produce clean training data for a ball-tracking model: click the ball as it
moves through the video and export a JSON of `{t, x, y}` positions.

## Use it

1. **Download / clone this repo.**
2. **Open `index.html`** in a modern browser (Chrome, Edge, Firefox, Safari) — just
   double-click it.
   - If your browser blocks a local video from `file://`, serve the folder instead:
     ```
     python3 -m http.server 8080
     ```
     then open <http://localhost:8080>.
3. Click **Load video** and pick a video file from your disk.
4. **Label** (see below).
5. Click **Export .json** to save your labels.

That's the whole thing. It runs entirely client-side.

## Labeling — continuous & fast

The workflow is built to move through a whole video quickly:

- **Click the ball** → records its position at the current time, then **auto-advances**
  by the step (default 0.30 s) so you can immediately click the next one. Click, click,
  click through the rally.
- **`n`** → *no ball in frame* (dead time, ball off-screen) — a negative example.
- **`h`** → *hard / occluded* (ball hidden behind a player) — flagged, not a position.
- **`z`** → undo the last label (jumps you back to it).
- **`→` / `←`** → skip forward / back by one step without labeling.
- **`space`** → play / pause. **`,` / `.`** → nudge 0.1 s.
- Adjust the **step** size, **playback speed**, and toggle **auto-advance** in the bar.
- The faint green ring shows the **previous ball position** so your eye can follow it.
- The **coverage strip** under the video shows what you've labeled (green = ball,
  red = no-ball, purple = hard); click it to jump anywhere.

Tips for good training data:
- **Diversity beats volume** — label across many rallies, angles, and both light and
  dead-ball moments, rather than thousands of frames from one rally.
- **Negatives matter** — press `n` on dead-ball / empty frames so a model learns when
  *not* to fire.
- A small step (0.2–0.3 s) gives dense, trajectory-like sequences.

## Output format

`Export .json` downloads `<videoname>.json`:

```json
{
  "video": "match_01",
  "labels": [
    { "t": 6.30, "type": "ball", "x": 1187, "y": 402 },
    { "t": 6.60, "type": "ball_absent" },
    { "t": 6.90, "type": "ball_hard" }
  ]
}
```

- `t` — seconds into the video.
- `type` — `ball` (with `x`,`y`), `ball_absent` (no ball), or `ball_hard` (occluded).
- `x`,`y` — pixel coordinates in the video's **native** resolution (origin top-left).

**Import .json** loads a file back so you can resume. Your progress is also
auto-saved in the browser (localStorage, keyed by video name), so a refresh won't
lose it.

## Privacy

100% local. The video is read with an in-browser object URL and never uploaded;
labels live in your browser and in the files you export. There is no network call
anywhere in this app.

## License

MIT — see `LICENSE`.
