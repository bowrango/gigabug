+++
title = 'bug8'
date = 2025-03-29T13:44:15-04:00
draft = false
+++

We're researching the dynamics of driven stochastic electronics to solve combinatorial optimization problems. This physics-based approach has the potential to be orders of magnitude more power efficient than classical combinatorial algorithms to enable low-power edge-compute for a variety of applications.

![bug8](/bug8.png)

The bug8 is a proof-of-concept research platform for novel analog computing systems. It contains 8 electronic voltage oscillators (top board) connected with a programmable resistive network (bottom board). It's primarily designed for combinatorial optimization of binary variables. It uses a standard ESP32 microcontroller with a custom low-level Python/C++ API and has many testpoints to analyze hardware characteristics. The device finds an optimal binary solution
\begin{equation}
    \mathbf{x}^* = \arg\min_{\mathbf{x} \in ({0, 1})^N} x^{T}\mathbf{J}x
\end{equation}
to the problem matrix $\mathbf{J}$ using phase dynamics of the coupled electronic oscillators. In fact, [these dynamics actually perform Lagrange Multiplier optimization](https://arxiv.org/pdf/2007.05882). This simple quadratic function represents some of the most difficult problems for modern computers and is extensible to probabilistic energy-based models. It brings compute closer to the hardware.

Each binary variable of the input problem graph is represented by the phase of an analog LC oscillator. The oscillators are fully-connected with a programmable resistive network to encode the $\mathbf{J}$ matrix coefficients.

![](/schematic.png)

This principle can be demonstrated with a simple breadboard experiment. In this example, 10K ohm resistors connect the variable $x_3$ (blue) to $x_1$ (yellow), $x_2$ (purple), and $x_4$ (green) which themselves share no connections. Probes measure the voltage waveform at the inductor at each oscillator. The oscilloscope shows the set of variables $\{x_1, x_2, x_4\}$ are mis-aligned with $\{x_3\}$, which indicates the optimal solution $\mathbf{x}^{\*}=[1, 1, 0, 1]$ or equivalently $\mathbf{x}^{\*}=[0, 0, 1, 0]$. In the context of the [MaxCut](https://en.wikipedia.org/wiki/Maximum_cut) problem, this corresponds to "cutting all the resistors" which forms the optimal partition for this graph. The natural circuit dynamics perform the computation "for free".

![](/experiment.png)

Although the oscillators naturally reach equilibrium, the phase configuration does not necessarily correspond to $\mathbf{x}^*$ for a large system size. This is because their dynamics follow a gradient, which can get stuck in non-global minima. Not all stable points of the system map to global minima of the encoded combinatorial problem. Therefore, an external signal is used to [anneal](https://en.wikipedia.org/wiki/Simulated_annealing) the oscillators and ensure bi-stable configurations, such that a 0 or 1 can be measured. This relies on the principle of subharmonic injection locking, which uses a signal at twice the fundamental oscillator frequency and an amplitude corresponding to the anneal strength. The weighted [Kuramoto](https://www.google.com/search?client=safari&rls=en&q=kuramoto&ie=UTF-8&oe=UTF-8) model, used to represent the phases, admits a [Lyapunov function](https://en.wikipedia.org/wiki/Lyapunov_function) that minimizes $x^{T}\mathbf{J}x$ when they are locked to 0 or $\pi$. 

This analog approach can be significantly more power-efficient than GPUs or TPUs for combinatorial optimization. In practice, classical algorithms use simulated annealing or Monte Carlo methods that [cost about 20 nJ per trial bitflip](https://arxiv.org/abs/1903.11714). However, dedicated ASICs using analog in-memory compute or even FPGAs can achieve 2-5 orders of magnitude less energy per bitflip. The difficulty is scaling the connection network with high-precision weights.

The simulation below shows a larger example of 100 oscillators that almost settle to the ideal solution. This system uses a single scalar ramp function to anneal, but different schedules tend to behave differently for different problems. 

![](/100node.png)

An [optimal path through the space of control parameters](https://journals.aps.org/pre/pdf/10.1103/PhysRevE.109.065301) (e.g., temperature) should minimize dissipation or entropy production. Multidimensional annealing schedules are likely useful for constrained optimization where annealing parameters control constraint penalties. This notion appears in [recent work that develops a classical version of the quantum approximate optimization algorithm](https://arxiv.org/pdf/2507.07420), which effectively learns a discretized annealing pathway. We are expanding these ideas to novel annealing algorithms based on control theory and non-equilibrium statistical mechanics to avoid the heuristic annealing used in other analog and digital circuits. After all, optimizing annealing algorithms and optimizing the control of (thermodynamics) systems are closely related.
