+++
title = 'ising machine'
date = 2024-10-31T20:04:35-04:00
draft = false
+++

# Overview

Ising machines are novel analog computers designed to solve combinatorial optimization problems and have general machine learning applications. The Ising model is a mathematical model from statistical thermodynamics that doesn't describe a real physical system. However, as an abstract energy function, it becomes useful for constructing natural physics-based solvers. The energy function, known generally as the Hamiltonian, is
$$
H(s) = \frac{-s^{T}Js}{2}
$$
where vector s is the discrete spin variables and matrix J is the interaction graph. Each spin variable can be +1 or -1 and $J(i,j)$ is the coupling strength between spin i and spin j. Finding the configuration of spins with the lowest energy is difficult. In fact, finding this configuration is equally difficult as problems that are NP-Hard. These kinds of problems span many applications from scheduling, logisitics, finance, etc. 

The matrix representation of the combinatorial problem is mapped to the coupling strengths between the spins.


However, from statistical thermodynamics, we know the probability of a spin configuration at equilibirum follows the Boltzmann distribution

$$
p_{i} = \frac{e^{-H(s_{i}) / k_{b} T}}{\sum_{} e^{-H(s_{j}) / k_b T}}
$$

The probability of each configutation is proportional to its energy. We hope to measure the high quality solutions more often. 

The Ising machine is a device to realize a system that follow this distribution.

The problem is encoded on a physical network of coupled oscillators to leverage the natural convergence to an equilibrium state representing a solution. Many of the classically difficult combinatorial optimization problems (MaxCut, Subset Sum, etc.) can be encoded using a programmable coupling network. These types of problems are ubiquitous in practical applications such as finance, optimal logistics and scheduling, chip routing, and communication networks. As Moore’s Law nears its limits, these physics-based computers become an attractive potential for speed and power advantages over conventional algorithms run on traditional computers. Ising machines are an emerging area of research with fundamental open questions.

This post documents my development of such an Ising machine. I aim to open-source and ship a PCB that plugs into your laptop to solve general combinatorial problems using analog voltage signals. These signals model the spin variables s, and represent continuous probablistic bits. Lyapunov theory is applied to the generalized Kurumoto model to contruct an "energy function" for stability analysis of the network. A control input synchronizes the oscillations and induces locking to binarize the phases. An oscillator network has the natural tendency to minimize dissipated power by minimizing the amount of interactions.

