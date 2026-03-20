555 Timer + CD4017BE Circuit

## Overview
This project demonstrates a basic sequential LED chaser using a 555 timer IC and a CD4017BE decade counter. The 555 timer generates clock pulses, which advance the CD4017BE outputs in sequence.

### How the Circuit Works
1. The 555 timer outputs a square wave that acts as a clock for the CD4017BE.
2. The CD4017BE has 10 outputs, but this circuit uses only 5 outputs (Q0-Q4).
3. LEDs connected to these 5 outputs light up in sequence, creating a “running light” effect.
4. The sequence can be controlled to run 3-5 steps. A switch (3 position on-on-on switch) controls the reset pin (pin 15)
   - You can select whether the counter resets at step 3, 4, or 5.
   - This effectively limits the sequence to run only the first 3, 4, or 5 steps, depending on
     the switch position.
   - This allows a customizable limited sequence instead of counting through all outputs.
5. The speed of the sequence can be adjusted using the 555 timer’s resistor and capacitor values, or a potentiometer.

## Components Needed
- 555 Timer IC - configured in astable mode to generate clock pulses
- CD4017BE Decade Counter IC
- LEDs (x5–6) – one for each output, optional extra for testing 555-timer
- Resistors – appropriate values for LEDs (usually 220–470Ω)
- Capacitors – for 555 timer timing (e.g., 10nF–100µF depending on speed)
- Potentiometer – to adjust the clock speed of the 555 timer
- Switch – SPDT or rotary switch to select reset step (3, 4, or 5)
- Breadboard and jumper wires – for prototyping
- Power supply (5V–9V) – suitable for 555 timer and CD4017BE