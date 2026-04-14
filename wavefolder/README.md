## Wavefolder

A simple analog wavefolder module for modular synthesizer project, based on the CGS52 Simple Wave
Folder (https://www.elby-designs.com/webtek/cgs/cgs/cgs52/cgs52_folder.html) design by Ken Stone.

## What It Does

```
A wavefolder adds harmonic complexity to a signal by "folding" the waveform back on itself whenever it exceeds a threshold, rather than clipping it flat. The effect is similar in character to the Serge Wave Multiplier — feeding a plain sine or triangle wave in produces a harmonically rich output that can be swept for filter-like timbral changes.
It must be fed a triangle or sine wave to function correctly. Sawtooth and square waves won't fold properly because the fold points require a continuously varying input.
The module has two outputs:
    - Folded output — the waveshaped signal, buffered and amplified back to synthesizer levels.
    - Pulse output — a comparator output derived from the transistor stage, producing a pulse
        pattern that shifts and changes width as the input level or DC offset is modulated.
```

## Power

- VCC: +9V
- VEE: −9V


## References

Ken Stone, Simple Wave Folder (CGS52), 2004 — 
https://www.elby-designs.com/webtek/cgs/cgs/cgs52/cgs52_folder.html