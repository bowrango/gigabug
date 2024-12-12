+++
title = 'ising machine'
date = 2024-10-31T20:04:35-04:00
draft = false
+++

An "Ising machine" is a physic device to solve NP-complete combinatorial problems mapped to the Ising model

power grid -> scheduling problem -> combinatorial objective -> Ising machine -> coupled oscillators -> power grid

complex systems are self-similar at across time and length scales

This is my design moodboard for an electric oscillator network to study probabilistic bits. Probably photonics are end game, but a breadbroad is shippable.

This project is particularly inspired by [current research](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-12pdf). Lyapunov theory is applied to the generalized Kurumoto model to contruct an "energy function" for stability analysis of chaotic coupled oscillators. A control input synchronizes the oscillations to a "low energy" steady state, and induces locking to binarize the phases.

In-memory computation. Different fundamental architecture
Spread in oscillators natural frequency contributes a linear term in the global Lyapunov function

Direct SPICE-level simulation of network is inefficient and inaccurate.
How to determine gain schedule and sync input? 

- Waveform generator and oscilliscope interface with MATLAB. It can generate and flash C++ to the Arduino. 
- Use ```diffrax``` and ```thermox``` for GPU acceleration. Jobs will be sent to local server.

Code for this project is on [my Github](https://github.com/bowrango/ising-machine). The maxcut value improves when the node voltage signals are phase locked.
![](/kuramoto.png)
![](/phase-lock-devices.png)

Quality factor and PPV analysis

Arduino needs to control many potentiometers -> expand digital IO
- SPI port expander (MCP23S17); takes MOSI, MISO, SCLK, and a CS
- 3 hardware pins for 8 devices on bus

### Existing PCBS

- there is a lot out there
![](/tech-for-ising-machines.png)

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
- Normal Computing ... thought I saw a green chip at some point

![](/matlab-ising-pcb.png)
![](/clk-freq-impact.png)
![](/oim-comparison-tbl.png)
- 1440 nodes, 11,724 couplings; 2584 are freely routable, rest are locally fixed
- MATLAB for test management, OIM configuration, timing scheduling, and analysis
- AMD Threadripper Pro 5965WX 24-core, 256GB RAM took ~4.67 hours per problem

![](/mit-breadboard-lc.png)
![](/ising-projection.png)
![](/circ-graph-comparison.png)
- MIT Lincoln Lab
- 4 fully-connected nodes with cross bar array of digital pots
- no feedback
- get phases from a moving window Fourier transform on time-domain amplitude
- time-to-solution scales with the oscillator frequency

The injection locking signal, and its application in the context of LC-oscillator systems is mathematically similar to the case of the degenerate optical parametric oscillator used in previous optical Ising machines12,13, where optical pump pulses at twice the optical oscillation frequency are used to create binary phase values. The phase of pump and signal should be locked for amplitude (Figure from Stanford CIM). 
![](/phaselock.png)