# PO-33 K.O! Emulator

A web-based emulator of the PO-33 micro sampler, built as a single HTML file with embedded CSS and JavaScript. Deploying to the github pages.

## Architecture

**Single-file application**: All code is contained in `index.html` with three main sections:
- HTML structure: Device UI with LCD display, 16-pad grid, control buttons, and parameter knobs
- CSS styling: Retro device aesthetic with custom properties for colors and responsive grid layout
- JavaScript application: Complete audio engine, sequencer, and UI logic

**Core JavaScript modules**:
- Audio Context management
- State management: 16 sounds, 16 patterns, 16-step sequencer
- Sound synthesis engines: FM synth, drum synthesis, Karplus-Strong, additive synthesis
- Playback engine: Effects processing, looping, pitch manipulation
- Sequencer logic: 16-step pattern sequencing with real-time scheduling
- Microphone recording: MediaRecorder API integration
- UI event handlers: Pad controls, button holds, keyboard mapping

## Development Commands

**Local development**:
```bash
php -S localhost:8000
```

**No build process required** - open `index.html` directly in browser or serve via static server.

## Key Implementation Details

**Audio synthesis**: Uses Web Audio API with procedural sound generation. No external audio files - all sounds are synthesized mathematically using oscillators, envelopes, and filters.

**State management**: Centralized state object manages 16 sound slots, 16 patterns, and current UI mode. Each sound has parameters for pitch, volume, filter, and trim.

**Sequencer timing**: Uses `setTimeout` scheduling with lookahead buffer for precise timing. Sequencer runs at 16th notes based on current BPM.

**Recording flow**: Mic stream is cached after first permission request. MediaRecorder captures audio, decodes to AudioBuffer, and stores in sound slots.

**UI interaction model**: Sequential button interactions. Press a mode button to enter that mode (highlighted), then press a pad to perform the action. Press the same button again to exit the mode. Modes can also be controlled via keyboard shortcuts.

**Keyboard shortcuts**: See the Keyboard Shortcuts section below.

## Keyboard Shortcuts

### Pads (4x4 grid)
| Row 1 | Row 2 | Row 3 | Row 4 |
|-------|-------|-------|-------|
| 1 | Q | A | Z |
| 2 | W | S | X |
| 3 | E | D | C |
| 4 | R | F | V |

### Mode Buttons
| Key | Action |
|-----|--------|
| G | SOUND mode - select sound (1-16) |
| P | PATTERN mode - select pattern (1-16) |
| B | BPM mode - set BPM via knob A or pad presets |
| H | FX mode - select effect (1-16) |
| T | WRITE mode - toggle step sequencing |
| N | REC mode - arm/cancel/stop recording |

### Parameter Tabs
| Key | Action |
|-----|--------|
| I | TONE mode (pitch/volume) |
| O | FILTER mode (cutoff/resonance) |
| K | TRIM mode (start/length) |

### Transport
| Key | Action |
|-----|--------|
| Space | Play/Stop |
| Escape | Clear all modes, stop playback |

### Pattern Chaining
To create a pattern chain (e.g., 1-1-1-2-3-3):
1. Press **T** to enter WRITE mode
2. Press **P** to enter PATTERN mode
3. Tap pads to build the chain (displayed as `CHAIN: 1-1-1-2-3-3`)
4. Press **Space** to play - patterns will cycle through the chain

## Browser Compatibility

Requires modern browser with Web Audio API and MediaRecorder API support. Uses `webkitAudioContext` fallback for Safari.
