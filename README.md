# RACKUNIT — SB-12 Soundboard

A single-file browser soundboard with 14 synthesized sound effects, a hardware-fader volume control, and a stop-all kill switch. No audio files — every sound is generated live with the Web Audio API.

Demo Video : https://drive.google.com/file/d/1nzLz5MeZHq6NevD0rzAqmWzxQEmeMdDe/view?usp=sharing

## Quick start

1. Download `soundboard.html`.
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge).
3. Click a pad, or use your keyboard, to play a sound.

That's it — no build step, no server, no dependencies to install.

## Controls

| Pad | Sound   | Key |
|-----|---------|-----|
| 1   | Kick    | `1` |
| 2   | Snare   | `2` |
| 3   | Hi-Hat  | `3` |
| 4   | Clap    | `4` |
| 5   | Tom     | `5` |
| 6   | Cowbell | `6` |
| 7   | Rimshot | `7` |
| 8   | Laser   | `8` |
| 9   | Zap     | `9` |
| 10  | Blip    | `1` then `0` |
| 11  | Chime   | `1` then `1` |
| 12  | Boom    | `1` then `2` |
| 13  | Coin    | `1` then `3` |
| 14  | Whoosh  | `1` then `4` |

- **Pads 1–9** fire the instant you press the key.
- **Pads 10–14** are reached by pressing `1` and then the second digit within about a third of a second. If you press `1` and pause, it plays pad 1 on its own.
- **Volume fader** sets the master output level for every sound.
- **STOP ALL** immediately silences anything currently playing, including long decays like Boom and Chime.

## How it works

- A single `AudioContext` and master `GainNode` route every sound. The volume slider controls that gain node directly.
- Each sound is a small synthesis "recipe" built from oscillators (sine/square/triangle/sawtooth), filtered white noise, and gain envelopes — no samples are loaded or shipped.
- An `AnalyserNode` feeds the little oscilloscope readout in the display panel, so you can see the waveform of whatever just played.
- Every playing node is tracked in an array as it starts and removed when it ends naturally. **STOP ALL** walks that array and force-stops/disconnects anything still active.

## Customizing

All the logic lives in one file, `soundboard.html`:

- **Add a sound**: write a `playX()` function following the existing patterns (oscillator + gain envelope, or noise buffer + filter + gain envelope), then add an entry to the `PADS` array with a `key`, `label`, `glyph`, and your function.
- **Change the key bindings**: edit the `key` field on any `PADS` entry. Two-character keys (`'10'`–`'14'`) are handled automatically by the digit-buffer logic; anything you add beyond `14` would need its own prefix handling.
- **Restyle**: colors, fonts, and pad sizing are all defined as CSS custom properties and classes near the top of the `<style>` block.

## Browser support

Works anywhere the Web Audio API is available — all current versions of Chrome, Firefox, Safari, and Edge. Some browsers require a user gesture (a click or key press) before audio can play, which is why the first sound sometimes needs an extra click to "wake up" the audio context — this is handled automatically on the first pad hit.

## Accessibility

- All pads are real `<button>` elements with `aria-label`s, so they're reachable and readable via keyboard and screen reader.
- Respects `prefers-reduced-motion` for pad press animations.
