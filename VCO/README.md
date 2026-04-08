# DIY Synthesizer VCO — Exponential Voltage Controlled Oscillator

A 1V/octave analogue **Voltage Controlled Oscillator (VCO)** module, based on the design from 
the [DIY Synth Series Part 1](https://www.allaboutcircuits.com/projects/diy-synth-series-exponential-vco/). 
Simulated in LTspice, producing simultaneous **triangle** and **square** wave outputs.

---

## Overview

This is the heart of an analogue synthesizer. It takes in control voltages and generates raw 
waveforms that can be further processed by filters and other modules.

The oscillator is **1V/octave**, meaning every 1V increase on the input raises the output 
frequency by one octave (doubles the frequency). This matches human logarithmic hearing - the 
same relationship found on a piano keyboard where A4 = 440Hz, A5 = 880Hz, A6 = 1760Hz, and so on.

---

## How It Works

The circuit has two major sections:

### Section 1 — Exponential Converter (U1, Q1, Q2)

A keyboard or controller outputs a **linear** voltage (e.g. 0–5V across 5 octaves). But because 
human hearing is logarithmic, the VCO needs an **exponential** response — the output frequency 
must *double* for every 1V increase. A BJT transistor's base-emitter to collector-current 
relationship is inherently exponential:

```
Ic = Is * (e^(q*Vbe / kT) - 1)
```

- **U1** sums the three CV inputs (KEY, TUNE, LFO) and scales 1V in → −18mV out (inverting)
        U1 scales the input voltage down to a small signal (~tens of millivolts per volt)
        required to drive the exponential converter correctly.
- **Q1 & Q2** (BC548, matched hFE) form a differential pair that performs the exponential V-to-I conversion
- **Q1 and Q2 must have closely matched hFE values (within 10 of each other)** and must be 
    thermally bonded together (e.g. hot-glued) to track temperature changes identically

### Section 2 — VCO Core (U2, U3, U4, Q3)

The core oscillator has four sub-sections:

| Sub-section | Component | Function |
|-------------|-----------|----------|
| Integrator | U3 (LM358), C1 | Ramps the output up or down to produce the triangle wave |
| Schmitt Trigger | U2 (LM358) | Compares integrator output against thresholds; switches between 0V and 5V |
| Reset Circuit | Q3 (BC548) | Turns on/off based on Schmitt output to reverse the integrator's ramp direction |
| Output Buffers | U4 (LM358) | Buffers triangle and square outputs |

**Oscillation cycle:**
1. Q3 is ON → integrator output rises
2. Integrator crosses upper threshold → Schmitt trigger switches to 0V
3. Q3 turns OFF → integrator output begins to fall
4. Integrator crosses lower threshold → Schmitt trigger switches to 5V
5. Q3 turns ON → repeat from step 1

The **triangle wave** is taken from the integrator output. The **square wave** is taken 
from the Schmitt trigger output.

---

## Inputs & Outputs

| Signal | Description |
|--------|-------------|
| **KEY** | 1V/octave pitch CV from keyboard or controller (0–5V = 5 octaves) |
| **TUNE** | Manual tuning offset — small voltage trim for frequency adjustment |
| **LFO** | Low-frequency oscillation input for effects (vibrato, sirens, arpeggio) |
| **TRIANGLE** | Triangle wave output |
| **SQUARE** | Square wave output |

---

## Materials

| Component | Value / Part | Qty | Reference |
|-----------|-------------|-----|-----------|
| Op-Amp | LM358 | 4 | U1, U2, U3, U4 |
| NPN Transistor (matched hFE) | BC548 | 2 | Q1, Q2 |
| NPN Transistor | BC548 | 1 | Q3 |
| Resistor | 1 MΩ | 1 | R2 (R6 in sim) |
| Resistor | 100K | 8 | R3, R4, R5, R8, R12, R14, R15 |
| Resistor | 56K | 4 | R9, R10, R11, R13 |
| Resistor | 22K | 1 | R1 |
| Resistor | 1K | 1 | R6 (R5 in sim) |
| Capacitor (timing) | 1 nF | 1 | C1 |
| Capacitor (timing) | ~4 nF (adjust as needed) | 1 | C2 (3.8863 nF in sim) |
| Capacitor (decoupling) | 100 nF | 6 | Near each IC power pin |
| Potentiometer | 10K linear | 2 | RV1 (tune), RV2 |
| Potentiometer | 100K | 1 | For KEY input testing |

> **Note:** Q1 and Q2 must have hFE values within 10 of each other. Buy a bag of ~100 BC548s
    and measure with a multimeter that reads hFE, then select a matched pair.

---

## Power Supply

| Rail | Voltage | Notes |
|------|---------|-------|
| VCC | +5V (regulated) | Sets VCO saturation points and output frequency range |
| VEE | −4V to −5V | Used only in the exponential converter stage |
| VREF | +2.5V | Mid-rail reference for comparator symmetry |

A dual supply is required (+5V / 0V / −5V). Options:
- Two batteries in series with the middle tap as ground

Only VCC needs to be regulated to 5V. The negative rail does not need regulation during normal 
operation. The negative rail is less critical for accuracy than the positive rail, but large 
variations can still affect stability and tuning.

---

## LTspice Simulation

```spice
.tran 0 1000ms 0 10us startup uic
```

| Source | Value | Purpose |
|--------|-------|---------|
| V1 (VCC) | +5V | Positive supply |
| V2 (VEE) | −4V | Negative supply |
| V3 (TUNE) | 2.833V DC | Tune CV — sets base pitch |
| V4 (KEY) | PULSE(0 5 0 1ms 0 0 0) | Simulates a key press (0→5V pulse) |
| V6 (VREF) | 2.5V | Comparator reference |

Probe the `TRIANGLE` and `SQUARE` nets to view waveforms. Adjust C2 to shift the frequency 
range up or down.

---

## Construction Tips

1. **Breadboard first** — verify each stage works before committing to stripboard or PCB
2. **Build and test one section at a time** — exponential converter first, then the VCO core
3. **Match Q1 & Q2 hFE** — values must be within 10 of each other
4. **Thermally bond Q1 & Q2** — hot-glue them together so temperature drift is identical on both
5. **Decouple power rails** — place a 100nF capacitor near each IC's power pins
6. **Touching Q1 or Q2** with a finger will change the output frequency — an intentional and fun effect!

---

## Reference

> Robin Mitchell, *"DIY Synth Series Part 1 — The Exponential VCO"*, All About Circuits, August 8, 2016.
> https://www.allaboutcircuits.com/projects/diy-synth-series-exponential-vco/

---