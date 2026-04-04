## VCA Module
Voltage Controlled Amplifier module built as part of a modular analog synthesizer project.
Uses a differential transistor pair with TL072 op amps to achieve linear CV-controlled gain.

## Overview
```
- The VCA controls the amplitude of an audio signal using a control voltage (CV).
    - Higher CV → higher output amplitude (louder signal)
    - Lower CV → lower output amplitude (quieter signal)
- The most common use is connecting a sequencer's CV output to shape the VCO's audio into distinct notes.
```

## Signal Flow Diagram
```
VCO → [Audio IN] → U1.1 → Q1 ──┐
                                 ├──> U1.2 → [Audio OUT]
Sequencer → [CV IN] → Q2 ───────┘
```

## Power
- Supply: +9V single rail
- GND reference shared with VCO and sequencer modules

## Circuit Architecture
```
Stage 1 — Input buffer (U1.1)
- TL072 op-amp configured as a unity gain buffer - Isolates the input audio source from the rest of the circuit
- Prevents loading effects and preserves signal integrity

Stage 2 — Differential pair (Q1 + Q2)
- Two BC548C NPN transistors in a differential configuration.
    - Q1 → carries the audio signal
    - Q2 → carries the control voltage (CV) + bias offset
- Shared 10kΩ tail resistor + 10k trimpot to GND for transistor matching.
- Each collector has a 20kΩ load resistor to VCC.

Stage 3 — Subtractor (U1.2)
- TL072 op amp wired as a difference amplifier.
- Subtracts the two collector outputs to cancel noise, DC offset, and temperature drift.
- All resistors in this stage are matched 100kΩ.

Stage 4 — Output
- 1kΩ series resistor protects against short circuits at the output jack.
```

## Notes
```
- Proper transistor matching improves linearity and reduces distortion
- The trimpot allows fine adjustment of symmetry in the differential pair
- TL072 is chosen for:
  - Low noise
  - High input impedance
  - Good performance in audio applications
```

## Known Limitations
- Single supply (+9V) — op amps use a virtual ground reference
- No envelope generator included — CV comes from sequencer directly
- Breadboard version may show noise; PCB version is significantly cleaner
