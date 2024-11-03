+++
title = 'thoughts on quantum software'
date = 2024-09-04T23:10:07-04:00
draft = false
+++

Software design for quantum computing and thermodynamic hardware.

Modern quantum computing software follows deep learning and wants to train models. The model learns a parameterized quantum state that minimizes a cost expectation. The variational quantum eigensolver (VQE) is the dubbed algorithm for this procedure. The cost Hamiltonian is a Hermitian matrix, expressed as a weighted sum of projective measurements. The model is a parameterized quantum circuit $U$, and optimized to find the eigenstate 
$$
\psi(\theta) = U(\theta)| 0 \rangle 
$$
of smallest eigenvalue. The goal is therefore to minimize the expectation value 
$$
cost(\theta) = \langle \psi(\theta) | \hat{H}_C | \psi(\theta) \rangle
$$
The circuit structure is problem inspired or analgous to deep learning architectures. Techniques for encoding classical data into quantum circuits, typically as gate rotation angles, [can be expressed by Fourier series](https://arxiv.org/pdf/2008.08605) and sees extensive research. 

There are three main types of conventional textbook quantum algorithms with theoretical advantages:
- Fourier (qft, phase estimation, period finding)
- Search (grover)
- Simulation (chemistry)

Where does this modern framework fit in? 

Consider the adiabatic evolution 
$$
\hat{H}(t) = s \hat{H}_C + (1 - s) \hat{H}_M
$$
with a schedule function $s$. If H(t) changes "slowly" with t, the state evolves adiabatically (stays in ground state of the instantaneous Hamiltonian) and transitions to the ground state of the cost.
Quantum annealing hardware uses a transverse field Ising model.

This process can be emulated on a quantum gate-based hardware by discretizing
$$
U = e^{-i(A + B)t} = \left( e^{-iAt/p} e^{-iBt/p} \right)^p
$$
in small steps. This is known as a Trotter decomposition. Here enters the quantum approximate optimization algorithm (QAOA). It is a special case of VQE with a circuit structure applying $U$ above. The algorithm learns a parameterized quantum state
$$
|\psi(\gamma, \beta)\rangle = e^{-i\beta_p \hat{H}_M} e^{-i\gamma_p \hat{H}_C} \cdots e^{-i\beta_1 \hat{H}_M} e^{-i\gamma_1 \hat{H}_C} |s\rangle.
$$
to minimize the expectation value. The learned parameters $\gamma$ and $\beta$ are application times for the cost and mixer operations. The number of repetitions $p$ truncates the approximation. The algorithm can be viewed as a discretized version of quantum annealing with a parametrized annealing pathway. The model basically learns an adiabatic path evolution from a known ground state to the ground state of the cost Hamiltonian, and upon measurement yields candidate solutions to the original (binary) optimization problem.

But the destination is more important than the journey, I think. It is not fundamentally necessary for the model to learn the path in order to solve the optimization problem. Or rather, we should allow physics to find the path instead. [Scott Aaronson put out a recent blog about](https://scottaaronson.blog/?p=8375). The trouble with QAOA, and more generally "quantum machine learning", is  

These models are trained unsupervised and suffer from barren plateaus in the cost function landscape.
I wrote a basic XOR classification model using parameter-shift to compute gradients.

What are the hardware implications of training models like this?

In the gate-based model of quantum computation, the algorithm is composed of discrete unitary operations, commonly represented as a directed acyclic graph. The target hardware can only execute certain operations so optimization passes modify the graph according to various rewrite rules. A scheduler then compiles the discrete gates into pulse sequences driving the qubits. There are many complications in the stack and often these routines are very expensive and rely on heuristics. It is ironic how NP-Hard combinatorial problems are a candidate for quantum methods but (optimal) qubit-routing on the hardware is itself an NP-Hard task. Quantum computing software of the future will abstract away gate operations, just as all modern languages do for bit operations.

For context, read [Zach's blog post](https://www.zach.be/p/whats-the-difference-between-extropic) for a great introduction on energy-based computing and companies in the space. The research from Normal Computing gives a quantum-inspired prespective. I investigated their work, and wanted to reproduce my own simple stochastic processing unit. The image belows shows a prototype built by Normal Computing. It uses coupled analog electronic oscillators that exploit thermal noise for solving linear algebra problems from a statistical thermodynamics approach. Checkout their library [thermox](https://github.com/normal-computing/thermox).

![abc](/normal-circuit.png)

I have all the electrical equipment to prototype this on a breadboard. I'll try Diffrax to model the SDEs of the circuit and compare hardware fluctuations to numerical simulation. The schematic of a single node doesn't look too complicated but coupling the whole system is messy. Labview transmits the user-defined digital potentiometer values to an Arduino Uno board. The Arduino board then transmits the digital I2C resistors values to the weights. The analog oscillator voltage signals are monitored by the Labview program with a data acquisition board, at a maximum sampling rate of 500 kHz. Python on the host PC can send target signals to waveform generator via LAN and interface with Arduino.

Perhaps quantum computing will become commercially viable once I catch my ambitions.