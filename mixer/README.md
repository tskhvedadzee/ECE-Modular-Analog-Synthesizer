## Audio Mixer Circuit

### Overview
This is a 3-channel analog audio mixer circuit designed and simulated in LTSpice. It combines 
three independent sine wave input signals at different frequencies into a mixed output using 
operational amplifiers and a diode stage.

Circuit Description
The circuit is divided into the following stages:
1. Input Stage
Three independent signal sources feed into the mixer.

Each input passes through a 100k potentiometer before reaching the summing stage. 
A 100k potentiometer (R1/R2) on the V1 channel allows input level control.


## Diode Clipping Stage (D1, D2)
Two diodes (D1, D2) form a clipping/limiting stage on the signal path. R12 (100k) feeds the 
signal into the diode network, which connects to the final amplifier stage.

## Output Nodes

R8 (1k) — Direct summed output (out)
R11 (1k) — Post-main-amplifier output
R15 (1k) — Final mixed output after diode and output amplifier stage