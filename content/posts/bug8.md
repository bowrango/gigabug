+++
title = 'bug8 (coming soon)'
date = 2025-03-29T13:44:15-04:00
draft = false
+++

The bug8 device is a printed circuit board containing 8 oscillators connected with a programmable resistive network. Its designed to solve 8-variable binary optimization problems.
Powered via USB-C

![bug8](/bug8_realistic.png)

Inital version
Many testpoints for analysis

Software interface
The user uploads their problem as the matrix $\mathbf{J}$.

Consider an example [MaxCut](https://en.wikipedia.org/wiki/Maximum_cut) graph problem. It can be encoded with $\mathbf{J}=-\mathbf{A}$ where $\mathbf{A}$ is the graph adjacency matrix. A Lyapunov function exists for the oscillator phase macro-model that minimizes $H$ when the phases are locked to 0 or $\pi$. This is achieved by perturbing the oscillators with a small injection signal at twice the fundamental frequency.

TODO Simulation here
