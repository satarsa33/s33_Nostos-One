# S33 - Nostos One

A **polyphonic modular synthesizer** that runs in the browser, made to be played from a **MIDI keyboard**.
It has two faces on the same engine:

- the **Board**: modules (oscillators, filters, envelopes, effects…) can be dragged around, connected with cables and heard immediately;
- the **Panel**: a clean front panel with knobs, drawbars, arpeggiator and monitors, acting on the very same modules.

It is a **PWA** in a single HTML file, with no dependencies or build step: open it, install it as an app, and it keeps working offline.

> A personal project of the S33 family, born as an experiment in learning and having fun. The name comes from *nostos*, the "return home" that gives us the word nostalgia: it offers sounds from the past. No warranty: see [Known limits](#known-limits).

---

## Quick start

### Try it locally

Service workers and MIDI require `http://localhost` or `https://`, so don't open the file by double-clicking: serve the folder.

```bash
cd s33-nostos-one
python3 -m http.server 8000
# then open http://localhost:8000 in Chrome
```

### Publish on GitHub Pages

1. Create a repository and upload **the contents** of this folder (`index.html`, `sw.js`, `manifest.webmanifest`, `icons/`, `README.md`).
2. *Settings → Pages → Deploy from a branch →* `main` / `(root)`.
3. Open `https://<user>.github.io/<repository>/`.

All paths are relative, so it also works from a subfolder. Note that GitHub is case-sensitive: the folder must be called `icons`, not `Icons`.

### Install as an app

In **Chrome** (recommended) open the address and choose *Install S33 - Nostos One* from the address bar (or the ⋮ menu). On the Mac it appears as a standalone application with its own icon.

### Playing

1. Connect the MIDI keyboard **before** opening the page (or reload after connecting it).
2. Press **Start audio** (the browser requires a user gesture) and allow MIDI access.
3. Pick a preset from the menu and play.

Without a MIDI keyboard you can use the on-screen keyboard or the computer keys: `A W S E D F T G Y H U J K` map to one octave from middle C.

| | Chrome / Edge | Safari | Firefox |
|---|---|---|---|
| Audio and interface | yes | yes | yes |
| MIDI input (Web MIDI) | **yes** | no | limited |

---

## What's inside

### Modules

Keyboard (note and velocity), Oscillator (sine, triangle, saw, square; octave, semitone, fine), **Sync Osc** (hard sync with modulatable ratio), **FM** (ratio and index, with an input to modulate the index), Noise, LFO, 3-input Mixer, **Ladder Filter** (24 dB/oct, resonant, with saturation), **High-pass**, ADSR, VCA, Mod Wheel, Delay (also **tempo-synced**), Reverb, Chorus, **Drive** (soft tanh saturation with 4x oversampling, tone control and automatic level compensation; master only, best placed before chorus and delay), Output.

Modules to the left of the dashed line are **per voice** (copied up to 8 times); those on the right are **master** (once). A module's position decides which zone it belongs to.

### Polyphony

1, 4 or 8 voices with voice allocation and oldest-voice stealing, glide, pitch bend and mod wheel (CC 1).

### Board

- drag from one circle to another to connect; click a module or a cable to edit it;
- double-click a selected cable to delete it;
- cables with an amount (for instance envelope → filter) get their own knob in the module's panel.

### Panel

The panel adapts to the preset with a dedicated **model** (Minimoog, FM, Organ, Percussion, Pad, Prophet, Farfisa, Hammond, Rhodes, Piano) or in **Automatic** mode, which shows the knobs of every module. It includes:

- an **Arpeggiator** with up / down / up-down / random / order modes, triplet divisions, 1–4 octaves, gate, swing, latch and BPM tempo;
- **Touch Dynamics** from MIDI velocity (see below);
- a **preset** monitor, **oscilloscope** and **spectrum**;
- an on-screen keyboard with pitch and mod wheels, and an octave display.

### Touch Dynamics

MIDI velocity drives **volume** and **brightness** (it opens filters and FM index). You can:

- switch it on and off for the whole synth (**DYNAMICS**);
- exclude it for a single preset (**THIS PRESET**): organs and string machines start without it, like the original instruments;
- set the **curve** (soft / normal / hard), **volume** and **brightness**.

On the Board the Keyboard module also has a `velocity` output to wire to any parameter.

### Skins

From the **Skin** menu: Automatic (follows the system light/dark setting), Moog Ivory, Prophet Black, Graphite, Neon, Blueprint. The raised look of the knobs changes with the skin.

### Presets (46)

Presets are grouped by instrument family. Inside each group they are ordered from the most "natural" to the most unusual.

| Group | Presets |
|---|---|
| **Basics** | Init Patch |
| **Pianos** | Grand Piano, Upright Piano, Felt Piano, Bright Pop Piano, Pedal Piano (Ambient) |
| **Electric Pianos** | Fender Rhodes – Mark I, Fender Rhodes – Suitcase (Tremolo), FM Electric Piano |
| **Organs** | Drawbar Organ, Hammond C3 – 888000000 (Slow Leslie), Hammond C3 – 888000000 (Fast Leslie), Farfisa Compact – Flute, Oboe, Flute 4′, Farfisa Compact – Booster |
| **Strings & Pads** | Strings 70s, ARP String Ensemble – Violins, ARP String Ensemble – Cellos & Violas, Warm Pad, Air Pad, Abyss Pad, Supersaw |
| **Leads & Brass** | Solo Lead, Brass, **Shine On - P I**, **Shine On - P III**, Prophet-5 – One of My Turns (intro), Prophet-5 – Sync Lead (Run Like Hell) |
| **Bass** | Moog Bass |
| **Plucks & Bells** | Pizzicato, FM Bell |
| **Arpeggiator** | Arp – Dry Pluck, Digital Staccato, Sequencer Bass, Soft Flute, Velvet, Kalimba, FM Bells, FM Metal, Steel (Sync), Nocturne |
| **Drums & Percussion** | Synth Kick, Snare, Hi-hat – Closed, Hi-hat – Open, Cymbal, Noise Burst |

Presets that evoke real tracks or instruments are **reconstructions made by ear and from descriptions found online**, not copies: treat them as a starting point to refine (see [Preset sources](#preset-sources)).

Your own presets appear in a **My Presets** group at the end of the menu.

### Save, export, import

- **Save as…** keeps your preset in the browser;
- **Export** / **Export mine** downloads one or more presets as a `.json` file;
- **Import** loads a file, which is checked and normalized (unknown modules and ports, out-of-range values, loops and invalid cables are rejected or clamped).

The file contains data only, never code. Files saved by earlier (Italian-language) versions still import correctly.

---

## Folder structure

```
index.html               the whole application (HTML, CSS and JavaScript)
manifest.webmanifest     app name, icons and colors
sw.js                    service worker: offline operation
icons/                   PNG icons (192, 512, maskable, Apple touch, favicon); icon.svg is only the source and is not needed for the app to run
README.md                this file
```

### Updating the app

The service worker answers from the cache and updates silently when online: after publishing a change, the new version appears **the second time you open it**. To force a refresh, change `VERSIONE` in `sw.js`.

### App identifier

Chrome identifies a PWA by the manifest `id` field, resolved **against the site's origin** (not the folder). If several apps live under the same `user.github.io`, each must have a different `id` (here `s33-nostos-one`); with `"id": "./"` they would all count as the same app and Chrome would offer "Open in app" instead of "Install".

### Data saved in the browser (`localStorage`)

`synth-lavagna-v3` (current patch), `synth-user-presets`, `synth-mon-w` (monitor width), `synth-skin` (skin), `synth-vel` (dynamics).

---

## Known limits

- **It has not been judged by ear by its author**: structure, import, offline mode and audio behavior were checked with automated tests, but the sounds need to be judged and tweaked by ear.
- No real phaser, spring reverb or Leslie (the Hammond presets approximate the Leslie with tremolo and chorus); the string machines lack the original's phaser.
- The pianos and the Rhodes are **FM synthesis**, not samples: they sound like synthetic pianos.
- MIDI inputs handled: notes with velocity, pitch bend, CC 1. **Not** handled: sustain pedal, aftertouch, program change.
- The mixer has 3 inputs (chain two mixers if you need more).
- The ladder filter uses an AudioWorklet; if it is unavailable the browser falls back to a simpler filter.
- MIDI works only in Chrome and Edge, and only over HTTPS or on `localhost`.

## Ideas for the future

- desktop package with **Electron** or **Tauri**;
- sustain pedal and aftertouch;
- more modules (phaser, waveshaper, step sequencer).

---

## Preset sources

The settings of presets inspired by real instruments come from public descriptions, not official data:

- [Pink Floyd – One of My Turns, intro (matrixsynth)](https://www.matrixsynth.com/2008/09/pink-floyd-one-of-my-turns-intro.html)
- [Sound On Sound – re-creating the sound of old string synths](https://www.soundonsound.com/sound-advice/q-how-do-re-create-sound-those-old-string-synths)
- [Farfisa Compact (combo-organ.com)](https://www.combo-organ.com/Farfisa/Compact/)
- [Hammond organ (Wikipedia)](https://en.wikipedia.org/wiki/Hammond_organ)
- [The synths of Pink Floyd (gearnews)](https://gearnews.com/synths-of-pink-floyd-synth/)

The title typeface is **Oswald** (SIL Open Font License), embedded in the file.

Moog, Minimoog, Sequential, Prophet-5, ARP, Solina, Hammond, Farfisa, Fender, Rhodes and the other names mentioned belong to their respective owners and are used only to indicate the sonic character a preset is inspired by. No affiliation with the brands mentioned.

## Use

Personal-use project. If you publish it, choose the license you want to add.
