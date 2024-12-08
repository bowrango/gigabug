+++
title = 'ising machines'
date = 2024-10-31T20:04:35-04:00
draft = false
+++

An "Ising machine" is a physic device to solve NP Ising problems

This is my design moodboard for an electric oscillator network to study probabilistic bits.

Its particularly inspired by [current research](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-12pdf). Lyapunov theory is applied to the generalized Kurumoto model to contruct an "energy function" for stability analysis of chaotic coupled oscillators. A control input synchronizes the oscillations to a "low energy" steady state, and induces locking to binarize the phases.

![](/phase-lock-devices.png)

Spread in oscillators natural frequency contributes a linear term in the global Lyapunov function

This is what Normal Computing predicts
![](/normal-roadmap.png)

### Overview and results from another machine

- 1440 oscillators, 11,724 couplings; 2584 are freely routable, rest are locally fixed
- MATLAB for test management, OIM configuration, timing scheduling, and analysis
- AMD Threadripper Pro 5965WX 24-core, 256GB RAM took ~4.67 hours per problem

![](/matlab-ising-pcb.png)

- MIT Lincoln Lab
- breadboard, 4 nodes fully connected with cross bar array of digital pots
- no feedback
- get phases from a moving window Fourier transform on time-domain amplitude
- time-to-solution scales with the oscillator frequency
- voltage amplitudes of each oscillator are shifted by 10 V to help distinguish
![](/mit-breadboard-lc.png)
![](/ising-projection.png)]
![](/circ-graph-comparison.png)]



![](/clk-freq-impact.png)

![](/oim-comparison-tbl.png)


Computation is in-memory

This is also done with photonic oscillators. The phase of pump and signal should be locked for amplitude (Figure from Stanford CIM). 

The injection locking signal, and its application in the context of LC-oscillator systems is mathematically similar to the case of the degenerate optical parametric oscillator used in previous optical Ising machines12,13, where optical pump pulses at twice the optical oscillation frequency are used to create binary phase values.

![](/phaselock.png)

### Software

- Waveform generator and oscilliscope interface with MATLAB. It can generate and flash C++ to the Arduino. 
- Use ```diffrax``` and ```thermox``` for GPU acceleration. Jobs will be sent to local server.

Code for this project is on [my Github](https://github.com/bowrango/ising-machine). The maxcut value improves when the node voltage signals are phase locked.

![](/kuramoto.png)


- 
Direct SPICE-level simulation of network is inefficient and inaccurate.

How to determine gain schedule and sync input? 

### Hardware

Take design inspiration from photonics and current electric chips. Moodboard.

![](/normal-chip-2023.png)


There are two main components

**Oscillator**. 

- Complementary cross-coupled CMOS LC oscillator. Implemented with 
- ~5V Vdd
- OIM8/OIM32 use 33lH inductors with 30pF capacitors for 5MHz natural frequency
- OIM64/OIM240 use digital potentiometer (AD5206), reduced to 1MHz

- cross-coupled inverter (TI SN74HC04N), fixed inductor, trimmer capacitor; XOR gate (TI SN74HC86N ) for readout
- sync at GND to induce locking

**Coupler**. 
-  coupling resistors with conductances Jij/100kΩ
- OIM32/240 chimera, OIM64 grid. Consider if synchronization connection should be dedicated



Quality factor and PPV analysis