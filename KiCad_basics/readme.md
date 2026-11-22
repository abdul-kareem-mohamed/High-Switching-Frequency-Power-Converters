
<img width="1238" height="728" alt="image" src="https://github.com/user-attachments/assets/dbb9cb67-1f30-4d41-aac3-75f178f10b3a" /> <br>
Figure 1: Schematic in Kicad

Assumptions:
* VCE ≈ 0, β ≈ 10 <br>
* VE < VB > VC for saturation mode.

Simulations:
* VC = 57.1 mV 
* VB = 0.808 V
* IC = 33.0 mA
* IB = 4.19 mA

VE < VB > VC equals 0 < 0.808 > 0.0571 - it is indeed in saturation mode.
VCE = VC – VE = 57.1 – 0 = 57.1 mV and β = IC / IB = 33.0 / 4.19 = 7.86. The VCE and β are indeed close to our assumptions. 

For the load, the assumed IC current would be VCC / R2 = 5 / 150 = 33.3 mA and we got a value of 33.0 mA in our simulation.

And finally, the assumed IB transistor base current is IC / 10 = 33.3 / 10 = 3.33 mA and we got a value of 4.19 mA.  

Thus, simulations match our assumptions.  

<img width="1821" height="657" alt="image" src="https://github.com/user-attachments/assets/f7d2d643-259c-47e8-961c-cc21dd9e313d" /> <br>
Figure 2: Voltage waveform for Vin = 5 v and Vcc = 5 V <br>

<img width="1363" height="646" alt="image" src="https://github.com/user-attachments/assets/9c229225-e9b0-49b1-a9b5-2352759da1f0" /> <br>
Figure 3: Current waveform for Vin = 5 v and Vcc = 5 V <br>

Now, the input voltage is given as sinusoidal. <br>

<img width="1277" height="793" alt="image" src="https://github.com/user-attachments/assets/1e91c4b3-76d4-48a6-8631-434fdf243320" /> <br>
Figure 4: Schematic with sinusoidal voltage input <br>

<img width="1821" height="676" alt="image" src="https://github.com/user-attachments/assets/1a9ca6eb-2d47-453e-9e9b-b13d568392d3" /> <br>
Figure 5: driver input voltage waveform <br>

<img width="1820" height="708" alt="image" src="https://github.com/user-attachments/assets/0386aaee-2dd8-423d-a008-74f991490f04" /> <br>
Figure 6: Collector current waveform for sinusoidal input voltage <br>

Reference: https://www.woolseyworkshop.com/2019/07/01/performing-a-circuit-simulation-in-kicad/










