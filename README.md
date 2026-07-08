# 🏐 Volleyball Labeler

**Label a volleyball video in your browser** — where the ball is, who did what, and
who won each rally.

It runs entirely inside your web browser. Nothing gets installed, and your videos are
**never uploaded anywhere** — they stay on your machine. The result is a small file of
labels used to train and check a volleyball-analysis model.

No coding needed. If you can use a web browser, you can use this.

There are three labeling modes, and you switch between them with the buttons at the top:

- **● Ball** — mark where the ball is, moment to moment.
- **Actions** — mark who did what (serve / attack / block / dig / set) and how it ended.
- **Score** — mark who won each rally.

---

## What you'll do (4 steps)

1. **Get the app** onto your computer.
2. **Open it** in your web browser.
3. **Load a video** and label it.
4. **Save / send** your work.

---

## Step 1 — Get the app onto your computer

### Easiest way (no tools needed)
1. On this project's GitHub page, click the green **`< > Code`** button → **Download ZIP**.
2. Unzip it:
   - **Windows:** right-click the file → **Extract All…** → **Extract**.
   - **macOS:** double-click the file.
   - **Linux:** right-click → **Extract Here** (or `unzip *.zip`).

You'll get a folder with a file named **`index.html`** inside.

### If you know git
```
git clone <repository-url>
```

---

## Step 2 — Open the app in your browser

Inside the folder, **double-click `index.html`**. It opens in your default browser
(Chrome, Edge, Firefox, or Safari). In most cases you're done — skip to Step 3.

> **If your video later refuses to load** (some browsers block local videos for
> security), use the "local server" method below — one command, ~30 seconds. You only
> need it if double-clicking didn't let the video play.

