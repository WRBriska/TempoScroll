# Tempo Scroll (Work in progress)

A beat-synced PDF score reader for musicians who want to keep their eyes on the music, not the scroll bar.

Open a PDF sheet music file, set your tempo, and Tempo Scroll holds the current system steady — then jumps to the next line exactly when the bar ends. No scrambling to find your place mid-phrase.


---

## The problem it solves

When you practice from a PDF on a laptop or tablet, you either scroll manually (uses a hand) or let the page auto-scroll continuously (your eye chases a moving target). Neither is great when you're trying to sight-read or build up speed.

Tempo Scroll takes a different approach: the page stays completely still while you play through a system, then snaps to the next line right on the downbeat. Your eyes can stay in one place the entire time.

---

## Features

- **Beat-synced line jumps** — advances one system per N bars, locked to your BPM, so the jump lands on the barline
- **Metronome** — optional audible click with a visual pip display; downbeats are visually distinct
- **Tap tempo** — tap any key or button in rhythm to set the BPM by feel
- **Italian tempo markings** — pick *Allegro*, *Andante*, *Presto*, etc. from a dropdown; it sets the BPM automatically, and the label updates as you adjust the number
- **Count-in** — 0, 1, or 2 bar count-in before the score starts scrolling
- **Configurable layout** — set beats per bar (2–6), bars per line (1–8), and line height (pixel offset per jump) to match your score's layout
- **Tabs & score mode** — doubles the jump distance for scores that pair a tab staff with a notation staff
- **Draggable guide line** — a horizontal reference line you can position where your eyes naturally rest
- **Zoom** — render the PDF at 50%–300% to suit your screen size
- **TEST button** — preview exactly one jump so you can dial in the line height before playing
- **Elapsed timer and beat countdown** — bottom bar shows time elapsed and beats until the next jump
- **Drag-and-drop** — drop a PDF straight onto the window

---

## Usage

1. Open `index.html` in any modern browser — no install, no server needed
2. Drop a PDF score onto the window, or click **Open score**
3. Set your tempo (type a BPM, tap the TAP button, or pick a tempo marking)
4. Configure beats per bar and bars per line to match the score
5. Adjust the line height slider until **TEST ▼** jumps exactly one system
6. Press **Space** (or the play button) to start

---

## Configuration reference

| Control | What it does |
|---|---|
| BPM | Beats per minute, 20–300 |
| TAP | Sets BPM from the average interval of your last 7 taps |
| Tempo mark | Italian marking dropdown; synced two-way with the BPM field |
| Beats / bar | Time signature numerator (2, 3, 4, 5, or 6) |
| Bars / line | How many bars fit on one system in your score |
| Line height | Pixel distance to scroll per jump — use TEST ▼ to calibrate |
| Count-in | Silent bars before scrolling begins (0, 1, or 2) |
| Metronome | Audible click on every beat; downbeat is accented |
| Tabs & score | For guitar scores with paired tab + notation staves |
| Guide line | Draggable horizontal marker; drag the ▲▼ grip to reposition |
| Zoom | Scale the PDF render from 50% to 300% |

---

## How it works

The scroll engine runs on `requestAnimationFrame` and accumulates fractional beats using the system clock rather than `setInterval`, which avoids drift over long sessions. Each beat fires an `onBeat` callback that updates the pip display, triggers the metronome click via the Web Audio API, and — when the beat count hits a bar boundary — increments the scroll target. A separate easing loop interpolates `scrollTop` toward the target with an exponential decay (`1 - e^(-dt/0.10)`), giving a quick glide that settles immediately rather than bouncing.

PDF rendering uses [PDF.js](https://mozilla.github.io/pdf.js/) at device pixel ratio for sharp output on retina displays. Re-renders on zoom preserve scroll position as a fractional ratio of total scrollable height.

Everything runs client-side. No score data is ever uploaded anywhere.

---

## Browser support

Works in any browser with Web Audio API and PDF.js support — Chrome, Firefox, Safari, and Edge. No build step, no dependencies beyond the PDF.js CDN script.

