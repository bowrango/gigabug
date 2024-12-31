+++
title = 'ising machine'
date = 2024-10-31T20:04:35-04:00
draft = false
+++

# Overview

An "Ising machine" is hardware to realize the mathematical Ising model, which extends to NP-hard problems in general. 
$$
H(s) = -s^{T}Js + -s^{T}h
$$
This post documents my development of such an Ising machine. I aim to open-source and ship a PCB that plugs into your laptop to solve general combinatorial problems using analog voltage signals. These signals model the spin variables s, and represent continous probablistic bits. Lyapunov theory is applied to the generalized Kurumoto model to contruct an "energy function" for stability analysis of the network. A control input synchronizes the oscillations and induces locking to binarize the phases. An oscillator network has the natural tendency to minimize dissipated power by minimizing the amount of interactions.

There are exciting research oppertunities because these machines are low energy consumption and have vast applications in machine learning and beyond. While promising, provable solution quality remains an open problem which drives the need for algorithmic development (e.g., synchronization scheduling and programmatic coupling). This project is a shippable testbed to encourage this research. I develop quantum computing software for work and want to build electronics in my living room.

# Ising Machine

[This project](https://github.com/bowrango/ising-machine) is in early development. The Kuramoto model can be formulated as a stochastic differential equation with a coupling and synchronization term. The simulated voltage phases converge to discrete values as the maxcut value increases.
![](/kuramoto.png)

In-memory computation. Different fundamental architecture
Spread in oscillators natural frequency contributes a linear term in the global Lyapunov function

Developing locally on Macbook. Simulation jobs will be sent to my local Linux server.

- MATLAB interface to the Arduino, waveform generator, oscilloscope, etc.
- ```diffrax``` for GPU acceleration; I can formulate Kuramoto for ```thermox```
- Multisim, LTSpice for SPICE-level simulation of PCB

**Oscillator**
- Transistor inverting amplifier with feedback (Colpitts/Hartley)
- Cross-Coupled Differential-Pair (SN74HC04N)
- Logic Phase-Locked Loop With VCO (SN74LV4046AN/SN74LV4046ANG4); can use XOR comparator with ref for phase readout

**Programmatic Coupling**
Coupling based on problem encoding. Can't interfere with oscillators. 
Use digital pins to select channel. Use analog A0 to write
Some of these models are deprecated for boardboard
- Digital potentiometer (MCP4251; 2 channel, 8bit, ~$1), (AD5206; 6 channel, 8bit, ~$3)

Two approaches:
- Analog multiplexor (SN74HC138N; 1x3:8, <$1), (CD74HC4067; 1x4:16, ~$1), (CD74HC4515E; 1x4:16, ~$1), (MC74HC154DWR2; 1x4:16, <$1)
- Digital port expander (MCP23S17; SPI, 16 outputs)

**Quality Factor**
Perturbation characteristics of the oscillator. Various definitions across hardware 

**Synchronization Schedule**
Encode problem weights coefficien the tendencies of the network
Map binary optimization problems to synchronization tendencies to encode problem.
Sub-harmonic Injection Locking (SHIL) external signal. Some prototypes converge naturally.

# References

There are many designs in the literature. However, I'm focused on electronic oscillators mainly for their ease of fabrication. Ising machines are a [novel computing architecture](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-12pdf) so there are open questions and strong research oppertunities.

![](/tech-for-ising-machines.png)
![](/phase-lock-devices.png)

![](/mit-breadboard-lc.png)
![](/ising-projection.png)
![](/circ-graph-comparison.png)
- MIT Lincoln Lab
- 4 fully-connected nodes with cross bar array of digital pots (AD5272)
- coupling polarity controlled by output polarity of the differential summing amplifier(THS4140)
- no feedback
- get phases from a moving window Fourier transform on time-domain amplitude
- time-to-solution scales with the oscillator frequency

![](/fig334.png)
- 8 grid-connected nodes
- transistor arrays (ALD1106/ALD1107), 10m inductors, 6.8nF capacitors for ~10kHz
- SYNC input at 20kHz
- fixed inductors and rotary potentiometers

![](/fig480.png)
![](/oscillator-wang.png)
- 8 grid-connected nodes
- cross-coupled inverters (TI SN74HC04N), fixed 33μH inductors, trimmer capacitors (tuned ~30pF), 5V single supply
- SYNC supplied to GND of inverter chips
- manually plug in resistors and potentiometers for different problems
- why are pots only used for diagonal edges?
- fixed inductors and rotary potentiometers
- read the oscillator phases using xor gates (TI SN74HC86N)
- 5MHz

![](/fig490.png)
- 32 Chimera-connected nodes
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- rotary potentiometer for coupling; each one has male pin connectors to set polarity by shorting different pins
- 5MHz

![](/fig414.png)
- 64 grid-connected nodes; 4 per PCB
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- digital potentiometer (AD5206) for coupling; each one uses one channel and single pole double throw on-off-on switch for polarity
- 192 couplings?
- 1MHz

![](/fig415.png)
- 240 nodes; 12 dense-connected per PCB with 1200 couplings
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- digital potentiometer (AD5206) for coupling; uses wiper position to switch polarity and eliminate switches
- motherboard edge connects 20 PCBs in 4-by-5 toroidal grid
- 1MHz
- ∼5W for all nodes and peripheral circuitry, excluding ony LEDs

![](/normal-chip-2023.png)
![](/normal-roadmap.png)
![](/normal-hardware.png)
- Normal Computing
- (weight diffuser) ↔ (diffusion process) ↔ (s-mode device)
- (posterior drift network) ↔ (score network) ↔ (Maxwell’s demon device)
- (auxiliary SDE) ↔ (s-mode device)
- (optimization ODE) ↔ (latent variable evolution in Maxwell’s demon device)
- analog or digital-analog score network architecture for less latency
- Thought I saw a green chip at some point

![](/beattieFig17.png)
![](/beattieFig3.png)
- simulates wave-digital system of equations (MATLAB) 
- bread boards are used with
- "we observe higher power dissipation, when SHIL is used. However, since anti-phase synchronization still minimizes power dissipation, we see that the oscilla- tors assume this states nonetheless. This phenomenon is very important and has not been pointed out in litera- ture so far."

![](/matlab-ising-pcb.png)
![](/clk-freq-impact.png)
![](/oim-comparison-tbl.png)
- 1440 nodes with 11,724 couplings; 2584 are freely routable, rest are locally fixed
- MATLAB for test management, OIM configuration, timing scheduling, and analysis
- AMD Threadripper Pro 5965WX 24-core, 256GB RAM took ~4.67 hours per problem

![](/brim.png)
![](/tmb.png)
- adds support for cubic interactions
- proposes "tanh-make-break" heuristic; incoming current at each node is related to its M and B
- allows multiple nodes to flip in parallel without synchronization 

- DIMPLE
- FPGA emulation of ising machine for FPGA, runs on AWS
- "One important part of this system that I haven't thought about at all is how to actually write software to leverage this design"

The injection locking signal, and its application in the context of LC-oscillator systems is mathematically similar to the case of the degenerate optical parametric oscillator used in optical Ising machines. The optical pump pulses at twice the optical oscillation frequency to binarize phases. The phase of pump and signal should be locked for amplitude (Figure from Stanford CIM). 
![](/phaselock.png)