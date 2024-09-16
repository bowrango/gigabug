+++
title = 'workbench'
date = 2024-09-04T23:10:07-04:00
draft = true
+++

I wanted to make an analog computer so first I had to make an electronics workbench. Now I don't have an analog computer but instead a cool workbench.

The goal was to reproduce the simplist possible Stochastic Processing Unit (SPU) to learn about various ways physics can be used as a compute medium. Curiosity followed Normal Computing and their development of "Thermodynamic AI". The unit uses coupled electronic oscillators and leverages noise. Digital compilation and data loading/readout are the major SPU overheads

![abc](/normal-circuit.png)

This blog documents my experience replicating the paper [1]. I didn't get too far.

I'd read Zach's blog for a great introduction on energy-based computing and players in the field. Building coupled oscillators on a breadboard is the simpliest hardware to work with. But scaling is fundamentally challenging because inductors and capacitors are hard to shrink. Alternatively, oscillators using traditional CMOS logic gates can fit on a ~1 mm2 chip [3][4]. Maybe after the home lab I'll make a home fab.

First a note on quantum. The quantum approximate optimization algorithm (QAOA) alternates cost and mixer operations to move from ground state of mixer to ground state of cost, yielding candidate solutions. The application times of these operations to transition the state are learned, and increasing the number of repetitions improves the approximation. It can be viewed as a Trotterized ("discretized") version of quantum annealing with a parametrized annealing pathway [2]. We're learning the bitter lesson. I've been reading Normal Computing papers, and saw how the Extropic founders transitioned away from quantum. I'll continue to work on quantum computing software and experiment with these other paradigms as an oppertunity to get closer to hardware.

I have the basic electrical equipment and need to diagram the whole setup. I'll use Diffrax to model the SDEs of the circuit and compare hardware fluctuations [5] to numerical simulation. The schematic of a single node doesn't look too complicated but coupling the whole system is messy. Best to start as simple as possible but not any simpler.

Labview transmits the user-defined digital potentiometer values to an Arduino Uno board. The Arduino board then transmits the digital I2C resistors values to the weights. The analog oscillator voltage signals are monitored by the Labview program with a data acquisition board, at a maximum sampling rate of 500 kHz.

Python on the host PC can send target signals to waveform generator via LAN and interface with Arduino. Diffrax can leverage the onboard 2x NVIDIA A1000 GPUs.

# Materials

Power supply, waveform generator, oscilliscope, multimeter
Capacitors, inductors, resistors, transistors, wires
Differential summing amplifier (Texas Instruments THS4140)
Digital potentiometers (Analog Devices AD5272)

# References

[1] Analog Coupled Oscillator Based Weighted Ising Machine
[2] Ising machines: Hardware solvers for combinatorial optimization problems
[3] An Ising solver chip based on coupled ring oscillators with a 48-node all-to-all connected array architecture
[4] A 1,968-node coupled ring oscillator circuit for combinatorial optimization problem solving
[5] Thermodynamic AI and the fluctuation frontier