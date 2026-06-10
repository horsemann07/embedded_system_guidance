# Comprehensive Analog Hardware Interview Questions
## 10+ Years Experience | Question-Based Learning Format
### Basic → Intermediate → Advanced → Expert Level

---

## TABLE OF CONTENTS

1. [Passive Components & Fundamentals](#passive-components--fundamentals)
2. [Semiconductor Basics (BJT, FET, MOSFET)](#semiconductor-basics-bjt-fet-mosfet)
3. [Diodes & Rectification](#diodes--rectification)
4. [Op-Amp Fundamentals](#op-amp-fundamentals)
5. [Op-Amp Applications & Circuits](#op-amp-applications--circuits)
6. [Transistor Amplifiers](#transistor-amplifiers)
7. [Frequency Response & Filters](#frequency-response--filters)
8. [Stability & Compensation](#stability--compensation)
9. [Power Electronics & Regulation](#power-electronics--regulation)
10. [Signal Conditioning & Measurement](#signal-conditioning--measurement)
11. [Mixed-Signal & PCB Design](#mixed-signal--pcb-design)
12. [Real-World Troubleshooting](#real-world-troubleshooting)
13. [Advanced Topics](#advanced-topics)

---

# PASSIVE COMPONENTS & FUNDAMENTALS

## RESISTORS

### BASIC LEVEL

**Q1.1: What is a resistor and what does it do?**

**Q1.2: Explain the difference between ohmic and non-ohmic materials**

**Q1.3: Why do resistor values follow the E12, E24 series instead of any arbitrary value?**

**Q1.4: What is meant by "1% tolerance" on a resistor?**

**Q1.5: If you have a 1kΩ resistor with 5% tolerance, what is the actual resistance range?**

**Q1.6: Why do high-power resistors get hot? Calculate the temperature rise for a 10Ω, 10W resistor with ambient at 25°C (assume θJA = 100°C/W)**

**Q1.7: What is the difference between carbon film, metal film, and wirewound resistors?**

**Q1.8: When would you use a thermistor instead of a regular resistor?**

### INTERMEDIATE LEVEL

**Q1.9: A resistor is rated for 0.25W, 1kΩ. What maximum voltage and current can it handle safely?**

**Q1.10: Explain the temperature coefficient of resistance (TCR). Why is it important in precision circuits?**

**Q1.11: What is meant by "parasitic inductance" in a resistor? At what frequency does it become problematic?**

**Q1.12: You need a 15kΩ resistor but only have 10kΩ and 5kΩ available. How would you create 15kΩ? (Series? Parallel?) Show calculations**

**Q1.13: Design a circuit to measure current through a component without interrupting the circuit. (Hint: Use a low-value resistor as current sense)**

**Q1.14: In a precision analog circuit, you need ±0.1% accuracy. What type of resistor would you use and why? At what temperature range can you guarantee this?**

**Q1.15: What is the difference between tolerance and stability? Can a resistor be 1% tolerance but still drift 5% over temperature?**

### ADVANCED LEVEL

**Q1.16: Explain how resistor tolerance affects the gain accuracy of an inverting amplifier with gain = Rf/Rin. If both resistors are 1%, what is the gain tolerance?**

**Q1.17: In a high-frequency circuit (100MHz), why would a 0805 resistor behave differently than a 2512 resistor? What parasitic effect dominates?**

**Q1.18: A resistor has TCR of +100 ppm/°C and another has -100 ppm/°C. If you use both in a precision measurement circuit, what happens to error over temperature?**

**Q1.19: Calculate the voltage noise across a 10kΩ resistor at room temperature over 100kHz bandwidth. (Use thermal noise formula: Vn = √(4kTRΔf))**

**Q1.20: Design a precision current source using a resistor and op-amp. Explain why resistor accuracy is critical and how errors cascade**

### EXPERT LEVEL

**Q1.21: In a 10V supply with 1A load, you want ≤1°C temperature rise of a current-sense resistor. What should be the resistor value? What happens if you use a lower value (easier to measure) vs higher value (less heating)?**

**Q1.22: Explain the non-linear behavior of resistors under high electric field stress. What is the mechanism?**

**Q1.23: How does moisture absorption affect resistor stability? Why do some resistors require moisture barrier coatings?**

**Q1.24: In a precision network (ladder DAC), all resistors must track over temperature. Explain why matching is more important than absolute accuracy**

**Q1.25: Calculate the power dissipation and temperature distribution in a 10-resistor resistor ladder (voltage divider) supplying 1mA through each stage**

---

## CAPACITORS

### BASIC LEVEL

**Q2.1: What is capacitance? What physical property determines it?**

**Q2.2: A capacitor stores energy. How much energy is stored in a 100µF capacitor charged to 5V?**

**Q2.3: Explain the difference between capacitance (C) and charge (Q). What is the relationship?**

**Q2.4: If you charge a 1µF capacitor to 10V, how many coulombs of charge are stored?**

**Q2.5: What happens when you apply AC voltage across a capacitor? Why is it called a "short circuit at high frequency"?**

**Q2.6: What is the impedance of a capacitor? Does it change with frequency?**

**Q2.7: Why do some circuits have multiple capacitors (0.1µF + 10µF) on the power supply?**

### INTERMEDIATE LEVEL

**Q2.8: Calculate the impedance of a 100nF capacitor at DC, 1kHz, 100kHz, and 1MHz**

**Q2.9: What is the RC time constant? If R=1kΩ and C=100µF, what is τ? How long does it take to charge to 63% of supply voltage?**

**Q2.10: Explain the difference between ceramic, electrolytic, and film capacitors. When would you use each?**

**Q2.11: What is ESR (Equivalent Series Resistance) in a capacitor? Why does it matter in switching power supplies?**

**Q2.12: A circuit has a 0.1µF ceramic capacitor very close to an IC and a 10µF electrolytic far away. Why this arrangement? What problem does each solve?**

**Q2.13: Calculate the voltage across a capacitor if 1mA charges it for 10ms. (C = 1µF)**

**Q2.14: What is leakage current in a capacitor? Can it ruin a precision DC measurement?**

### ADVANCED LEVEL

**Q2.15: Design a decoupling network for a high-speed digital IC using 0.1µF, 1µF, and 10µF capacitors. Explain why each is needed**

**Q2.16: A power supply is unstable with excessive ripple. You measure that adding a large capacitor (100µF) helps but adding a small capacitor (10nF) hurts. Why?**

**Q2.17: In a precision analog circuit, a ceramic capacitor drifts 20% when heated from 25°C to 85°C. What capacitor type would you use instead? What are the tradeoffs?**

**Q2.18: Explain dielectric absorption in capacitors. Why is this a problem in precision sample-and-hold circuits?**

**Q2.19: Calculate the charge redistribution error if a 1V charged 1µF capacitor is switched to a 0V capacitor. How much error does this introduce in a precision ADC?**

**Q2.20: Design a precision integrator circuit. Explain why capacitor leakage, dielectric absorption, and dielectric constant variation all matter**

### EXPERT LEVEL

**Q2.21: A multi-layer ceramic capacitor (MLCC) shows X7R vs X5R temperature coefficient. What do these ratings mean? Which would you use for bias networks?**

**Q2.22: Explain the ferroelectric effect in ceramic dielectrics. How does it relate to DC bias on MLCCs?**

**Q2.23: In a 100V power supply, why does a 100µF/50V capacitor rated for ≤10V DC bias actually only store half the energy?**

**Q2.24: Design a precision voltage reference using capacitors as precision dividers. What errors must you account for?**

**Q2.25: Calculate the stability of an RC oscillator with R=100kΩ (±1%) and C=10µF (±5%). What is the frequency stability? How would you improve it?**

---

## INDUCTORS

### BASIC LEVEL

**Q3.1: What is inductance? What physical property determines it?**

**Q3.2: An inductor resists changes in current. Why would you use an inductor vs a resistor to limit current?**

**Q3.3: What is the voltage across a 100mH inductor if the current through it changes from 0 to 1A in 10µs?**

**Q3.4: At what frequency does an inductor with L=10µH have impedance of 100Ω?**

### INTERMEDIATE LEVEL

**Q3.5: Explain the difference between ideal and real inductors. What parasitic effects exist?**

**Q3.6: What is the DC resistance (DCR) of an inductor? How does it affect circuit efficiency?**

**Q3.7: Calculate the energy stored in a 10mH inductor carrying 5A**

**Q3.8: In a buck converter, the inductor current ripple is ±200mA. Why does this matter? What controls the ripple?**

### ADVANCED LEVEL

**Q3.9: Design an LC filter for a switching power supply. Explain the tradeoff between L, C, and switching frequency**

**Q3.10: An inductor shows self-resonance at 10MHz. What causes this? Why is it a problem?**

**Q3.11: Explain skin effect in inductors. At what frequency does it become significant? (Calculate for 1mm copper wire)**

**Q3.12: Design a precision current source using an inductor as a frequency-dependent element**

### EXPERT LEVEL

**Q3.13: In a multilayer inductor, explain eddy current losses and how to minimize them**

**Q3.14: Design an EMI filter using multiple inductors in series. Explain why a single large inductor doesn't work**

**Q3.15: Calculate the Q factor of an inductor at different frequencies and explain what it means for circuit stability**

---

# SEMICONDUCTOR BASICS (BJT, FET, MOSFET)

## DIODES

### BASIC LEVEL

**Q4.1: What is a PN junction? How is it formed?**

**Q4.2: What is the forward voltage drop of a silicon diode? A Schottky diode? A germanium diode?**

**Q4.3: What happens when reverse voltage exceeds the diode's breakdown voltage (Vbr)?**

**Q4.4: Why can't you measure diode resistance with an ohmmeter in both directions?**

**Q4.5: Draw the I-V characteristic of a diode. Identify forward bias, reverse bias, and breakdown regions**

**Q4.6: Calculate the voltage drop across a 1N4148 diode carrying 10mA forward current**

### INTERMEDIATE LEVEL

**Q4.7: What is the dynamic resistance (rd) of a diode? How does it change with operating point?**

**Q4.8: Explain the relationship between diode forward voltage and temperature. Why does it matter?**

**Q4.9: Design a simple power supply rectifier using a diode bridge. Explain how peak inverse voltage (PIV) rating matters**

**Q4.10: What is reverse recovery time in a diode? When does it cause problems?**

**Q4.11: Why is a Schottky diode preferred over a silicon diode in switching applications?**

**Q4.12: Calculate the average and RMS current in a full-wave rectifier with 10V peak AC input and 100Ω load**

### ADVANCED LEVEL

**Q4.13: Explain parasitic capacitance in a diode. How does it affect high-frequency performance?**

**Q4.14: Design a precision photodiode detector circuit. Explain all error sources**

**Q4.15: In a switching converter, Schottky diode reverse recovery causes spikes. How would you eliminate them?**

**Q4.16: Explain the Zener diode voltage regulation mechanism. Why isn't it ideal?**

**Q4.17: Design a precision voltage regulator using a Zener diode, resistor, and load. Calculate component values for 5V output from 12V input**

### EXPERT LEVEL

**Q4.18: Explain avalanche breakdown vs Zener breakdown in reverse-biased diodes**

**Q4.19: Design a precision current source using diodes and resistors. How would you temperature compensate it?**

**Q4.20: In a RF mixer circuit, predict diode performance nonlinearities. How do you characterize them?**

---

## BIPOLAR JUNCTION TRANSISTOR (BJT)

### BASIC LEVEL

**Q5.1: Draw a BJT (NPN and PNP). Label all terminals (Base, Collector, Emitter)**

**Q5.2: What does β (beta) mean for a BJT? Typical value?**

**Q5.3: Explain the three operating regions: Cutoff, Active, and Saturation**

**Q5.4: In the active region, what is the relationship between Ic, Ib, and β?**

**Q5.5: What is Vce(sat)? Why is it important in digital switching?**

**Q5.6: Why must you never leave the base of a BJT floating (unconnected)?**

**Q5.7: Calculate the base current needed to turn on a BJT with β=100, driving a 1A load**

### INTERMEDIATE LEVEL

**Q5.8: Draw a BJT as a current amplifier. Explain why it's a current-controlled device**

**Q5.9: What is Early voltage (Va)? How does it affect the BJT's output impedance?**

**Q5.10: Design a BJT switch to turn on a 100mA LED from a microcontroller GPIO (3.3V output, needs base resistor)**

**Q5.11: In a common-emitter amplifier, what is the voltage gain? How does it depend on load resistance?**

**Q5.12: Explain the difference between small-signal and large-signal operation**

**Q5.13: What is the input impedance of a BJT? How does biasing affect it?**

**Q5.14: Why do BJTs have lower frequency response than FETs?**

### ADVANCED LEVEL

**Q5.15: Design a precision current mirror using BJTs. Explain why matching is critical**

**Q5.16: Analyze a Widlar current source. Why is it superior to simple current mirror?**

**Q5.17: Explain the Ebers-Moll model of BJT. When is it necessary to use it?**

**Q5.18: In a BJT differential pair, explain CMRR (Common Mode Rejection Ratio)**

**Q5.19: Design a transimpedance amplifier for low-level signal measurement. Include BJT noise analysis**

**Q5.20: Explain thermal runaway in BJTs. How do you prevent it?**

### EXPERT LEVEL

**Q5.21: Derive the frequency response of a BJT amplifier stage. Calculate -3dB bandwidth**

**Q5.22: In precision analog circuits, explain why BJT collectors must be matched in temperature**

**Q5.23: Design a precision logarithmic amplifier using BJTs and op-amps. Explain error sources**

**Q5.24: Analyze substrate noise coupling from BJTs on an IC to sensitive analog circuits**

**Q5.25: Calculate the maximum frequency at which a BJT can operate based on fT and application requirements**

---

## FIELD EFFECT TRANSISTOR (FET/JFET)

### BASIC LEVEL

**Q6.1: Draw a JFET (N-channel and P-channel). Label terminals (Gate, Source, Drain)**

**Q6.2: What is Vgs(off) (pinch-off voltage)? What happens when Vgs reaches this?**

**Q6.3: Why is JFET called a "voltage-controlled" device unlike BJT which is "current-controlled"?**

**Q6.4: What is the input impedance of a JFET? How does it compare to BJT?**

**Q6.5: In the saturation region, what controls the drain current?**

**Q6.6: Explain why a JFET has extremely low gate current (nanoamps range)**

**Q6.7: If a JFET has Idss = 10mA and Vgs(off) = -5V, what is Id when Vgs = -2V? (Use Shockley equation)**

### INTERMEDIATE LEVEL

**Q6.8: Draw the I-V characteristics of a JFET and identify all regions**

**Q6.9: Explain transconductance (gm) of a FET. How does it compare to BJT?**

**Q6.10: Design a JFET source follower (common-drain) amplifier. Calculate voltage gain**

**Q6.11: What is the output impedance of a JFET in common-source configuration?**

**Q6.12: Explain how a JFET can be used as a voltage-controlled resistor**

**Q6.13: Why do JFETs have better high-frequency noise performance than BJTs?**

**Q6.14: Design a precision sample-and-hold circuit using a JFET as a switch**

### ADVANCED LEVEL

**Q6.15: Explain the substrate effect in JFETs. How does it affect circuit performance?**

**Q6.16: Derive the frequency response of a JFET amplifier. What limits bandwidth?**

**Q6.17: Design a low-noise preamplifier for a photodiode using a JFET input stage**

**Q6.18: Explain how to match JFETs for precision current mirrors**

**Q6.19: Analyze a cascode amplifier using JFET and BJT. Explain why it has better gain and bandwidth**

**Q6.20: Design a precision integrator using JFET as switch. Explain leakage current limitations**

### EXPERT LEVEL

**Q6.21: Explain parasitic capacitances in JFETs and their effect on high-frequency response**

**Q6.22: Design a precision logarithmic amplifier using JFET. Explain temperature compensation**

**Q6.23: Analyze noise sources in JFET amplifier: thermal, 1/f, shot noise. Calculate total noise**

**Q6.24: In precision measurement circuits, explain why JFET input impedance is better than BJT for certain applications**

**Q6.25: Design a precision current source using matched JFETs. Calculate accuracy and temperature stability**

---

## METAL OXIDE SEMICONDUCTOR FET (MOSFET)

### BASIC LEVEL

**Q7.1: Draw an N-channel and P-channel MOSFET. Label all terminals (Gate, Source, Drain, Substrate)**

**Q7.2: What is Vth (threshold voltage) of a MOSFET?**

**Q7.3: What happens when Vgs > Vth? When Vgs < Vth?**

**Q7.4: Explain the difference between depletion-mode and enhancement-mode MOSFETs**

**Q7.5: Why can MOSFETs switch faster than BJTs?**

**Q7.6: What is drain current saturation? Explain the saturation voltage Vds(sat)**

**Q7.7: Compare input capacitance of MOSFET vs BJT. Which is larger?**

### INTERMEDIATE LEVEL

**Q7.8: What is Rds(on) of a MOSFET? How does it change with temperature?**

**Q7.9: In a MOSFET switch driving a resistive load, calculate the voltage drop across Rds(on)**

**Q7.10: Explain the difference between MOSFET and JFET in terms of gate current and input impedance**

**Q7.11: Why do MOSFETs have "body diode"? When does it conduct?**

**Q7.12: Design a MOSFET gate driver circuit for a 5V MOSFET driven from 3.3V logic**

**Q7.13: Explain channel length modulation in MOSFETs. How does it affect output impedance?**

**Q7.14: What are the advantages of n-channel MOSFETs over p-channel for power switching?**

### ADVANCED LEVEL

**Q7.15: Design a MOSFET gate driver for high-speed switching (10MHz+). Include deadtime considerations**

**Q7.16: Explain parasitic capacitances in MOSFETs (Cgs, Cgd, Cds). How do they affect switching speed?**

**Q7.17: In a synchronous buck converter, why do you use low Rds(on) MOSFETs? Calculate efficiency improvement**

**Q7.18: Design a precision current mirror using MOSFETs. Explain matching requirements**

**Q7.19: Analyze the frequency response of a MOSFET common-source amplifier. Calculate gain-bandwidth product**

**Q7.20: Explain thermal stability of MOSFETs. Why do they need current sharing circuits in parallel?**

### EXPERT LEVEL

**Q7.21: Derive the small-signal model of a MOSFET including all parasitic elements**

**Q7.22: Design a cascode amplifier using MOSFETs for very high gain. Explain tradeoffs**

**Q7.23: Explain short-channel effects in MOSFETs. How do they affect circuit design?**

**Q7.24: In an analog IC, explain leakage current issues and mitigation techniques**

**Q7.25: Design a precision logarithmic amplifier using MOSFET as temperature sensor element**

---

## BJT vs FET vs MOSFET COMPARISON

**Q8.1: Create a comparison table: Input impedance, Speed, Noise, Cost, Power consumption**

**Q8.2: For each application, which device is best? Why?**
- **Audio preamplifier**
- **Power supply controller**
- **High-frequency RF amplifier**
- **Precision measurement circuit**
- **Digital logic gate**

**Q8.3: Why do op-amps use both BJT and FET internally?**

**Q8.4: Explain the "fT" figure of merit for BJTs vs MOSFETs at different frequencies**

**Q8.5: In a precision circuit, would you use BJT or FET input stage? Justify**

---

# OP-AMP FUNDAMENTALS

### BASIC LEVEL

**Q9.1: Draw an op-amp symbol. Label inputs (+ and -), output, and power supplies**

**Q9.2: What does "ideal op-amp" mean? List all ideal characteristics**

**Q9.3: What is "open-loop gain" of an op-amp? Typical value?**

**Q9.4: Explain the concept of "virtual short" (virtual ground) in op-amp circuits**

**Q9.5: Why must an op-amp have feedback to be useful? What happens without feedback?**

**Q9.6: In a closed-loop amplifier, the output swings to rail. Is this normal? Why or why not?**

### INTERMEDIATE LEVEL

**Q9.7: Calculate the closed-loop gain of an amplifier with Aol = 100,000 and feedback factor β = 0.1**

**Q9.8: What is CMRR (Common Mode Rejection Ratio)? Typical value? Why is it important?**

**Q9.9: Explain input offset voltage (Vos). How does it affect DC circuits?**

**Q9.10: What are input bias currents (Ib+, Ib-)? Why do they matter?**

**Q9.11: Explain "slew rate". Calculate the maximum signal frequency that can be amplified at 1V amplitude**

**Q9.12: What is PSRR (Power Supply Rejection Ratio)? Why is it important?**

**Q9.13: Calculate the output noise voltage of an op-amp amplifier with gain=100, input noise = 100nV/√Hz, BW = 1MHz**

### ADVANCED LEVEL

**Q9.14: Derive the closed-loop gain including finite open-loop gain: Acl = Aol / (1 + Aol × β)**

**Q9.15: Explain frequency compensation in op-amps. What is GBW (Gain-Bandwidth Product)?**

**Q9.16: In a precision DC amplifier, why must you use input offset nulling?**

**Q9.17: Design a precision photodiode transimpedance amplifier. Address all error sources**

**Q9.18: Explain the difference between rail-to-rail and non-rail-to-rail op-amps**

**Q9.19: Analyze the effect of finite output impedance on loading effects**

**Q9.20: Design a precision instrumentation amplifier using three op-amps. Calculate gain accuracy**

### EXPERT LEVEL

**Q9.21: Explain noise figure (NF) of an op-amp. Calculate total system noise figure**

**Q9.22: Derive the frequency response of an op-amp amplifier including all poles and zeros**

**Q9.23: In precision signal conditioning, explain intermodulation distortion and IM3**

**Q9.24: Design a precision analog summing junction. Explain error sources and mitigation**

**Q9.25: Analyze the stability of a multi-stage op-amp system. Calculate overall phase margin and gain margin**

---

# OP-AMP APPLICATIONS & CIRCUITS

### BASIC LEVEL

**Q10.1: Draw an inverting amplifier. Derive the gain formula**

**Q10.2: Draw a non-inverting amplifier. What is the minimum gain?**

**Q10.3: Draw a voltage follower (unity-gain buffer). What is its purpose?**

**Q10.4: Draw a summing amplifier. Explain how it works**

**Q10.5: Draw a differencing (subtraction) amplifier. Derive the output voltage**

**Q10.6: Explain the difference between inverting and non-inverting configurations**

### INTERMEDIATE LEVEL

**Q10.7: Design a 10V/V non-inverting amplifier for a 100mV input signal. Choose resistor values**

**Q10.8: In an inverting amplifier with gain = -100, what is the input impedance? What happens if Rin is very large (1MΩ)?**

**Q10.9: Design a precision current-to-voltage converter (transimpedance amplifier) for 1µA input. Target output: 1V**

**Q10.10: Draw a precision integrator circuit. Explain the integrator time constant**

**Q10.11: Draw a precision differentiator circuit. What is its main problem? How do you fix it?**

**Q10.12: Design a precision logarithmic amplifier using op-amp and diode. Derive the output formula**

### ADVANCED LEVEL

**Q10.13: Design a precision exponential amplifier (antilog). Explain temperature compensation needs**

**Q10.14: Analyze the frequency response of an integrator. Calculate -3dB point**

**Q10.15: Design a precision comparator with hysteresis (Schmitt trigger) using op-amp. Set threshold at 2.5V**

**Q10.16: Explain the difference between comparator and op-amp in switching applications**

**Q10.17: Design a precision peak detector (sample-and-hold). Address leakage and droop**

**Q10.18: Design a precision precision precision peak-hold circuit with fast attack, slow release**

**Q10.19: Design a multi-stage signal conditioning circuit for thermocouple. Include: CJC, amplification, filtering**

**Q10.20: Design a precision bridge amplifier for load cell measurement. Calculate gain and linearity**

### EXPERT LEVEL

**Q10.21: Design a low-noise instrumentation amplifier for ECG measurement. Calculate SNR and CMRR**

**Q10.22: Analyze the total harmonic distortion (THD) of a precision amplifier. Include all nonlinearity sources**

**Q10.23: Design a precision transconductance amplifier (voltage-to-current converter). Address finite output impedance**

**Q10.24: Design a precision precision precision charge amplifier for piezoelectric sensor. Address high-impedance leakage**

**Q10.25: Design a multi-channel precision data acquisition front-end. Address crosstalk, settling time, aliasing**

---

# TRANSISTOR AMPLIFIERS

### BASIC LEVEL

**Q11.1: Draw a BJT common-emitter amplifier with bias network. Label all components**

**Q11.2: What is the AC voltage gain of a common-emitter amplifier with Rc = 1kΩ and re = 25Ω?**

**Q11.3: Draw a common-collector (emitter-follower) amplifier. What is its voltage gain?**

**Q11.4: Draw a common-base amplifier. What are its advantages over common-emitter?**

**Q11.5: Explain DC biasing of a transistor amplifier. What is the purpose of bias resistors?**

**Q11.6: What is quiescent operating point (Q-point)? Why is it important?**

### INTERMEDIATE LEVEL

**Q11.7: Design a common-emitter amplifier for 10V/V voltage gain with 1kΩ load resistance. Choose component values**

**Q11.8: Calculate the input impedance of a common-emitter amplifier with β = 100, re = 25Ω, Rb = 100kΩ**

**Q11.9: Design an emitter-follower amplifier for impedance buffering (high input to low output impedance)**

**Q11.10: Explain the frequency response of a common-emitter amplifier. Identify all poles**

**Q11.11: What limits the maximum frequency of operation in BJT amplifiers?**

**Q11.12: Design a two-stage BJT amplifier with overall gain = 1000V/V. How do you cascade them?**

**Q11.13: Analyze the noise of a BJT amplifier. Which stage dominates?**

### ADVANCED LEVEL

**Q11.14: Design a precision BJT amplifier for low-level signal (100µV). Address all noise sources**

**Q11.15: Analyze the frequency response of a multi-stage amplifier. Calculate overall -3dB bandwidth**

**Q11.16: Design a cascode amplifier using BJTs. Explain why it has higher gain-bandwidth product**

**Q11.17: Explain the Early effect on output impedance. How does it affect voltage gain?**

**Q11.18: Design a precision current mirror using matched BJTs. Calculate accuracy**

**Q11.19: Design a precision active load for BJT amplifier. Explain advantages over resistive load**

**Q11.20: Analyze the nonlinearity (THD) of a BJT amplifier. How does biasing affect it?**

### EXPERT LEVEL

**Q11.21: Derive the small-signal equivalent circuit of a BJT common-emitter amplifier including all parasitic impedances**

**Q11.22: Design a low-noise RF amplifier using BJTs. Calculate noise figure and gain**

**Q11.23: Design a precision transimpedance amplifier using BJT for low-level current measurement**

**Q11.24: Analyze stability of a BJT feedback amplifier. Calculate phase margin and gain margin**

**Q11.25: Design a precision log amplifier using matched BJTs. Explain temperature compensation**

---

# FREQUENCY RESPONSE & FILTERS

### BASIC LEVEL

**Q12.1: What is frequency response? Why is it important?**

**Q12.2: Define -3dB bandwidth. Why is 3dB significant?**

**Q12.3: What is cutoff frequency (fc) of an RC filter?**

**Q12.4: Sketch the Bode plot of a first-order low-pass filter. Label key points**

**Q12.5: Calculate the cutoff frequency of a circuit with R = 1kΩ and C = 100nF**

**Q12.6: At the cutoff frequency, what is the phase shift in a first-order filter?**

**Q12.7: Why is "rolloff rate" important in filter design?**

### INTERMEDIATE LEVEL

**Q12.8: Design a first-order low-pass filter with fc = 10kHz. Choose R and C values**

**Q12.9: Compare first-order, second-order, and third-order filters. What are tradeoffs?**

**Q12.10: Explain Butterworth, Chebyshev, and Bessel filter responses. When would you use each?**

**Q12.11: Design a Butterworth low-pass filter with fc = 1kHz and -20dB at 10kHz. Calculate required order**

**Q12.12: Draw and analyze a high-pass filter. What is its frequency response?**

**Q12.13: What is a bandpass filter? Design one with fc = 10kHz and Q = 10**

**Q12.14: Design a notch filter to remove 60Hz powerline noise while preserving other frequencies**

### ADVANCED LEVEL

**Q12.15: Derive the transfer function of a second-order Butterworth filter. Identify pole locations**

**Q12.16: Design a multi-stage filter for sensor signal conditioning. Address noise vs settling time tradeoff**

**Q12.17: Explain the relationship between filter order, bandwidth, and ringing (overshoot)**

**Q12.18: Design an analog anti-aliasing filter for an ADC. Explain Nyquist rate requirement**

**Q12.19: Analyze the group delay of different filter types. Why is it important for transient signals?**

**Q12.20: Design a precision notch filter with -60dB rejection at center frequency. Address component matching**

### EXPERT LEVEL

**Q12.21: Design a multiple feedback filter topology. Explain advantages over Sallen-Key**

**Q12.22: Analyze the sensitivity of filter performance to component tolerances. Calculate worst-case response**

**Q12.23: Design a variable-frequency filter for tuning applications. Explain tuning mechanism**

**Q12.24: Design an elliptic (Cauer) filter for maximum stopband attenuation with minimum order**

**Q12.25: Analyze the transient response of various filter types. Calculate settling time and overshoot**

---

# STABILITY & COMPENSATION

### BASIC LEVEL

**Q13.1: What does "stability" mean in feedback circuits? What is "oscillation"?**

**Q13.2: What is "phase margin"? Typical acceptable value?**

**Q13.3: What is "gain margin"? Why is it needed?**

**Q13.4: Draw a Bode plot showing stable and unstable systems**

**Q13.5: What happens to phase as frequency increases in a multi-pole system?**

### INTERMEDIATE LEVEL

**Q13.6: Explain the Nyquist criterion for stability. How do you use a Nyquist plot?**

**Q13.7: What is "compensation" in feedback circuits? Why is it needed?**

**Q13.8: Explain pole-zero cancellation. When is it used in circuit design?**

**Q13.9: What is lead compensation? How does it improve phase margin?**

**Q13.10: What is lag compensation? What does it do to system response?**

**Q13.11: Design a compensating network for an unstable op-amp circuit with phase margin = 10°. Target 45° margin**

**Q13.12: Explain the difference between dominant-pole compensation and other compensation techniques**

### ADVANCED LEVEL

**Q13.13: Analyze the stability of a three-pole system. Calculate required phase margin for adequate stability**

**Q13.14: Design a precision feedback system with speed and accuracy requirements. Address tradeoffs**

**Q13.15: Explain right-half-plane (RHP) zeros and their effect on stability**

**Q13.16: Design a current-mode feedback system. Explain stability advantages**

**Q13.17: Analyze the transient response (step response) of a feedback system. Explain overshoot vs settling time**

**Q13.18: Design a precision feedback amplifier with low distortion. Address nonlinearity in the feedback path**

**Q13.19: Analyze the effect of parasitic impedances on feedback loop stability**

**Q13.20: Design a precision regulated power supply. Explain stability against load and line changes**

### EXPERT LEVEL

**Q13.21: Derive the closed-loop transfer function including all poles and zeros**

**Q13.22: Design a compensator for a system with multiple resonant peaks. Use peak shaving technique**

**Q13.23: Analyze the stability of a precision SAR ADC feedback system during conversion**

**Q13.24: Design a precision precision precision digital compensator for analog system**

**Q13.25: Analyze the stability of a full H-bridge power converter system. Address current feedback interactions**

---

# POWER ELECTRONICS & REGULATION

### BASIC LEVEL

**Q14.1: What is a linear voltage regulator? How does it work?**

**Q14.2: What is "dropout voltage"? Why does it matter?**

**Q14.3: Explain the difference between series and shunt regulation**

**Q14.4: What is a switching (buck/boost) converter? Why are they more efficient?**

**Q14.5: Explain the difference between buck and boost converters**

**Q14.6: Calculate the efficiency of a linear regulator with 12V input, 5V output, 1A load**

**Q14.7: What is "load regulation"? Why is it specified for voltage regulators?**

### INTERMEDIATE LEVEL

**Q14.8: Design a linear voltage regulator to provide 3.3V @ 500mA from 5V input. Address thermal dissipation**

**Q14.9: Design a switching buck converter for the same application. Calculate efficiency**

**Q14.10: Explain the inductor current ripple in a buck converter. How does it affect design?**

**Q14.11: What is the boost converter duty cycle for 5V input to 12V output?**

**Q14.12: Design a buck-boost converter for a circuit requiring ±15V supplies from single 12V input**

**Q14.13: Explain the difference between pulse-width modulation (PWM) and voltage control**

**Q14.14: What is "loop bandwidth" of a switching power supply? How does it affect transient response?**

### ADVANCED LEVEL

**Q14.15: Design a precision current-limiting power supply. Address current-sense accuracy**

**Q14.16: Design a soft-start circuit for a switching power supply. Explain its purpose**

**Q14.17: Analyze the frequency response of a buck converter feedback system. Explain stability issues**

**Q14.18: Design a multi-rail power supply with sequencing (one supply must turn on before another)**

**Q14.19: Design a redundant power supply system with diode OR-ing. Address current sharing**

**Q14.20: Design a precision voltage reference. Explain temperature and aging stability**

### EXPERT LEVEL

**Q14.21: Design a synchronous buck converter. Explain advantages over standard buck**

**Q14.22: Analyze the EMI/RFI issues in switching power supplies. Design mitigation**

**Q14.23: Design a precision feedback system for a switching power supply. Address cross-regulation**

**Q14.24: Design a modular power supply for parallel load sharing. Address load balancing**

**Q14.25: Design a high-efficiency power converter with soft-switching (zero-voltage switching)**

---

# SIGNAL CONDITIONING & MEASUREMENT

### BASIC LEVEL

**Q15.1: What is a sensor? What does signal conditioning do?**

**Q15.2: Draw the basic signal conditioning chain: Sensor → Filter → Amplifier → ADC**

**Q15.3: What is a transducer?**

**Q15.4: Explain the difference between a sensor and a transducer**

**Q15.5: What is excitation voltage for sensors? Why is it needed?**

### INTERMEDIATE LEVEL

**Q15.6: Design a signal conditioning circuit for a thermistor (NTC) temperature sensor**

**Q15.7: Design a signal conditioning circuit for a strain gauge. Explain bridge balance**

**Q15.8: What is a Wheatstone bridge? When is it used?**

**Q15.9: Design a signal conditioning circuit for a load cell (bridge output)**

**Q15.10: Explain the difference between single-ended and differential signal measurement**

**Q15.11: When would you use an instrumentation amplifier vs op-amp?**

**Q15.12: Design a precision RTD (Pt100) measurement circuit. Address 4-wire configuration**

**Q15.13: Design a precision analog signal conditioning system for a 4-20mA current loop**

### ADVANCED LEVEL

**Q15.14: Design a precision thermocouple measurement circuit including cold-junction compensation**

**Q15.15: Design a precision pressure sensor signal conditioning circuit. Address bridge nonlinearity**

**Q15.16: Design a precision flow measurement system. Explain differential pressure transducer interface**

**Q15.17: Analyze the noise in a precision signal conditioning system. Calculate SNR**

**Q15.18: Design a multi-channel data acquisition system. Address crosstalk and settling time**

**Q15.19: Design a precision pH sensor conditioning circuit (high-impedance input)**

**Q15.20: Design a precision accelerometer signal conditioning circuit. Include AC coupling and filtering**

### EXPERT LEVEL

**Q15.21: Design a precision precision precision weigh scale signal conditioning. Include hysteresis and nonlinearity correction**

**Q15.22: Design a microwave power sensor signal conditioning circuit. Address RF rectification**

**Q15.23: Design a precision capacitive sensor conditioning circuit for displacement measurement**

**Q15.24: Design a precision Hall effect sensor signal conditioning circuit with temperature compensation**

**Q15.25: Design a complete precision data acquisition system with self-calibration capability**

---

# MIXED-SIGNAL & PCB DESIGN

### BASIC LEVEL

**Q16.1: What is "mixed-signal"? Why is it challenging?**

**Q16.2: Explain the difference between analog ground and digital ground**

**Q16.3: What is a "star point"? Why do you need it?**

**Q16.4: Why do you separate power and ground planes?**

**Q16.5: What is impedance in a PCB trace?**

### INTERMEDIATE LEVEL

**Q16.6: Design a PCB layout for a mixed-signal board. Show ground and power plane strategy**

**Q16.7: Explain the importance of component placement in high-speed circuits**

**Q16.8: What is "trace impedance"? How do you calculate it?**

**Q16.9: When is controlled impedance important? What traces need it?**

**Q16.10: What is "EMI" and "RFI"? How do you prevent it?**

**Q16.11: Explain differential pair routing. Why is it used?**

**Q16.12: What is "signal integrity"? What causes degradation?**

**Q16.13: Design a PCB with high-speed digital and sensitive analog circuits on the same board. Show isolation strategy**

### ADVANCED LEVEL

**Q16.14: Design the ground plane strategy for a multi-layer PCB with digital, analog, and power sections**

**Q16.15: Analyze via placement on PCB. When is via shielding needed?**

**Q16.16: Explain crosstalk between adjacent traces. How do you minimize it?**

**Q16.17: Design the decoupling network for a high-speed microcontroller. Choose capacitor values and placement**

**Q16.18: Explain skin effect in PCB traces. At what frequency is it significant?**

**Q16.19: Design the layout for a mixed-signal data acquisition board with ADC and DAC**

**Q16.20: Analyze the thermal design for a board with high-power components. Calculate temperature distribution**

### EXPERT LEVEL

**Q16.21: Design a controlled-impedance multilayer board for 500MHz+ digital signals**

**Q16.22: Analyze the power distribution network (PDN) for a high-current digital IC. Calculate resonances**

**Q16.23: Design EMI shielding strategy for a product with RF and precision analog sections**

**Q16.24: Design the thermal management system for a high-power switching power supply on PCB**

**Q16.25: Analyze and mitigate substrate noise coupling in an analog IC on a mixed-signal board**

---

# REAL-WORLD TROUBLESHOOTING

### BASIC LEVEL

**Q17.1: Your amplifier oscillates at 100MHz. What's the most likely cause?**

**Q17.2: A DC voltage measurement shows ±50mV noise. What could cause this?**

**Q17.3: An op-amp output is stuck at 5V. What do you check first?**

**Q17.4: A power supply won't regulate. What are possible causes?**

**Q17.5: A temperature sensor reads inconsistent values. What could be wrong?**

### INTERMEDIATE LEVEL

**Q17.6: Your circuit works on the bench but fails in the field. What environmental factors could be involved?**

**Q17.7: A precision amplifier has 100mV DC offset that shouldn't be there. Debug procedure?**

**Q17.8: A switching power supply is making noise in nearby electronics. What's the issue?**

**Q17.9: Your sensor measurement drifts over temperature. Is it the sensor or the circuit?**

**Q17.10: A circuit works at 25°C but fails at 80°C. What could be temperature-dependent?**

**Q17.11: An ADC reading is noisy but within spec. How do you verify the noise?**

**Q17.12: Your circuit draws 10x more current than expected. What are energy hogs?**

**Q17.13: A precision reference voltage is drifting. What affects voltage references?**

### ADVANCED LEVEL

**Q17.14: Design a troubleshooting strategy for an unstable feedback system. What measurements prove instability?**

**Q17.15: A high-speed digital system fails when an analog measurement circuit is added. Why?**

**Q17.16: Your circuit fails intermittently under specific environmental conditions. Debug approach?**

**Q17.17: Power supply regulation is poor under dynamic loads. How do you fix it?**

**Q17.18: A precision measurement system has calibration drift. Where are the failure points?**

**Q17.19: Explain how to use an oscilloscope to diagnose frequency response problems**

**Q17.20: A thermal sensor has nonlinear error. How do you diagnose: sensor vs circuit?**

### EXPERT LEVEL

**Q17.21: Use Fourier analysis to diagnose the exact harmonic distortion in an amplifier**

**Q17.22: Design a test procedure to validate all specifications of a precision ADC**

**Q17.23: Diagnose and fix a subtle EMI coupling issue between digital and analog sections**

**Q17.24: Design a reliability test for power supply performance under extended conditions**

**Q17.25: Develop a comprehensive test plan to validate a precision data acquisition system**

---

# ADVANCED TOPICS

### PRECISION ANALOG DESIGN

**Q18.1: What is the difference between "accuracy" and "precision"?**

**Q18.2: Explain systematic errors vs random errors in measurements**

**Q18.3: What is calibration? Why is it necessary?**

**Q18.4: Explain offset error, gain error, and linearity error**

**Q18.5: How do you measure and correct offset error in an ADC?**

**Q18.6: Design a precision measurement system that maintains ±0.1% accuracy over -40 to +85°C**

**Q18.7: Explain the effect of reference voltage drift on overall measurement accuracy**

**Q18.8: Design a self-calibrating measurement system**

**Q18.9: What is "ratiometric measurement"? When is it useful?**

**Q18.10: Design a precision delta-sigma ADC signal path. Address noise and settling time**

---

### NOISE IN ANALOG CIRCUITS

**Q19.1: What are the types of noise: thermal, shot, flicker?**

**Q19.2: Calculate thermal noise in a 10kΩ resistor at room temperature over 100kHz bandwidth**

**Q19.3: What is "noise figure" of an amplifier?**

**Q19.4: Explain 1/f (flicker) noise. At what frequencies is it significant?**

**Q19.5: Design a low-noise preamplifier for a 100pA photodiode signal**

**Q19.6: What is "noise factor"? How does it differ from noise figure?**

**Q19.7: Calculate the SNR of a precision analog system with multiple noise sources**

**Q19.8: Design a noise budget for a precision measurement system**

**Q19.9: Explain how to measure noise in a circuit using FFT analyzer**

**Q19.10: Design a precision low-noise current measurement circuit for 1nA signals**

---

### NONLINEARITY & DISTORTION

**Q20.1: What is "total harmonic distortion" (THD)?**

**Q20.2: How do you measure THD?**

**Q20.3: What causes nonlinearity in amplifiers?**

**Q20.4: Explain "intercept point" (IP3, IP2)**

**Q20.5: Design a precision low-distortion amplifier. Target THD < 0.1%**

**Q20.6: What is "intermodulation distortion" (IMD)?**

**Q20.7: Calculate IP3 from measured intermodulation products**

**Q20.8: Explain harmonic distortion in a switching power supply and mitigation**

**Q20.9: Design a precision audio amplifier with THD < 0.01% at 1kHz**

**Q20.10: Analyze and minimize nonlinearity in a precision logarithmic amplifier**

---

### INTEGRATED CIRCUIT DESIGN CONCEPTS

**Q21.1: Explain why BJTs are used in precision analog ICs vs discrete transistors**

**Q21.2: What is "matching" in analog ICs?**

**Q21.3: How do you design precision current mirrors on an IC?**

**Q21.4: Explain the "subthreshold region" of MOSFETs**

**Q21.5: What is "substrate noise"? How does it couple to circuits?**

**Q21.6: Design a precision bandgap reference circuit. Explain temperature compensation**

**Q21.7: Explain why layout is critical for precision analog ICs**

**Q21.8: Design a precision logarithmic amplifier on an IC**

**Q21.9: What is "process corner"? Why does it matter?**

**Q21.10: Explain the tradeoff between accuracy, speed, and power in analog IC design**

---

### SPECIAL CIRCUITS & APPLICATIONS

**Q22.1: Design a precision waveform generator (sine, triangle, square wave)**

**Q22.2: Design a precision audio amplifier with preamp + power amp sections**

**Q22.3: Design a precision precision precision AC signal measurement circuit (RMS)**

**Q22.4: Design a precision impedance analyzer circuit**

**Q22.5: Design a precision function generator with 100Hz to 100kHz range**

**Q22.6: Design a precision capacitance measurement circuit**

**Q22.7: Design a precision inductance measurement circuit**

**Q22.8: Design a precision conductivity measurement system for fluid sensing**

**Q22.9: Design a precision lock-in amplifier for low-level signal detection**

**Q22.10: Design a precision parametric amplifier (gain by energy pump)**

---

### SYSTEM-LEVEL DESIGN

**Q23.1: Design a complete 12-bit data acquisition system for 8 analog channels**

**Q23.2: Design a precision 16-channel temperature monitoring system**

**Q23.3: Design a precision 4-channel current measurement system with 16-bit precision**

**Q23.4: Design a precision audio system with microphone preamp + ADC + DSP + DAC + headphone amp**

**Q23.5: Design a precision medical patient monitor with ECG, SpO2, and respiration measurement**

**Q23.6: Design a precision industrial process control system with feedback**

**Q23.7: Design a precision sensor interface for wireless data transmission**

**Q23.8: Design a precision IoT sensor node with power optimization**

**Q23.9: Design a precision battery management system for multi-cell lithium battery**

**Q23.10: Design a precision uninterruptible power supply (UPS) system**

---

### FAILURE ANALYSIS & RELIABILITY

**Q24.1: What causes capacitor failures? How do you prevent them?**

**Q24.2: Explain "electromigration" in PCB traces. At what current density does it occur?**

**Q24.3: What is "solder joint fatigue"? How do you design for reliability?**

**Q24.4: Explain the "bathtub curve" of reliability**

**Q24.5: Design a robust power supply for extended -40 to +125°C temperature range**

**Q24.6: What is "thermal cycling"? How does it damage circuits?**

**Q24.7: Design a circuit to survive "electrostatic discharge" (ESD)**

**Q24.8: Explain "latch-up" in CMOS circuits. How do you prevent it?**

**Q24.9: Design redundancy into a critical analog measurement system**

**Q24.10: Develop a comprehensive reliability test plan for a precision sensor**

---

### MEASUREMENTS & INSTRUMENTATION

**Q25.1: How do you measure frequency response of a circuit accurately?**

**Q25.2: How do you measure phase shift at a specific frequency?**

**Q25.3: Explain "oscillator phase noise". Why is it important?**

**Q25.4: Design a precision phase measurement system for 100MHz signals**

**Q25.5: How do you measure differential-mode vs common-mode noise?**

**Q25.6: Design a precision impedance measurement system from 1Hz to 1MHz**

**Q25.7: How do you measure very high impedances (GΩ range) accurately?**

**Q25.8: Design a precision ultra-low current measurement system (picoamp range)**

**Q25.9: Explain "group delay" measurement. Why is it important for analog filters?**

**Q25.10: Design a comprehensive test system for validating precision analog components**

---

## SUMMARY MATRIX

┌─────────────────────────────────────────────────────────────────┐
│ QUESTION DIFFICULTY DISTRIBUTION                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ BASIC (Fundamentals):        Questions 1-100                   │
│ ├─ Passive components         Q1-Q8                            │
│ ├─ Semiconductor basics       Q4-Q8                            │
│ ├─ Op-amp fundamentals        Q9-Q10                           │
│ └─ Simple circuits            ~25 questions each section      │
│                                                                 │
│ INTERMEDIATE (Application):  Questions 101-225                 │
│ ├─ Component selection        Various questions                │
│ ├─ Basic circuit design       Various questions                │
│ ├─ Frequency analysis         Various questions                │
│ └─ Simple troubleshooting     Various questions                │
│                                                                 │
│ ADVANCED (Integration):       Questions 226-340                │
│ ├─ Multi-stage systems        Various questions                │
│ ├─ Precision measurement      Various questions                │
│ ├─ Stability analysis         Various questions                │
│ └─ Complex troubleshooting    Various questions                │
│                                                                 │
│ EXPERT (Mastery):            Questions 341-400+               │
│ ├─ IC design concepts         Various questions                │
│ ├─ System optimization        Various questions                │
│ ├─ Reliability engineering    Various questions                │
│ └─ Advanced measurements      Various questions                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

---

## HOW TO USE THIS DOCUMENT

**For Interview Preparation (10-15 mins per session):**

1. **Day 1-3**: Answer 5-10 BASIC questions
   - Focus on conceptual understanding
   - Draw circuits from memory
   - Derive formulas

2. **Day 4-7**: Answer 5-10 INTERMEDIATE questions
   - Design simple circuits
   - Calculate component values
   - Explain tradeoffs

3. **Day 8-14**: Answer 3-5 ADVANCED questions
   - Multi-stage design
   - Frequency response analysis
   - Stability considerations

4. **Day 15+**: Answer 2-3 EXPERT questions
   - System-level design
   - Optimization problems
   - Failure analysis

**For Self-Assessment:**

- **Score 80%+ on BASIC**: Ready for mid-level role
- **Score 70%+ on INTERMEDIATE**: Ready for senior role
- **Score 60%+ on ADVANCED**: Ready for principal engineer role
- **Score 50%+ on EXPERT**: Ready for staff engineer role

**For Teaching/Mentoring:**

- Use BASIC questions for entry-level engineers
- Use INTERMEDIATE for mid-level engineers
- Use ADVANCED for senior engineers
- Use EXPERT for specialized roles

---

## KEY TOPICS TO MASTER

Before your interview, ensure you can:

1. ✓ Draw and explain any passive component
2. ✓ Draw and explain any semiconductor device
3. ✓ Analyze simple transistor amplifier circuits
4. ✓ Design simple op-amp circuits
5. ✓ Calculate frequency response and filter performance
6. ✓ Explain frequency compensation and stability
7. ✓ Design power supplies (linear and switching)
8. ✓ Analyze noise and distortion
9. ✓ Design signal conditioning for sensors
10. ✓ Understand PCB layout for analog circuits
11. ✓ Troubleshoot real-world circuit problems
12. ✓ Explain tradeoffs in design decisions
