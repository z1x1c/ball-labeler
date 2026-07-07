# 🏐 Ball Labeler

**Mark where the ball is in a volleyball video, one click at a time.**

It runs entirely inside your web browser. Nothing gets installed on your computer,
and your videos are **never uploaded anywhere** — they stay on your machine. The
result is a small file listing the ball's position over time, used to train a
ball-tracking model.

No coding needed. If you can use a web browser, you can use this.

---

## What you'll do (the whole thing in 4 steps)

1. **Get the app** onto your computer.
2. **Open it** in your web browser.
3. **Load a video** and click the ball as it moves.
4. **Save** your labels to a file.

That's it. Below is the same thing, spelled out.

---

## Step 1 — Get the app onto your computer

### Easiest way (no tools needed)
1. On this project's GitHub page, click the green **`< > Code`** button.
2. Click **Download ZIP**.
3. Find the downloaded `.zip` file and unzip it:
   - **Windows:** right-click the file → **Extract All…** → **Extract**.
   - **macOS:** double-click the file.
   - **Linux:** right-click → **Extract Here** (or `unzip ball-labeler.zip`).

You now have a folder called `ball-labeler` with a file named **`index.html`** inside.

### If you know git
```
git clone <repository-url>
```

---

## Step 2 — Open the app in your browser

Inside the folder, **double-click `index.html`**. It should open in your default web
browser (Chrome, Edge, Firefox, or Safari). In most cases, you're done — skip to Step 3.

> **If your video later refuses to load** (some browsers block videos opened this way
> for security), use the "local server" method below. It's one command and takes 30
> seconds. You only need this if double-clicking didn't let the video play.

<details>
<summary><b>▶ Local server method (only if the video won't load)</b></summary>

This needs **Python** (free). Pick your operating system:

### 🪟 Windows

1. Open the `ball-labeler` folder in **File Explorer**.
2. Click the **address bar** at the top (where the folder path is shown), type **`cmd`**,
   and press **Enter**. A black **Command Prompt** window opens, already in the folder.
3. Type this and press Enter:
   ```
   python -m http.server 8000
   ```
   - If it says *"python is not recognized"*, try **`py -m http.server 8000`** instead.
   - If you have neither, install Python from <https://www.python.org/downloads/>
     (during setup, tick **"Add Python to PATH"**), then repeat.
4. Open your browser and go to: **http://localhost:8000**
5. When you're done labeling, close the Command Prompt window (or press **Ctrl + C**).

### 🍎 macOS

1. Open the **Terminal** app (press **Cmd + Space**, type *Terminal*, press Enter).
2. Type **`cd `** (with a space), then **drag the `ball-labeler` folder** into the
   Terminal window and press **Enter**.
3. Type this and press Enter:
   ```
   python3 -m http.server 8000
   ```
4. Open your browser and go to: **http://localhost:8000**
5. When done, close Terminal (or press **Ctrl + C**).

### 🐧 Linux / Unix

1. Open a **terminal** in the `ball-labeler` folder (`cd /path/to/ball-labeler`).
2. Run:
   ```
   python3 -m http.server 8000
   ```
3. Open your browser at **http://localhost:8000**. Stop it with **Ctrl + C**.

</details>

---

## Step 3 — Label the ball

1. Click **Load video** (top-left) and choose a video file from your computer.
2. Now label. The app is built so you can move through a whole video quickly:

| Do this | To mark… |
|---|---|
| **Click on the ball** | the ball's position — the video then **jumps ahead a little** so you can immediately click the next one |
| Press **`n`** | **no ball** in this frame (e.g. between points) |
| Press **`h`** | ball is **hard to see / hidden** behind a player |
| Press **`z`** | **undo** your last label |
| Press **`→`** / **`←`** | skip **forward / back** without labeling |
| Press **`space`** | play / pause the video |

Helpful bits on screen:
- A faint **green ring** shows where you put the ball last time, so your eye can follow it.
- The **step** box controls how far the video jumps after each click (0.30 seconds is a good default).
- The **speed** buttons (1× / .5× / .25×) slow the video down for fast rallies.
- The colored **bar under the video** shows everything you've labeled — click it to jump anywhere.

**Tips for good labels**
- **Spread them out.** Labeling many different rallies, angles, and moments is far more
  useful than thousands of clicks from a single rally.
- **Use `n` on dead-ball moments** (ball out of play / off-screen). These "no ball"
  examples teach the model when *not* to guess.

---

## Step 4 — Save / send your work

Top-right, you have two ways to save:

- **⬇ Export video + labels** *(use this to send your work to someone)* — packages
  **your video and your labels together into one `.zip` file** (e.g.
  `match_01_labeled.zip`). Whoever receives it gets everything they need in a single
  file. (Large videos take a few seconds to package — you'll see a progress %.)
- **labels .json** — saves **just the labels** (a small `match_01.json`), for when the
  other person already has the video.

Also:
- Your work is **auto-saved inside the browser**, so if you close the tab and come back
  to the same video, your labels are still there.
- To continue from a saved file later, click **Import** and pick a `.json` (or a
  `.json` you pulled out of a `.zip`).

---

## What's in the saved file

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

- `t` — time in seconds from the start of the video.
- `type` — `ball` (with pixel position `x`,`y`), `ball_absent` (no ball), or
  `ball_hard` (hidden/occluded).
- `x`,`y` — the ball's pixel location in the video's original resolution (0,0 = top-left).

---

## Common questions

**Do I need internet?** No. After you have the folder, it works fully offline.

**Is my video uploaded anywhere?** No. It's opened directly from your disk by the
browser and never sent over the network. There are no accounts and no servers.

**Which browsers work?** Chrome, Edge, Firefox, and Safari (recent versions).

**I lost my labels!** They auto-save per video in the browser. As long as you use the
same browser and load the same video, they come back. Export a `.json` for safekeeping.

**Nothing happens when I click the ball.** Make sure a video is loaded first (Step 3),
and click *on the video image*, not the controls.

---

## License

MIT — free to use, change, and share. See [`LICENSE`](LICENSE).
