# RACKUNIT — SB-12 FX Module

A retro rack-mounted drum machine / soundboard, built as a single self-contained HTML page. Every sound is synthesized live in the browser with the Web Audio API — there are no audio files to load.

Demo Video : https://drive.google.com/file/d/1nzLz5MeZHq6NevD0rzAqmWzxQEmeMdDe/view?usp=sharing

## Quick start

## Features

- **14 synthesized channels** — kick, snare, hi-hat, clap, tom, cowbell, rimshot, laser, zap, blip, chime, boom, coin, and whoosh, each built from oscillators, filtered noise, or both.
- **Click or keyboard triggers** — every pad responds to a mouse click or its numbered key. Pads 1–9 fire instantly; pads 10–14 are triggered as a two-key sequence (press `1`, then `0`–`4` within ~350ms). Press `1` alone and let it time out to fire pad 1 by itself.
- **Live LED display** — shows the name of the currently playing sound with an amber glow, reverting to `STANDBY` after ~0.9s of silence.
- **Built-in oscilloscope** — a small teal waveform scope in the display, driven by an `AnalyserNode`, renders the live master output in real time.
- **Master volume fader** — a hardware-style slider (0–100) smoothly ramps the master gain.
- **Stop All** — instantly halts every currently playing sound and resets the display.
- **Tactile pad UI** — each pad depresses, glows, and lights an LED on hit, styled like a hardware rack unit (brushed-metal gradients, screws, inset shadows).
- **No external assets** — only a Google Fonts import (Oswald / Inter / JetBrains Mono) is fetched; all audio is generated with `OscillatorNode`, `AudioBuffer` noise, `BiquadFilterNode`, and `GainNode` envelopes.

## How it works

| Piece | Implementation |
|---|---|
| Percussive tones (kick, tom, boom) | Sine oscillators with exponential frequency and gain ramps |
| Noise-based hits (snare, hat, clap, zap) | White noise buffers shaped with high/band-pass filters and short gain envelopes |
| Melodic hits (chime, coin) | Stacked/sequenced oscillators with staggered gain envelopes |
| Sweeps (laser, whoosh) | Oscillator/noise with sweeping filter or frequency ramps |
| Signal chain | Each voice → its own gain envelope → shared `masterGain` → `AnalyserNode` → `AudioContext.destination` |
| Cleanup | Every playing node is tracked in an `activeNodes` array and auto-removed via `onended`, so **Stop All** can cut every voice instantly |

## Usage

1. Open the HTML file in any modern browser (Chrome, Firefox, Safari, Edge).
2. Click a pad, or press its key, to trigger a sound.
3. Adjust the **VOL** fader to change master volume.
4. Hit **STOP ALL** to silence everything immediately.

No build step, server, or dependencies beyond a browser with Web Audio API support (all evergreen browsers).

## Keyboard map

| Key(s) | Pad |
|---|---|
| `1` | Kick |
| `2` | Snare |
| `3` | Hi-Hat |
| `4` | Clap |
| `5` | Tom |
| `6` | Cowbell |
| `7` | Rimshot |
| `8` | Laser |
| `9` | Zap |
| `1`→`0` | Blip |
| `1`→`1` | Chime |
| `1`→`2` | Boom |
| `1`→`3` | Coin |
| `1`→`4` | Whoosh |

## Browser support notes

- All pads are real `<button>` elements with `aria-label`s, so they're reachable and readable via keyboard and screen reader.
- Respects `prefers-reduced-motion` for pad press animations.
