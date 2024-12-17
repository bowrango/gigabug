+++
title = 'ising machine'
date = 2024-10-31T20:04:35-04:00
draft = false
+++

# Overview

The Ising model is of fundamental computational interest because any problem in the 
complexity class NP can be formulated as an Ising problem with polynomial complexity. 

$$
FIXME
H = \sum_{(i, j)} J_{ij}s_i s_j - h \sum_i s_i
$$

where $\textbf{J}$ is the weight matrix and $\textbf{h}$ is the bias. The state probability is then determined by the Boltzmann distribution

$$
P(E) = \frac{e^{-E/k_BT}}{\sum_{i} e^{-E_i / k_B T}}
$$

An Ising machine is a physic device obeying this distribution. It represents probablistic logic and has general applications across optimization and generative machine learning.

Objective (binary) -> Ising Model (spin) -> Ising Machine (voltage) 

This post documents my development of an Ising machine. My plan is to open-source and ship a PCB that plugs into your laptop to solve combinatorial problems. This will encourage further hardware and software research to make these machines competitive.

Quality of obtained solutions remains an open problem. 

This project is particularly inspired by [current research](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-12pdf). Lyapunov theory is applied to the generalized Kurumoto model to contruct an "energy function" for stability analysis of chaotic coupled oscillators. A control input synchronizes the oscillations to a "low energy" steady state, and induces locking to binarize the phases.

Implemented as a network of coupled electric oscillators.
Oscillator network has the natural tendency of minimizing dissipated power by minimizing the amount of interactions between oscillators.

In-memory computation. Different fundamental architecture
Spread in oscillators natural frequency contributes a linear term in the global Lyapunov function

![](/phase-lock-devices.png)

# Ising Machine

Code for this project is on [my Github](https://github.com/bowrango/ising-machine). The simulated Kuramoto oscillator converges to the maxcut when applied with square wave. 
![](/kuramoto.png)

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
There are many physical realizations of Ising-like hardware seen in the literature.
![](/tech-for-ising-machines.png)

These are some particular ones of interest.

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
- Normal Computing
- Thought I saw a green chip at some point

![](/matlab-ising-pcb.png)
![](/clk-freq-impact.png)
![](/oim-comparison-tbl.png)
- 1440 nodes with 11,724 couplings; 2584 are freely routable, rest are locally fixed
- MATLAB for test management, OIM configuration, timing scheduling, and analysis
- AMD Threadripper Pro 5965WX 24-core, 256GB RAM took ~4.67 hours per problem

The injection locking signal, and its application in the context of LC-oscillator systems is mathematically similar to the case of the degenerate optical parametric oscillator used in optical Ising machines. The optical pump pulses at twice the optical oscillation frequency to binarize phases. The phase of pump and signal should be locked for amplitude (Figure from Stanford CIM). 
![](/phaselock.png)