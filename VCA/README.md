# Differential Pair VCA
A discrete voltage-controlled amplifier based on a matched NPN differential pair (Q1/Q2), 
suitable for audio and modular synthesizer applications. Operates on ±15V dual supply.

---

## Overview
This circuit uses the classic long-tailed pair topology: two matched transistors share a common 
emitter tail whose current is set by the CV path. Because gain is proportional to tail current, 
the audio signal at Q1's base is amplified by an amount determined by the control voltage. 
A differential output amplifier (U3) rejects common-mode noise and provides a single-ended output.

---

## Schematic Summary

| Block | Components | Function |
|---|---|---|
| Power supply | V1 +15V, V2 −15V | Dual-rail supply |
| Audio input attenuation | R1 (200kΩ) | Divides input down to ~50 mV at base |
| Gain cell | Q1, Q2 (2N3904 / BCM56DS) | Differential pair; gain ∝ tail current |
| Collector loads | R3, R4 (47kΩ 0.1%) | Matched precision loads for good CMRR |
| Emitter resistors | R2, R5 (1kΩ) | Stability and distortion control |
| CV processor | V4, R10 (100kΩ), U2, R11 (100kΩ), R12 (47kΩ) | Converts CV to tail current |
| Output diff amp | R6, R7 (15kΩ), U3, R8, R9 (100kΩ) | Differential → single-ended, gain ≈ 6.7× |

---

## Component List (BOM)

| Ref | Value | Notes |
|---|---|---|
| Q1, Q2 | 2N3904 | Use BCM56DS dual matched pair for best performance |
| U2, U3 | Op-amp (e.g. TL072, NE5532) | U2: CV integrator / U3: output diff amp |
| R1 | 200kΩ | Audio input attenuator |
| R2, R5 | 1kΩ | Emitter degeneration |
| R3, R4 | 47kΩ **0.1%** | Matched collector loads — precision required |
| R6, R7 | 15kΩ | Diff amp input resistors |
| R8, R9 | 100kΩ | Diff amp feedback resistors |
| R10, R11 | 100kΩ | CV op-amp gain network |
| R12 | 47kΩ | CV to emitter tail |
| V4 (CV in) | 0–4V DC | Nominal control voltage range |

---

## How It Works

### 1. Audio path

The audio source feeds Q1's base through R1 (200kΩ), which attenuates a large input signal 
(up to 20 V P-P) down to the ~50 mV range safe for the transistor's base. Q1 amplifies the 
signal; Q2 acts as the mirror half of the differential pair.

### 2. CV path

V4 (the control voltage) is processed by U2 in an inverting integrator configuration 
(R10/R11 = unity gain). The output drives the shared emitter node via R12 (47kΩ). 
Higher CV → higher tail current → higher transconductance gm of Q1/Q2 → more gain.

At zero CV the tail current is minimal and the circuit is nearly silent. Gain rises as 
CV increases toward +4V.

### 3. Output stage

U3 is a classic differential amplifier:

```
Av = R8 / R6 = 100k / 15k ≈ 6.67 (≈ +16.5 dB)
```

R6/R7 and R8/R9 must be matched in ratio (they are here). This stage subtracts the two 
collector voltages, rejecting any common-mode noise picked up in the gain cell, and delivers a 
clean single-ended output.

---

## Gain Law (approximate)

The incremental transconductance of a BJT is:

```
gm = Ic / VT     (VT ≈ 26 mV at room temperature)
```

Where Ic is the tail current divided between Q1 and Q2. Because gm is proportional to Ic, 
and Ic is proportional to CV, the gain tracks CV linearly in this small-signal regime. 
For an exponential (dB-linear) response, replace R12/R10/R11 with an exponential converter cell.

---

## Performance Notes

**Why matched transistors?**
R3 and R4 are 0.1% tolerance for a reason. The output is the *difference* of two collector 
voltages. Any mismatch in transistor Vbe or in collector resistor values appears as a DC 
offset and degrades CMRR. The BCM56DS is a monolithic dual matched NPN: both transistors share 
the same die, guaranteeing <1 mV Vbe matching and thermally tracking behavior.

**Why ±15V?**
The differential pair and op-amps need enough headroom for the audio signal swing and the DC 
operating point. ±15V is standard Eurorack / pro-audio rail voltage. The circuit will work on 
±12V with slightly reduced headroom.

**Distortion**
R2 and R5 (1kΩ emitter degeneration) linearise the gain cell at the cost of some gain. Reducing 
them to 470Ω increases gain but raises THD. Removing them entirely makes the gain cell strongly 
nonlinear — only do this intentionally (e.g. for waveshaping effects).

---

## Simulation

The schematic includes an LTspice simulation setup:

```
.tran 0 0.1 0 1
V3 (audio): SINE(0 10 300 0 0 0 30)   — 10V amplitude, 300 Hz, 30° phase offset
V4 (CV):    4V DC
```
Probe `Audio_Out` to observe the amplified output. Sweep V4 from 0 to 4V to observe gain 
variation.

---

## Modifications

| Goal | Change |
|---|---|
| More gain | Reduce R6/R7 (e.g. 10kΩ → Av ≈ 10×) |
| Less gain | Increase R6/R7 or reduce R8/R9 |
| Exponential (dB-linear) CV response | Add a transistor exponential converter before U2 |
| Lower noise | Substitute NE5532 for TL072 in U3; use lower-noise matched pair |
| Stereo VCA | Mirror the entire Q1/Q2/U3 section; share the CV path |

---