# Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso



# Introduction 

<img width="626" height="626" alt="image" src="https://github.com/user-attachments/assets/ba6da39a-0660-4ec3-bd6a-bfff3936a853" />

The CMOS inverter is the fundamental building block of nearly every digital integrated circuit. By complementarily pairing an NMOS pull-down network with a PMOS pull-up network, it provides:
-	It converts logic “1” to “0” and vice-versa, enabling Boolean functionality for larger logic gates and sequential elements.
-	In steady states one transistor is always off, so static current ideally approaches zero, making CMOS inverters far more power-efficient than resistor–transistor or NMOS-only logic families.
-	Driver scalability – by adjusting transistor widths, the inverter’s drive strength can be tuned to meet timing, noise-margin, and fan-out requirements, or to buffer and distribute high-frequency on-chip clocks.



By the end of the project the inverter:
•	Achieves balanced noise margins and a switching threshold within 1% of VDD/2V.
•	Meets propagation-delay and dynamic-power targets derived from first-order RC analysis of CMOS delays.
•	Passes all sign-off checks (DRC, LVS, PEX) under worst-case process, voltage, and temperature conditions.

# 1	CIRCUIT DESIGN 
## 1.1	Schematic Design 

<img width="940" height="1016" alt="image" src="https://github.com/user-attachments/assets/0fd8985e-91df-4221-ab6f-debbfbeca804" />

### CMOS Inverter: Schematic Summary

| Device | Type | Width | Length | Comments |
| :-- | :-- | :-- | :-- | :-- |
| M1 | PMOS | 120nm | 100nm | Pull-up, bulk to VDD |
| M2 | NMOS | 120nm | 100nm | Pull-down, bulk to GND |

**Pins:**
VIN, VOUT, VDD, GND

## 1.2	Symbol Creation

This symbolic view represents the CMOS inverter as a simple block with clear input, output, VDD, and GND pins.
It makes the inverter easy to reuse in bigger circuits and speeds up creating testbenches.

<img width="786" height="525" alt="image" src="https://github.com/user-attachments/assets/d8783972-8c09-4744-91b0-11915d865dc0" />


## 1.3  Testbench Setup

The testbench includes the following connections:

-	VIN: A pulse voltage source providing the input signal to the inverter.
0V to 1.2V, Tr =10ps, Tf=10ps, period=200ps
-	VOUT: The output node of the inverter, where the response is measured.
-	VDD: The supply voltage connected to the inverter’s PMOS source and the power rail.
1.2V
-	GND: The ground connection tied to the NMOS source and reference node.
-	Load Capacitor (1 fF): Connected between VOUT and GND to model the capacitive load of real circuits.
The 1 fF capacitor at the output simulates the small electrical load an inverter would have when driving other gates in a real circuit. This makes the timing and power results from the simulation more accurate and realistic. All the analyses in this project—like delay, noise margin, and power—are performed using this testbench setup.

<img width="940" height="572" alt="image" src="https://github.com/user-attachments/assets/65bbcd29-d022-4629-9fd0-5816eb754dd3" />

# 2.	Simulation & Analysis                                                                  

## 2.1	Transient Analysis

<img width="940" height="1064" alt="image" src="https://github.com/user-attachments/assets/6065baa3-7fae-40d3-b68c-40a888cdb5fa" />




## 2.2	DC Analysis

<img width="940" height="1064" alt="image" src="https://github.com/user-attachments/assets/c4d8b754-ac16-44c7-8afe-ddade6110097" />


## 2.3	Propagation Delay Measurement

<img width="1031" height="435" alt="image" src="https://github.com/user-attachments/assets/612b5bf9-769b-4106-befe-b12294302049" />

- Tpdr = 22.2904 ps
- Tpdf = 10.3975 ps
- Tpd = (Tpdr +Tpdf) / 2 

ADE L calculator read-out of tpd = 16.344 ps


## 2.4	Energy Estimation


<img width="1025" height="430" alt="image" src="https://github.com/user-attachments/assets/bb1d5111-6b54-48f3-bf1b-7b1c788a291f" />

the energy consumed by the CMOS inverter for transition is displayed in the results window. This metric is calculated using the measured supply current and voltage as the input pulse switches the inverter from low to high or high to low.

Estimated Energy per Transition:
The value indicated in setup is 2.104 femtojoules (fJ) per switching event.

## 2.5  Noise-Margin Calculation

<img width="1834" height="771" alt="image" src="https://github.com/user-attachments/assets/a394a8c7-979e-4683-a5c6-72c869ecb6ef" />

Here I have done derivative of the vout signal and then the noise margin calculation is done

Based on the given values:

-  VIH = 609.484 mV
-  VIL = 308.612 mV 
-  VOH = 1.2 V 
-  VOL = 0 V 

The noise margins are calculated as follows:

$$
\begin{aligned}
\text{Noise Margin High (NMH)} &= V_{OH} - V_{IH} = 1.2\, \text{V} - 0.609484\, \text{V} = 0.590516\, \text{V} \\
\text{Noise Margin Low (NML)} &= V_{IL} - V_{OL} = 0.308612\, \text{V} - 0\, \text{V} = 0.308612\, \text{V}
\end{aligned}
$$





# Layout Design 

##  Layout Implementation	




























