These devices are natural physics-inspired solvers that operate at room-temperature and low power. While promising, their scale and programmability are limited. But they haven't been given enough attention. These machines are not used in practice because modern classic computing has been pushed so far. But as [Moore's law comes to an end](https://ieeexplore.ieee.org/document/7878935) they come back to light.

The Kuramoto model can be formulated as a stochastic differential equation with a coupling and synchronization term. The simulated voltage phases converge to discrete values as the objective value (e.g., MaxCut) increases. Machine learning software frameworks can be built on this primitive. The coupled network evolves to the low-energy steady state.  

![](/kuramoto.png)

**Software**
Development Tools

Macbook
- Research
- MATLAB/C++
- Arduino and IoT prototyping

Windows machine
- Instrument control
- Multisim/KiCAD for PCB design

Linux Server
- ```diffrax```/```CUDA``` for GPU simulations
- ```thermox``` to explore linear algebra primitives

# References

Notes on state-of-the-art

FPGA
- Purdue P
    - Trying to obtain MATLAB code
- DIMPLE
    - Runs on AWS
    - "One important part I haven't thought about is how to write software to leverage this design"

![](/tech-for-ising-machines.png)
![](/phase-lock-devices.png)
![](/mohseniFig1.png)

- State of the art survey
![](/mohseniFig2.png)
![](/mohseniTable1.png)

- MIT Lincoln Lab
- 4 fully-connected nodes with cross bar array of digital pots (AD5272)
- coupling polarity controlled by output polarity of the differential summing amplifier(THS4140)
- no feedback
- get phases from a moving window Fourier transform on time-domain amplitude
- time-to-solution scales with the oscillator frequency
![](/mit-breadboard-lc.png)

-  full adder using cross-coupled CMOS LC oscillators
- transistor arrays (ALD1106/ALD1107), 10m inductors, 6.8nF capacitors for ~10kHz
- SYNC input at 20kHz
- fixed inductors and rotary potentiometers
![](/wangFig334.png)
![](/wangFig331.png)

- 8 grid-connected nodes
- cross-coupled inverters (TI SN74HC04N), fixed 33μH inductors, trimmer capacitors (tuned ~30pF), 5V single supply
- SYNC supplied to GND of inverter chips
- manually plug in resistors and potentiometers for different problems
- why are pots only used for diagonal edges?
- fixed inductors and rotary potentiometers
- read the oscillator phases using xor gates (TI SN74HC86N)
- 5MHz
![](/wangFig480.png)
![](/wangFig8.png)

- 32 Chimera-connected nodes
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- rotary potentiometer for coupling; each one has male pin connectors to set polarity by shorting different pins
- 5MHz
![](/wangFig490.png)

- 64 grid-connected nodes; 4 per PCB
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- digital potentiometer (AD5206) for coupling; each one uses one channel and single pole double throw on-off-on switch for polarity
- 192 couplings?
- 1MHz
![](/wangFig414.png)

- 240 nodes; 12 dense-connected per PCB with 1200 couplings
- cross-coupled inverters (TI SN74HC04N), fixed inductors, trimmer capacitors, 5V single supply
- digital potentiometer (AD5206) for coupling; uses wiper position to switch polarity and eliminate switches
- motherboard edge connects 20 PCBs in 4-by-5 toroidal grid
- 1MHz
- ∼5W for all nodes and peripheral circuitry, excluding ony LEDs
![](/wangFig415.png)

- Normal Computing (2023)
- (weight diffuser) ↔ (diffusion process) ↔ (s-mode device)
- (posterior drift network) ↔ (score network) ↔ (Maxwell’s demon device)
- (auxiliary SDE) ↔ (s-mode device)
- (optimization ODE) ↔ (latent variable evolution in Maxwell’s demon device)
- analog or digital-analog score network architecture for less latency
- Thought I saw a green chip at some point
![](/normal-chip-2023.png)
![](/normal-roadmap.png)
![](/normal-hardware.png)

- simulates wave-digital system of equations (MATLAB) 
- bread boards are used with
- "we observe higher power dissipation, when SHIL is used. However, since anti-phase synchronization still minimizes power dissipation, we see that the oscilla- tors assume this states nonetheless. This phenomenon is very important and has not been pointed out in litera- ture so far."
![](/beattieFig17.png)
![](/beattieFig3.png)

- 1440 nodes with 11,724 couplings; 2584 are freely routable, rest are locally fixed
- MATLAB for test management, OIM configuration, timing scheduling, and analysis
- AMD Threadripper Pro 5965WX 24-core, 256GB RAM took ~4.67 hours per problem
![](/graberFig2c.png)
![](/graberFig4.png)
![](/graberTable1-2024.png)

- 0.878-approx for maxcut with log computation time scaling
- OP-amp Schmidt trigger oscillator
![](/delacourTable1.png)
![](/delacourTable3.png)
![](/delacourFig5.png)

- Scalable ring-oscillator-based integrated circuit
![](/moyTable1.png)
![](/moyFig2.png)

- Proposed system is intended to provide a flexible experimental platform for further research of the internal mechanisms.
![](/graberTable1.png)
![](/graberFig8.png)

- Proposes "tanh-make-break" annealing heuristic; incoming current at each node is related to its M and B
- Allows multiple nodes to flip in parallel without synchronization
- Supports cubic interactions
![](/sharmaFig3.png)

- Uses intrinsic noise in memristor Hopfield neural network
![](/caiFig1.png)
![](/caiFig3.png)
![](/caiTable1.png)

- Coupled ring oscillator (ROSC)-based scalable probabilistic Ising computer
![](/ahmedFig10.png)
![](/ahmedTable1.png)

- Spin operator (processing local memory) which provides coefficients with
expandable bit width and fast parallel spin updates according to the Gibbs distribution
- Low-latency inter-chip interface connecting two Ising chips
![](/takemotoFig267.png)
![](/takemotoFig467.png)

