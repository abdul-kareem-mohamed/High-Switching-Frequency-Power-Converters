
ZVS GaN buck, 1MHz, 48V --> 12V, 100W

### Operating point

* Duty cycle: D = Vout/Vin = 12/48 = 0.25. This is the ideal value for continuous conduction.
* Load current: Iout = P/V = 100/12 = 8.33 A.
* Load resistor: Rload = V/I = 12/8.33 = 1.44 Ω.

### Inductor (3.3 µH)

The inductor ripple current is ΔI = Vout(1−D)/(L·fsw). I targeted about 30–40% ripple, which is a common compromise:

* Smaller L means more ripple, more RMS current and more core and AC loss.
* Larger L means a bigger, slower inductor.

For ΔI = 3 A: L = 12 × 0.75 / (1 MHz × 3 A) = 3.0 µH. I rounded up to the standard value 3.3 µH, which gives ΔI = 2.73 A.  

The peak inductor current is 8.33 + 2.73/2 ≈ 9.7 A, so a real inductor needs a saturation rating of about 11–12 A or more. The converter stays in continuous conduction down to a load of about 1.4 A.  

### Output capacitor (22 µF, 3 mΩ ESR)

The capacitive ripple is ΔV = ΔI/(8·fsw·C).

To get 50 mV: C = 2.73/(8 × 1 MHz × 0.05) ≈ 6.8 µF.
I chose 22 µF for margin, which gives ≈ 15 mV.
The ESR adds ΔI × ESR = 2.73 × 3 mΩ ≈ 8 mV.

The total output ripple is therefore in the range of 15–25 mV. Ceramic capacitors lose a lot of capacitance under DC bias, so a real build needs more than 22 µF of physical capacitance.  

### Input capacitor (6.8 µF)

The input ripple is ΔVin = Iout·D(1−D)/(fsw·C) = 8.33 × 0.1875 / (1 MHz × 6.8 µF) ≈ 0.23 V.

The input capacitor must also carry the pulsed current, with an RMS value of Iout·√(D(1−D)) ≈ 3.6 A. A real design needs several ceramic capacitors in parallel to handle this.

### Dead time (10 ns)

GaN switches in a few ns, so a short dead time is enough.

If the dead time is too short, both switches conduct at once (shoot-through).
If it is too long, the reverse-conduction diode loses power: 1.9 V × 8.33 A × 20 ns × 1 MHz ≈ 0.3 W.

I picked 10 ns as a compromise.  

### Gate pulse timing

* High-side on-time: D × T = 0.253 × 1000 ns = 253 ns. The extra 3 ns over the ideal 250 ns compensates for the small voltage losses.
* Low-side delay (td = 263 ns): the high-side on-time plus the 10 ns dead time.
* Low-side width (tw = 727 ns): 1000 − 253 − 2 × 10 = 727 ns, which leaves a dead time at both edges.
* Gate level 0 to 1 V: this is just a logic signal for the ideal switch (threshold 0.5 V). A real GaN gate is driven at about 5 V.

<img width="1872" height="853" alt="image" src="https://github.com/user-attachments/assets/1c3b886e-26b7-4900-af28-dc317b029f86" /> <br>
Figure 1: output voltage waveform

Vout rings up to about 20 V, then settles near 12 V. This is the startup transient. With no soft start or feedback, the L-C filter is underdamped, so it overshoots and rings.  

### Why the output overshoots to ~20 V

At t = 0 the output capacitor is empty and the inductor has no current. The converter suddenly applies a 25% duty cycle with no feedback to limit it. The L and C then form a resonant circuit that gets "kicked", like a spring with little damping, so the voltage overshoots its final value and rings before settling.

Q describes how strongly a resonant circuit rings. The load resistor is what damps it, and for an L–C filter with a parallel load:

Q = R·√(C/L) = 1.44 × √(22 µF / 3.3 µH) ≈ 3.7

Q < 0.5 means no ringing (overdamped).
Q > 0.5 means ringing (underdamped). Higher Q means more ringing.

Equivalently, the damping ratio is ζ = 1/(2Q) ≈ 0.13, which is very light damping.  

For a second-order system the step overshoot is exp(−πζ/√(1−ζ²)) ≈ 65%. So 12 V × 1.65 ≈ 19.8 V, which matches the ~20 V peak in the plot (figure 1).

The ringing is about 18 kHz (roughly 55 µs per cycle). That matches the filter's natural frequency, 1/(2π√(3.3 µH × 22 µF)) ≈ 18.7 kHz, which is a good sign the model behaves as designed.  

### Why it takes ~300 µs to settle

The ringing envelope decays with a time constant of about 2RC = 2 × 1.44 × 22 µF ≈ 63 µs. Settling takes around 4 time constants, or about 250 µs, which is consistent with the plot (figure 1). It settles about 12.2 V.

<img width="1886" height="850" alt="image" src="https://github.com/user-attachments/assets/deed296e-086d-4a4f-8a11-9adb885c1655" /> <br>
Figure 2: output voltage ripple in steady state. Average of Vout = 12.2 V and peak-to-peak output voltage corresponds to 17.1 mV <br>  

<img width="1882" height="753" alt="image" src="https://github.com/user-attachments/assets/fa1442bd-bf71-4f63-b355-eb04eda5f2be" /> <br>
Figure 3: Inductor current ripple in steady state. Average of I(L1) = 8.3 A and peak-to-peak current ripple corresponds to 2.66 A <br>

<img width="1870" height="747" alt="image" src="https://github.com/user-attachments/assets/9bc6f226-d394-48d9-a441-5f70200c016a" /> <br>
Figure 4: Switch node shape. 25% duty cycle = 250ns and switching period = 1us. -1.9 V dip for 10ns as a result of dead time and reverse conduction voltage drop of the switch. <br>