<details>
<summary><b>▶ Local server method (only if the video won't load)</b></summary>

Needs **Python** (free). Pick your operating system:

### 🪟 Windows
1. Open the folder in **File Explorer**.
2. Click the **address bar** (where the folder path shows), type **`cmd`**, press **Enter**.
   A black **Command Prompt** opens, already in the folder.
3. Type this and press Enter:
   ```
   python -m http.server 8000
   ```
   - If it says *"python is not recognized"*, try **`py -m http.server 8000`**.
   - If you have neither, install Python from <https://www.python.org/downloads/>
     (tick **"Add Python to PATH"** during setup) and try again.
4. Open your browser to **http://localhost:8000**
5. When done, close the window (or press **Ctrl + C**).

### 🍎 macOS
1. Open **Terminal** (Cmd + Space, type *Terminal*, Enter).
2. Type **`cd `** (with a space), **drag the folder** into the window, press **Enter**.
3. Run:
   ```
   python3 -m http.server 8000
   ```
4. Open your browser to **http://localhost:8000**. Stop with **Ctrl + C**.

### 🐧 Linux / Unix
1. Open a terminal in the folder (`cd /path/to/folder`).
2. Run `python3 -m http.server 8000`, then open **http://localhost:8000**. Stop with **Ctrl + C**.

</details>

---

## Step 3 — Label the video

Click **Load video** (top-left) and choose a file. Then pick a mode at the top.

Controls shared by every mode: a **scrubber** sits right below the video (outside the
frame, so it never covers anything you'd click) — drag it to jump anywhere; it shows
current / total time. Also **space** = play/pause, **`→`** / **`←`** = step forward/back,
**`z`** = undo, and the **speed** buttons (1× / .5× / .25×) slow fast rallies down. The
colored bar further down shows everything you've labeled — click it to jump too.

**Reviewing / correcting.** Click the **label count** in the top bar to open a **list of
all your labels**. Click any entry to **jump to it** (it also switches to the matching
mode), then fix it — re-click to re-place a ball, or hit **✕** in the list to delete it.
Handy for cleaning up mistakes without hunting through the video.
**⛶ Fullscreen** (or press **`f`**) fills the screen with the video *and* the controls —
so you get a bigger frame for precise clicking without losing any tools; press **`f`** or
**Esc** to exit.

**Audio waveform.** Below that, a waveform shows the video's sound synced to playback —
**contacts and whistles appear as spikes**, with a gold line marking the current moment.
**Click** it to jump to a sound, or **drag it side to side** (or two-finger / horizontal
scroll) to scrub through the audio. It's especially useful when something happens **off-frame
or in a blur** — e.g. a serve you can *hear* but can't *see* — so you can find the moment
by its sound and label it. (Adjust the **window** seconds to zoom in/out; untick **show**
to hide it. Your labels appear as colored ticks on it too.)

### ● Ball mode — where is the ball
| Do this | To mark |
|---|---|
| **Click the ball** | its position — the video then **jumps ahead a little** so you can immediately click the next one |
| **`n`** | **no ball** in this frame (between points / off-screen) |
| **`h`** | ball is **hard to see / hidden** behind a player |

A faint green ring shows your last ball so your eye can follow it. The **step** box sets
how far it jumps after each click (0.30 s is a good default).

### Actions mode — who did what
1. **Pause at the moment of contact** (the spike hit, the serve toss-contact, the block touch).
2. **Click the player** who made the play.
3. Pick the **action**: **`s`** serve · **`a`** attack · **`b`** block · **`d`** dig · **`t`** set.
4. Pick the **outcome** with **`1`** / **`2`** / **`3`** (some have a 4th) — e.g. attack →
   kill / in / error, set → assist / kill / in / error.

That's it — click, one key, one key. Press **`z`** to cancel or undo.

**How to do it well (and quickly):**
- **One marker per action** — it's a single event, *not* a track. You don't re-mark the
  player as they move; you place one marker at the contact frame and move on.
- **Approximate is fine.** Time within ~0.1–0.2 s of the hit is plenty (a training clip is
  taken from a window around the mark), and the position just needs to be **on the right
  player** (roughly their torso). What matters is the *right player* and the *right
  action/outcome* — not a frame-perfect or pixel-perfect mark.
- **No clear contact frame?** (blurred / hidden / skipped by the framerate) — use the
  nearest frame where you can place the player. The action/outcome usually come from the
  play context, not that one frame.
- **Two plays at the same instant** (a spike *and* a block) — label them as **two separate
  actions**: click the attacker → classify, then click the blocker → classify. Both are
  saved at that moment.
- **Not sure it happened, or who did it? Skip it.** A wrong label hurts more than a missing
  one — only label what you can confidently identify.

### Score mode — who won each rally
Play the match, and each time a point is scored, press **`1`** if the **left** team won
the rally or **`2`** if the **right** team won (or use the buttons). The running score at
the current time is shown. Each mark is one point / one rally.

**Tips for good labels**
- **Spread them out** — many different rallies and situations beat thousands of clicks
  from one rally.
- In Ball mode, **use `n`** on dead-ball moments — the model learns when *not* to guess.

---

## Step 4 — Save / send your work

Top-right, two options:

- **⬇ Export video + labels** *(use this to send your work to someone)* — packages your
  **video and labels into one `.zip`** (e.g. `match_01_labeled.zip`). One file has
  everything. (Big videos take a few seconds — you'll see a progress %.)
- **labels .json** — just the labels (small file), for when the other person already
  has the video.

Your work also **auto-saves in the browser**, so closing the tab and reopening the same
video keeps your labels. To resume from a file, click **Import** and pick a `.json`.

---

## What's in the saved file

```json
{
  "video": "match_01",
  "labels": [
    { "t": 6.30, "type": "ball", "x": 1187, "y": 402 },
    { "t": 6.60, "type": "ball_absent" },
    { "t": 7.10, "type": "ball_hard" },
    { "t": 9.80, "type": "action", "x": 1040, "y": 360, "action": "attack", "outcome": "kill" },
    { "t": 12.4, "type": "score", "scorer": "left" }
  ]
}
```

- `t` — time in seconds from the start of the video.
- `type` — `ball` / `ball_absent` / `ball_hard` / `action` / `score`.
- `x`,`y` — pixel location in the video's original resolution (0,0 = top-left).
  On a `ball` it's the ball; on an `action` it's the player who made the play.
- `action` — `serve` / `attack` / `block` / `dig` / `set`; `outcome` is that action's
  result (e.g. `kill`, `ace`, `error`, `in`, `stuff`, `touch`, `dig`, `assist`).
- `scorer` — `left` or `right` (which team won that rally).

---

## Common questions

**Do I need internet?** No. Once you have the folder, it works fully offline.

**Is my video uploaded anywhere?** No — it's read directly from your disk and never
sent over the network. No accounts, no servers.

**Which browsers work?** Chrome, Edge, Firefox, Safari (recent versions).

**I lost my labels!** They auto-save per video in the browser. Same browser + same video
= they come back. Export a file for safekeeping.

**Nothing happens when I click.** Make sure a video is loaded, you're in the right mode,
and you're clicking *on the video image* (not the buttons).

---

## License

MIT — free to use, change, and share. See [`LICENSE`](LICENSE).
