# 🏐 Data Labeling FAQ

Everything you need to label volleyball video **well**. Read this once before you start,
then keep it open for reference. If you just want install/setup steps, see the
[README](README.md); this doc is about *how to label and why*.

**The one-sentence version:** watch the rally, mark what happened at the moment it
happened, and when you're not sure — skip it. A smaller clean set beats a big noisy one.

> 💬 **In a hurry?** Open **`faq.html`** (double-click it) and just *ask* — an offline
> assistant answers these questions instantly. No internet needed.

---

## Table of contents

1. [The big picture — what am I doing and why](#1-the-big-picture)
2. [Golden rules](#2-golden-rules)
3. [Getting set up](#3-getting-set-up)
4. [Controls that work in every mode](#4-controls-that-work-in-every-mode)
5. [Ball mode](#5-ball-mode--where-is-the-ball)
6. [Actions mode](#6-actions-mode--who-did-what)
   - [The action types](#the-action-types)
   - [The outcomes (and what they mean)](#the-outcomes)
   - [Tagging the player (jersey number)](#tagging-the-player)
   - [When you can't see the player](#when-you-cant-see-the-player--the-2d-court)
7. [Score mode](#7-score-mode--who-won-each-rally)
8. [2D Court & lineup (rotation)](#8-2d-court--lineup)
9. [Tricky situations](#9-tricky-situations)
10. [Common mistakes to avoid](#10-common-mistakes-to-avoid)
11. [Quality checklist](#11-quality-checklist)
12. [Technical & privacy questions](#12-technical--privacy-questions)

---

## 1. The big picture

**What am I doing?** You're creating the *ground truth* — the examples an AI learns
volleyball from. For each video you mark: where the **ball** is, **who did what** (and how
it ended), and **who won each rally**. The app saves this as a small file of labels.

**Why does it matter?** The model is only ever as good as the labels. Clean, consistent,
varied labels → a model that works. Sloppy or guessed labels → a model that learns the wrong
thing. Your judgment *is* the product.

**Do I need to be a volleyball expert?** No, but knowing the basics helps. This FAQ defines
every term you'll need. If a play is genuinely ambiguous to you, it's fine to skip it.

---

## 2. Golden rules

1. **When in doubt, skip it.** A wrong label actively teaches the model something false; a
   missing label costs nothing. Only label what you can confidently identify.
2. **Mark the moment it happens.** Actions get marked at the **moment of contact**, not
   before or after (details below).
3. **Diversity beats volume.** 200 labels spread across many rallies, players, and
   situations are worth more than 2,000 from one rally. Move around the match.
4. **Be consistent.** Whatever convention you pick (e.g. always mark the contact frame),
   apply it the same way every time. The model can handle "slightly off" better than
   "sometimes this, sometimes that."
5. **Approximate is fine.** You do **not** need frame-perfect timing or pixel-perfect
   clicks. Being on the right player, in the right ~0.2 s, is plenty.
6. **Back up your work.** Export a `.json` (or the video+labels `.zip`) now and then.
   Auto-save lives in the browser, but a file is your real safety net.

---

## 3. Getting set up

**Q: How do I open it?** Double-click `index.html` in the folder you downloaded. It runs in
your browser. (If a video won't load, use the tiny "local server" method in the README.)

**Q: Which commit / version should I use?** Whatever the team agreed on — everyone should
use the **same** version so labels are consistent. Don't mix versions mid-project.

**Q: Where do my labels go?** They **auto-save in your browser**, per video. Close the tab
and reopen the *same* video in the *same* browser and they're still there. But that cache
can be cleared by the browser — so **export a file** to be safe.

**Q: Will I lose my labels if a new version comes out?** No. Your labels live in the
browser's storage and in any files you exported; replacing `index.html` doesn't touch them.
Old exports still open in newer versions (the format only ever adds fields).

**Q: Is my video uploaded anywhere?** **No.** Everything runs locally; the video never
leaves your machine. No accounts, no servers, works fully offline.

---

## 4. Controls that work in every mode

| Key / control | What it does |
|---|---|
| **scrubber** (below the video) | drag to jump anywhere; shows current / total time |
| **space** | play / pause |
| **`→` / `←`** | step one frame forward / back |
| **`,` / `.`** | nudge 0.1 s back / forward (fine control) |
| **speed buttons** (1× / .5× / .25×) | slow fast rallies down |
| **`z`** | **undo** your last label (any type) |
| **`x`** | **delete** the label at the current playhead |
| **`f`** or **⛶** | fullscreen (video *and* controls stay usable) |
| **Ctrl + scroll** (or pinch) | **zoom** toward the cursor for precise marking — badge shows the level, click it to reset |
| **label count** (top bar) | opens the list of all labels — click one to jump to it, ✕ to delete |
| **audio waveform** | contacts & whistles show as spikes; click to jump, drag/scroll to scrub — great for finding a hit you can *hear* but can't *see* |

**Q: The video is a bit shaky / has no sound — is that OK?** Yes. Shaky footage and silent
video are both fine to label. Sound just *helps* (you can find contacts on the waveform); it
isn't required.

---

## 5. Ball mode — where is the ball

Mark where the ball is, moment to moment.

| Do this | To mark |
|---|---|
| **click the ball** | its position (the video then jumps ahead a little so you can click the next one) |
| **`n`** | **no ball** in play (between points, off-screen, dead time) |
| **`h`** | ball is **hard to see / hidden** (behind a player, motion-blurred) |

**Q: Do I have to click the ball on every single frame?** No. Click it as often as is
practical along its path — the "step" box (default 0.30 s) sets how far it auto-jumps after
each click, so you're sampling the trajectory, not every frame.

**Q: What do I do during dead time (between rallies)?** Press **`n`** (no ball) on some of
those frames. This is *valuable* — it teaches the model when **not** to guess a ball. Don't
skip dead time entirely; sprinkle `n` through it.

**Q: The ball is a blur or behind a player — click it or press `h`?** If you can reasonably
tell where it is, click it. If it's genuinely hidden/unclear, press **`h`** (hard). Both are
useful; `h` tells the model "ball's here but ambiguous."

**Q: The ball left the frame — what then?** Press **`n`** (it's not visible). When it comes
back, resume clicking.

**Q: Tips for good ball labels?** Spread across many rallies and situations (serves, rallies,
spikes, free balls). A faint green ring shows your last ball so your eye can follow it.

---

## 6. Actions mode — who did what

This is the richest mode. The flow is always the same:

> **pause at contact → click the player → pick the action → pick the outcome → (type the jersey #)**

**Q: When exactly do I place the marker?** At the **moment of contact** — the instant the
player touches the ball (the spike hit, the hands on the set, the platform on a pass). That
pose+motion is what tells the model a dig from a set from a spike. Don't mark when the player
is "most visible" if that's before/after the touch — mark the touch.

**Q: How precise does the timing need to be?** Within about **0.1–0.2 s** of the contact.
The model takes a short *window* around your mark, so a frame or two off doesn't matter.
Being *consistent* (always aim for the touch) matters more than being exact.

**Q: How precise does the click position need to be?** Just **on the right player** (roughly
their torso). It's used to find *which* player made the play, not a pixel location. Use
**Ctrl+scroll zoom** if they're small on screen.

**Q: One action = one marker, right? Do I follow the player as they move?** One marker, one
event. You do **not** re-mark a player as they move — place a single marker at the contact
frame and move on. It's an *event*, not a track.

**Q: There's no clean contact frame (blur / hidden / skipped by framerate)?** Use the
**nearest** frame where you can place the player. The action and outcome usually come from
the play context, not that one frame. Approximate contact time is fine.

**Q: Two things happen at the same instant (a spike *and* a block)?** Label them as **two
separate actions**: click the attacker → classify, then click the blocker → classify. Both
are saved at that moment.

**Q: Two players overlap, or the one I want is partly behind another — which do I click?**
Click the part of the *acting* player you can see (head or torso), and **zoom**
(`Ctrl+scroll`) to separate them — your click just needs to be closer to them than to the
other player. If they're so stacked you genuinely can't tell who made the play, leave the
jersey off or skip it (don't guess the identity). *(Different from an **overlap / rotation
violation** — players out of position at the serve; a rally whistled dead for that is
**Other → rotation**.)*

**Q: The acting player is fully hidden behind someone — can I still label them?** If you can
see **any part** of them, click it (the model takes a short window, so it usually catches
clearer frames as they move out from behind). If they're **fully hidden**, **skip it** —
don't guess a spot, or the app grabs the *nearest visible* player (the one in front) and
mislabels the action onto them. A missing label beats a wrong one.

**Q: I can't tell what the action was, or who did it?** Skip it. (Golden rule #1.)

### The action types

Pick with a single key after clicking the player:

| Key | Action | What it is |
|---|---|---|
| **`s`** | **serve** | putting the ball in play from behind the end line |
| **`r`** | **receive** | **serve-receive** (passing a serve). *Different from a dig.* |
| **`a`** | **attack** | a spike / hit / tip trying to score |
| **`b`** | **block** | jumping at the net to stop an attack |
| **`d`** | **dig** | defending an **attack** (digging a spike up) |
| **`t`** | **set** | the second contact, setting up an attacker |
| **`o`** | **other** | a rule-break fault *off the ball* (see below) |

**Q: Receive vs Dig — what's the difference?** A **receive** is passing a **serve**. A
**dig** is defending an **attack** (a spiked/hit ball). Same skill, different situation —
and volleyball tracks them separately (passing rating vs digs), so we do too.

**Q: What's "Other" for?** Rallies that end because someone *not playing the ball* broke a
rule — a **net touch**, **foot fault** (foot over the line on serve/center line), **four
hits**, **reach over** the net, **rotation** (out of rotation), or **other**. No model could
guess these from the ball, so you mark them: click the offending player → **Other** → the
fault type. They all mean *point to the other team*.

### The outcomes

After the action, pick the outcome with number keys (**`1`**, **`2`**, …):

| Action | Outcomes |
|---|---|
| **serve** | `ace` · `effective` · `bad` · `error` |
| **receive** | `perfect` · `good` · `in` · `overpass` · `error` · `kill` |
| **attack** | `kill` · `tip` · `block-out` · `in` · `blocked` · `error` |
| **dig** | `perfect` · `good` · `in` · `overpass` · `error` · `kill` |
| **set** | `assist` · `dump` · `good set` · `error` |
| **block** | `stuff` · `touch` · `error` |
| **other** | `net touch` · `foot fault` · `four hits` · `reach over` · `rotation` · `other` |

**What each outcome means:**

- **Serve** — `ace` = point off the serve; `effective` = tough serve (opponent out of
  system); `bad` = easy serve; `error` = missed (net/out/foot fault).
- **Receive / Dig** — a pass/dig quality scale: `perfect` (great, all options open) →
  `good` → `in` (playable but poor); `overpass` = it went **over the net** giving the other
  team a free attack; `error` = shanked / aced / unplayable; `kill` = the rare case it
  directly scored.
- **Attack** — **how the point was won or lost**: `kill` = hard spike scores; `tip` =
  tip/roll scores; `block-out` = you hit off the block and it goes out (a "tool"); `in` =
  the attack was dug up, rally continues; `blocked` = stuffed by the block; `error` = hit
  out or into the net.
- **Set** — `assist` = your set led to a teammate's kill; `dump` = the setter attacked the
  second ball for a point; `good set` = a good, hittable set that just didn't lead to a
  point; `error` = double/lift/handling fault or set out of bounds.
- **Block** — `stuff` = blocked straight down for a point; `touch` = you touched it (slowed
  it, kept it in play — a "soft block"); `error` = net touch / blocking error.

**Q: Some of these quality grades feel subjective (perfect vs good). Does it matter?** Not
much *right now*. The model currently only cares whether a play **scored a point**, **kept
the ball in play**, or was an **error**. The fine grades (perfect/good/effective/tip/
block-out…) are **saved for later** — they're future insurance for when there's enough data
to train finer stats (passing charts, hitting %, tool %). **If grading slows you down, just
pick the coarse one** (`kill` / `in` / `error`) and move on. Consistency on the coarse
buckets is what counts.

**Q: A stuffed attack — do I label `attack → blocked` or `block → stuff`?** Either — they're
the same event from the two sides. Pick one convention (whichever you find easier) and stick
to it. Don't label both for the same touch.

### Tagging the player

**Q: How do I record *which* player did the action?** Right after you pick the outcome,
just **type the jersey number** (`7`, or `1``2` for #12). It attaches to that action and
shows on the marker. **Backspace** fixes a typo. This is what turns labels into **per-player
stats** (kills by #7, passing by #3).

**Q: Is the jersey required?** No — it's optional. Leave it blank and the action is
unattributed (still useful). Tag it when you can read the number and it matters.

**Q: The numbers conflict with the outcome keys!** They don't — digits **before** you pick
an outcome select the outcome; digits **after** set the jersey. The app knows the difference.

### When you can't see the player — the 2D Court

**Q: I can *hear* a serve but the camera doesn't show the server. How do I label it?** Use
the **2D Court** panel. If the lineup is set up (see below), it shows **who's serving** even
off-camera. Then: hear the serve on the waveform → glance at the court → **click that player
on the 2D court** → press **`s`** (serve) → pick the outcome. The label is tagged to their
jersey with no need to click them on the frame.

**Q: Should I label an action in *both* the video and the 2D court?** **No — one label per
action.** Click on the **video frame** whenever you can see the player (that gives the model
a real image location). Only use the **2D court click** as a fallback when the player is
off-camera. Never both for the same event.

**Q: 2D vs 3D — what's the difference, and which do I label on?** The **video** is the
camera's real view (the "3D" scene) — clicking a player there stores their **image
position**, which the ball/pose models actually learn from. The **2D court** is the top-down
schematic — clicking there only picks the **jersey** (from the rotation), with no image
position. **Rule:** click the **video** whenever you can see the player; use the **2D court**
only as a fallback when they're off-camera. Always **one label per action — never both**.

**Q: The server is off-camera and I *haven't* set up the lineup — what then?** You can still
label the serve (find it by sound on the waveform), but **leave the jersey blank** rather
than guessing — an unattributed serve is fine. Setting up the lineup is what lets the 2D
court tell you *who* served off-screen.

---

## 7. Score mode — who won each rally

Play the match; each time a point is scored, press **`1`** if the **left** team won that
rally or **`2`** if the **right** team won (or use the buttons). Each mark = one rally.

**Q: Which team is "left" vs "right"?** Set it with the **Teams are: ↔ Left/Right** (or ↕
Top/Bottom) toggle to match how the teams appear on screen. Keep it consistent for the
whole video.

**Q: Why does score matter for *labeling*?** Two reasons: it's a stat on its own (who's
winning), **and** it drives the **2D Court rotation** — the app figures out who's serving
from the sequence of rally winners. So if you want the serving info, **label who wins each
rally**.

**Q: Do I have to label every single point?** For the rotation feature to stay accurate,
yes — the running server is computed from the full sequence of winners. If you skip points,
the "who's serving" readout will drift (you can nudge it back; see below).

---

## 8. 2D Court & lineup

The 2D Court is a **top-down map that tracks the rotation** so you know who's serving —
handy for off-camera serves. It's a labeling aid, computed purely from your Score labels (no
AI, fully offline).

**Q: How do I set it up?** Click **Set lineup** → enter each team's **6 jersey numbers in
serving order** (the player who serves first, then the next to rotate in, and so on) → pick
**who serves the very first point** → Save.

**Q: What does "serving order" mean exactly?** The order players rotate to the serve for
that team — i.e. list them starting from whoever serves first *for that team*, then the next,
around the rotation. The app cycles through your list as each team wins the serve.

**Q: The highlighted server looks wrong / off by one.** Use **◀ rot / rot ▶** to nudge the
rotation for the serving team. (A common cause is a wrong "serves first" pick or a missed
Score label.) You can also just **click the player you *know* served** — that logs the
correct jersey regardless of the highlight.

**Q: There was a substitution — now the numbers are wrong.** Re-open **Set lineup** and edit
the affected team's order, then Save. (The rotation math assumes a fixed six; subs need a
manual update.)

**Q: What about the libero?** The libero plays back-row and normally doesn't serve, so the
player rotating into the serve position (zone 1) is never the libero. For back-row defense,
just tag whoever actually made the play.

**Q: Does the lineup save / export?** Yes — it's stored per video and travels with your
export file.

---

## 9. Tricky situations

**Q: The ball never has a clear moment of contact (too blurry / framerate skipped it).**
Use the nearest frame you can. Approximate is fine — see Actions timing above.

**Q: A player touches the ball twice quickly, or it's a joust (both teams at the net).**
Label the touches you can clearly attribute; skip the ones you can't. For a joust, if you
can't tell whose touch was last/decisive, skip the individual action but still mark the
**Score** (who won the rally).

**Q: Free ball / down ball (not a real spike) — is it an attack?** Yes, label it `attack`
with the outcome that fits (`in` if it's just returned, `kill` if it scores, etc.). If it's
really a *set/pass over*, use your best judgment; consistency matters more than the exact
label.

**Q: The setter tips the ball over for a point.** That's a **dump** → `set` → `dump`.

**Q: A pass sails over the net to the other side.** That's an **overpass** → `receive` (or
`dig`) → `overpass`.

**Q: The camera cuts / replays / crowd shots.** Skip those — only label live play.

**Q: I'm not sure if it was a dig or a receive.** Ask: was the ball a **serve** (→ receive)
or an **attack** (→ dig)? If you truly can't tell, pick the more likely one and stay
consistent, or skip.

**Q: Do warm-ups / timeouts / between-set footage get labeled?** No — only live rallies.
In Ball mode you can mark dead time with `n`, but don't label actions/score for non-play.

---

## 10. Common mistakes to avoid

- ❌ **Guessing when unsure** → skip instead. Wrong labels are worse than none.
- ❌ **Marking before/after the contact** because the player is clearer there → mark the
  *touch*.
- ❌ **Re-marking the same action as the player moves** → one marker per action.
- ❌ **Labeling only one exciting rally over and over** → spread out; diversity wins.
- ❌ **Flip-flopping conventions** (contact frame vs follow-through; `attack→blocked` vs
  `block→stuff`) → pick one, keep it.
- ❌ **Clicking empty space or the wrong player** when the real player is hidden → skip, or
  use the 2D court if it's a serve.
- ❌ **Forgetting to back up** → export a file periodically.
- ❌ **Obsessing over perfect/good grades** → coarse (`kill`/`in`/`error`) is fine; keep moving.

---

## 11. Quality checklist

Before you consider a video "done," a good labeler can say yes to these:

- [ ] Ball is sampled across **many** rallies, with **`n`** on dead-ball moments.
- [ ] Actions are on the **right player** at roughly the **contact** frame.
- [ ] Outcomes are consistent (at least the coarse point/in/error buckets).
- [ ] Score is marked for **every** rally (needed for rotation), with a fixed left/right.
- [ ] Jersey numbers tagged where readable and useful.
- [ ] Uncertain plays were **skipped**, not guessed.
- [ ] Work is **exported** to a file as a backup.

---

## 12. Technical & privacy questions

**Q: Do I need internet?** No — fully offline once you have the folder.

**Q: Is my video uploaded anywhere?** No. It's read from your disk and never sent anywhere.

**Q: Which browsers work?** Chrome, Edge, Firefox, Safari (recent versions).

**Q: I lost my labels!** They auto-save per video in the browser — same browser + same video
brings them back. If you exported a `.json`, **Import** it. This is why we export backups.

**Q: How do I send my work to someone?** Use **⬇ Export video + labels** (one `.zip` with
both), or **labels .json** if they already have the video.

**Q: What's in the saved file?** A list of labels with a time `t`, a `type`
(`ball`/`ball_absent`/`ball_hard`/`action`/`score`), positions `x,y` (in the video's
original pixels), and for actions the `action`, `outcome`, and optional `player` (jersey).
See the README's "What's in the saved file" for the exact format.

**Q: How many labels do you need from me?** There's no fixed quota — **quality and variety**
matter more than raw count. Steady, careful labeling across varied footage is exactly right.
If a coordinator gave you a target, follow that; otherwise, label well and consistently.

---

*Questions this FAQ didn't answer? Ask your labeling coordinator — and if it's a recurring
one, it belongs in here.*
