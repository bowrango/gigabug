+++
title = 'bug8'
date = 2025-03-29T13:44:15-04:00
draft = false
+++

The bug8 is a printed circuit board containing 8 electronic voltage oscillators connected with a programmable resistive network. It's designed to solve 8-variable binary optimization problems and plugs into your laptop via USB-C.

![bug8](/bug8.png)

This is our initial version and serves as a testbed for experimentation and demonstration of the underlying technology. Using our software interface, the user can upload their problem matrix $\mathbf{J}$ which sets the resistive network. Solutions are obtained by measuring the phases of each oscillator which are sent back to the user. It has many testpoints to easily analyze the phase dynamics to encourage further hardware and algorithmic research. 

Consider an example [MaxCut](https://en.wikipedia.org/wiki/Maximum_cut) problem. It can be encoded with $\mathbf{J}=-\mathbf{A}$ where $\mathbf{A}$ is the graph adjacency matrix. The simulation below shows the evolution of the oscillator phases over time which settle to discrete values corresponding to the optimal solution. A [Lyapunov function](https://en.wikipedia.org/wiki/Lyapunov_function) exists for the oscillator phase macro-model that minimizes $H$ when the phases are locked to 0 or $\pi$. This is achieved by perturbing the oscillators with a small injection signal at twice the fundamental frequency. The MaxCut value and it's corresponding Lyapunov energy are also shown.

![bug8](/maxcutdemo.png)
