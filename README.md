# Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso

# Contents

 - [Introduction](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#introduction) 
 - [1. Circuit Design](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#1circuit-design)
 - [2. Simulation Analysis](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#2simulation--analysis)
 - [3. Layout Design](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#3-layout-design)
 - [4. Optimization](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#4-optimization)
 - [5. Updated Design (320 nm PMOS)](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#5-updated-design-320-nm-pmos)
 - [6. Layout Update & Verification (320 nm PMOS)](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#6-layout-update--verification-320-nm-pmos)
 - [7. Final Comparision between 120n and 320n width of the pmos](https://github.com/Jaydeep8/Design-and-Optimization-of-CMOS-inverter-in-Cadence-Virtuoso?tab=readme-ov-file#7-final-comparision-between-120n-and-320n-width-of-the-pmos)



























# Introduction 

The CMOS inverter is the fundamental building block of nearly every digital integrated circuit. 
-	It converts logic “1” to “0” and vice-versa, enabling Boolean functionality for larger logic gates and sequential elements.
-	In steady states one transistor is always off, so static current ideally approaches zero, which makes it power-efficient. 

<img width="626" height="626" alt="image" src="https://github.com/user-attachments/assets/ba6da39a-0660-4ec3-bd6a-bfff3936a853" />



By the end of the project the inverter:

-	Achieves balanced noise margins and a switching threshold within 1% of VDD/2V.
-	Achieves low propagation delay.
-	Passes all sign-off checks (DRC, LVS, PEX) under worst-case process, voltage, and temperature conditions.

# 1.	CIRCUIT DESIGN 

## 1.1	Schematic Design 

<img width="940" height="1016" alt="image" src="https://github.com/user-attachments/assets/0fd8985e-91df-4221-ab6f-debbfbeca804" />

 **CMOS Inverter: Schematic Summary**

| Device | Type | Width | Length | Comments |
| :-- | :-- | :-- | :-- | :-- |
| M1 | PMOS | 120nm | 100nm | Pull-up, body to VDD |
| M2 | NMOS | 120nm | 100nm | Pull-down, body to GND |

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
-	VOUT: The output pin of the inverter, where the response is measured.
-	VDD: The supply voltage connected to the inverter’s PMOS source and the power rail.
1.2V
-	GND: The ground connection tied to the NMOS source and reference node.
-	Load Capacitor (1 fF): Connected at the vout to model the capacitive load of real circuits.
The 1 fF capacitor at the output simulates the small electrical load an inverter would have when driving other gates in a real circuit.

<img width="940" height="572" alt="image" src="https://github.com/user-attachments/assets/65bbcd29-d022-4629-9fd0-5816eb754dd3" />

 All the analyses in this project—like delay, noise margin, and power—are performed using this testbench setup.

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

This Energy is calculated using the measured supply current and voltage as the input pulse switches the inverter from low to high or high to low.

Estimated Energy per Transition:
The value indicated in setup is 2.104 femtojoules (fJ) per switching event.

## 2.5  Noise-Margin Calculation

<img width="1834" height="771" alt="image" src="https://github.com/user-attachments/assets/a394a8c7-979e-4683-a5c6-72c869ecb6ef" />

the second graph is the derivative of the vout signal with respect to the input signal.

Based on the given values:

-  VIH = 609.484 mV
-  VIL = 308.612 mV 
-  VOH = 1.2 V 
-  VOL = 0 V 

The noise margins are calculated as follows:

$$
\begin{aligned}
\text{Noise Margin High (NMH)} &= V_{OH} - V_{IH} = 1.2 \text{V} - 0.609484 \text{V} = 0.590516 \text{V} \\
\text{Noise Margin Low (NML)} &= V_{IL} - V_{OL} = 0.308612 \text{V} - 0 \text{V} = 0.308612 \text{V}
\end{aligned}
$$





# 3. Layout Design 

## 3.1  Layout Implementation	

 **inverter layout**
 
<img width="890" height="1093" alt="image" src="https://github.com/user-attachments/assets/5a801317-3105-491a-bc15-194438c30bdf" />

## 3.2  DRC Results

**“DRC clean” confirmation window**

<img width="689" height="686" alt="image" src="https://github.com/user-attachments/assets/154ecbcd-8bf4-4e30-88e8-14ee1afd1073" />

the Design Rule Check (DRC) results verify that the layout complies with all foundry manufacturing rules like minimum and maximum spacing, widths, layers, etc...

## 3.3  LVS Results

**“LVS match” confirmation window**

<img width="1019" height="651" alt="image" src="https://github.com/user-attachments/assets/d2d4c349-12fb-4493-a834-b84ec34c09d5" />

the Layout Versus Schematic (LVS) check confirms that the layout's transistor connectivity matches exactly with the original schematic.

## 3.4 Parasitic Extraction View	

**Assura RCX–generated extracted layout**

<img width="622" height="762" alt="4 inverter" src="https://github.com/user-attachments/assets/92eeaf68-87eb-45d3-8556-b8704f915a85" />

 here resistance and capacitance elements from wires and devices are identified. The extracted view enables accurate post-layout simulations to capture real-world effects on delay and power.

## 3.5 Post-Layout vs Schematic Timing	

here i did the timing comparision of output signal of schematic and the extracted layout with the help of config view which introduces all the resistance and the capacitance from the extracted layout.

<img width="940" height="367" alt="image" src="https://github.com/user-attachments/assets/68fefcef-a3ba-43a4-993e-ee94a9db1767" />

here the difference is **4.40943 ps**, this difference is caused by the resistance and capacitance present in the layout

# 4. Optimization

<img width="1063" height="789" alt="image" src="https://github.com/user-attachments/assets/2253a801-4b23-4c3a-bf49-4c1e5872f805" />

an iverter to be ideal the switching threshold should be equal to the vdd/2 which is 0.6 V
- The switching threshold is the input voltage when is vin = vout 
- here in the image it is observed that the **switching threshold** is < vdd/2
  
###
then after parametric analysis is performed with pmos width as parameter to achieve switching threshold near to **vdd/2**

<img width="1715" height="718" alt="image" src="https://github.com/user-attachments/assets/8791b5ff-3fe7-44a2-bf7f-8c5db6087c42" />

- here it is observed that at the **pmos width 331n & 373n** the swithching threshold is almost **vdd/2** 

## 4.1 Corner Sweep Analysis

To get the best performance and reliability across different operating conditions, I performed corner analysis using with the following combinations:

- PMOS widths analyzed: 120 nm, 240 nm, and 320 nm

- Temperature points: –24 °C, 27 °C, and 65 °C

- Process corners: FF (fast-fast), SS (slow-slow), FS (fast-slow), and SF (slow-fast)

<img width="1831" height="756" alt="image" src="https://github.com/user-attachments/assets/1275009f-b190-4267-a9f7-458726a004d8" />

## 4.2 Corner Sweep Analysis Results	

<img width="1825" height="788" alt="image" src="https://github.com/user-attachments/assets/01418d56-80de-4ebf-83e2-c078e08c2c96" />

<img width="1832" height="746" alt="image" src="https://github.com/user-attachments/assets/fae2af48-725a-4c57-a662-8a81c57514e2" />
output with the pmos width of 320n for all the cases 

# 5. Updated Design (320 nm PMOS)	

setting pmos width 320n in the schematic 

<img width="658" height="712" alt="1 320 inverter" src="https://github.com/user-attachments/assets/070e7236-f85b-46de-89af-0475a9782081" />

 **CMOS Inverter: Schematic Summary**

| Device | Type | Width | Length | Comments |
| :-- | :-- | :-- | :-- | :-- |
| M1 | PMOS | 320nm | 100nm | Pull-up, body to VDD |
| M2 | NMOS | 120nm | 100nm | Pull-down, body to GND |

**Pins:**
VIN, VOUT, VDD, GND

## 5.1 Transient Analysis	

<img width="940" height="1064" alt="image" src="https://github.com/user-attachments/assets/29faa5d0-0567-42f6-afb6-743af31558c0" />

## 5.2 DC Analysis (VTC)	

<img width="940" height="1064" alt="image" src="https://github.com/user-attachments/assets/de100fe1-0511-40e4-8d7a-3619713552a9" />

## 5.3	Propagation Delay Measurement

<img width="1831" height="750" alt="image" src="https://github.com/user-attachments/assets/a28fa135-799f-45f9-ab17-7f055b088fe1" />

- Tpdr = 10.4156 ps
- Tpdf = 12.6668 ps
- Tpd = (Tpdr +Tpdf) / 2 

ADE L calculator calculated tpd = 11.5414 ps


## 5.4	Energy Estimation

<img width="1829" height="747" alt="image" src="https://github.com/user-attachments/assets/7bcaaeaf-633d-4610-aabf-285db9d99f47" />


This Energy is calculated using the measured supply current and voltage as the input pulse switches the inverter from low to high or high to low.

Estimated Energy per Transition:
The value indicated in setup is 2.459 femtojoules (fJ) per switching event.

## 5.5  Noise-Margin Calculation

<img width="1832" height="748" alt="image" src="https://github.com/user-attachments/assets/3aa7df3d-61a5-4d13-abfd-d04098ed91db" />


the second graph is the derivative of the vout signal with respect to the input signal.

Based on the given values:

-  VIH = 747.224 mV
-  VIL = 461.942 mV 
-  VOH = 1.2 V 
-  VOL = 0 V 

The noise margins are calculated as follows:

$$
\begin{aligned}
\text{Noise Margin High (NMH)} &= V_{OH} - V_{IH} = 1.2 \text{V} - 0.747224 \text{V} = 0.452776 \text{V} \\
\text{Noise Margin Low (NML)} &= V_{IL} - V_{OL} = 0.461942 \text{V} - 0 \text{V} = 0.461942 \text{V}
\end{aligned}
$$

## 5.6 Power Analysis


**Static Power Analysis**

it is performed to check how much current is leakaged when circuit is not switching. Ideally it should be 0

<img width="1827" height="747" alt="image" src="https://github.com/user-attachments/assets/944fdb53-aadc-4cce-8fe9-f473ca31a4ae" />


**Dynamic Power Analysis**

It is performed to check average power consumed during the switching operation of the circuit.
- here the current drawn from VDD is averaged and then multiplied with the 1.2 (VDD voltage) to get the dynamic power
- The dynamic power is **12.3 microwatts**
  
<img width="1835" height="749" alt="image" src="https://github.com/user-attachments/assets/04a61178-0e1e-43c0-b363-bcb600a31bd0" />


# 6. Layout Update & Verification (320 nm PMOS)

## 6.1 Updated Layout	

**inverter layout**

<img width="566" height="749" alt="inverter layout 320n png" src="https://github.com/user-attachments/assets/49d87d03-7b12-42cf-8a48-69c367996376" />
 
## 6.2  DRC Results

**“DRC clean” confirmation window**

<img width="556" height="688" alt="image" src="https://github.com/user-attachments/assets/de46c801-9547-4ee7-8121-98caceaec063" />

the Design Rule Check (DRC) results verify that the layout complies with all foundry manufacturing rules (spacing, widths, layers, etc.).

## 6.3  LVS Results

**“LVS match” confirmation window**

<img width="978" height="691" alt="image" src="https://github.com/user-attachments/assets/ff271531-9e6f-48cb-87df-11cf7646713b" />

the Layout Versus Schematic (LVS) check confirms that the layout's transistor connectivity matches exactly with the original schematic.

## 6.4 Parasitic Extraction View	

**Assura RCX–generated extracted layout**

<img width="566" height="749" alt="inverter layout av extract 320n png" src="https://github.com/user-attachments/assets/bf57f378-a241-4e68-b2b9-d55b46a3e580" />

## 6.5 Post-Layout vs Schematic Timing	

the timing comparision of output signal of schematic and the updated extracted layout

<img width="1831" height="749" alt="image" src="https://github.com/user-attachments/assets/ca769517-cad4-4ee3-9fc6-68b7f8dc3e0d" />

here the difference is **1.313994 ps**, earlier it was **4.40943 ps** with the 120n pmos width


# 7. Final Comparision between 120n and 320n width of the pmos

| Metric | 120 nm PMOS | 320 nm PMOS |
| :-- | :-- | :-- |
| Tpd | 16.34 ps | **11.54 ps** |
| Energy | 2.10 fJ | **2.45 fJ** |
| NML / NMH | 0.308 V / 0.590 V | **0.461 V / 0.452 V** |
| Post-layout Δdelay | 4.40 ps | **1.31 ps** |
| VM deviation from 0.6 V | 23% | **0.8%** |





























