+++
title = 'thoughts on quantum'
date = 2024-09-04T23:10:07-04:00
draft = false
+++

Some perspective on quantum computing, and how I got interested in the adjacent field of thermodynamic computing. 

Quantum computing either looks like textbook algorithms (Shor, HHL, AA, etc.), or mimics deep learning. The former are direct algorithms with their own theoretical advantage, and the latter focuses on training parameterized models where advantage is fuzzy. Of course, there is much research exploring ways to combine these approaches. However, since textbooks algorithms are too large for modern hardware, these smaller parameterized models are hoped to learn despite noise. I wrote a [basic example that trains a quantum model to predict MNIST digits](https://github.com/mathworks/Quantum-Computing-MATLAB/tree/main/examples/machine-learning/classifiers/mnist). Training quantum models in this way opens a can of worms. My point being, does this even make sense? Why should we expect quantum models to be more expressive than classical models, especially for non-quantum data like digits or whatever.

In the gate-based model of quantum computation, the algorithm is composed of discrete unitary operations, commonly represented as a directed acyclic graph. The target hardware can only execute certain operations so optimization passes modify the graph according to various rewrite rules. A scheduler then compiles the discrete gates into pulse sequences driving the qubits. There are many complications in the stack and often these routines are very expensive and rely on heuristics. It is ironic how NP-Hard combinatorial problems are a candidate for quantum methods but (optimal) qubit-routing on the hardware is itself an NP-Hard task. Quantum computing software of the future will abstract away gate operations, just as all modern languages do for bit operations.

The quantum approximate optimization algorithm (QAOA) is a heuristic based on the adiabatic theorem. It basically applies alternating cost and mixer gate operations to slowly transition from a known ground state to the ground state of the cost operation, yielding candidate solutions. The operations have learned time parameters, and increasing the number of alternations improves the approximation. Originally it was shown to give a better approximation ratio than any polynomial-time classical algorithm, until better classical algorithms came out. [Scott Aaronson wrote a recent update on this](https://scottaaronson.blog/?p=8375). The algorithm can be viewed as a Trotterized ("discretized") version of quantum annealing with a parameterized annealing pathway [2]. It all seems roundabout, as if we're learning the bitter lesson, but I try and remain optimistic.

I've been reading papers from Normal Computing, and saw how the Extropic founders transitioned away from quantum. My goal was to reproduce a simple stochastic processing unit to learn about other paradigms in which physics can be used to compute. The image belows shows a prototype unit built by Normal Computing. It basically uses analog coupled electronic oscillators that exploit thermal noise, which they leverage for solving linear algebra problems. Checkout their library [thermox](https://github.com/normal-computing/thermox).

![abc](/normal-circuit.png)

I planed to replicate [1] but didn't get too far. I'd read Zach's blog for a great introduction on energy-based computing and players in the field. Building coupled oscillators on a breadboard is the simpliest hardware to work with. But scaling is fundamentally challenging because inductors and capacitors are hard to shrink. Alternatively, oscillators using traditional CMOS logic gates can fit on a ~1 mm2 chip [3][4]. Maybe after the home lab I'll make a home fab.

I have the basic electrical equipment and need to diagram the whole setup. I'll use Diffrax to model the SDEs of the circuit and compare hardware fluctuations [5] to numerical simulation. The schematic of a single node doesn't look too complicated but coupling the whole system is messy. Labview transmits the user-defined digital potentiometer values to an Arduino Uno board. The Arduino board then transmits the digital I2C resistors values to the weights. The analog oscillator voltage signals are monitored by the Labview program with a data acquisition board, at a maximum sampling rate of 500 kHz. Python on the host PC can send target signals to waveform generator via LAN and interface with Arduino.

# References

[1] Analog Coupled Oscillator Based Weighted Ising Machine
[2] Ising machines: Hardware solvers for combinatorial optimization problems
[3] An Ising solver chip based on coupled ring oscillators with a 48-node all-to-all connected array architecture
[4] A 1,968-node coupled ring oscillator circuit for combinatorial optimization problem solving
[5] Thermodynamic AI and the fluctuation frontier