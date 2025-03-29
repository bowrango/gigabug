+++
title = 'bug8'
date = 2025-03-29T13:44:15-04:00
draft = false
+++

The bug8 device will solve 8-variable binary combinatorial problems consuming less than 1 W of power. It contains 8 voltage oscillators connected with custom programmable resistor network.

The user uploads their problem as the matrix $$\mathbf{J}$$. Consider an example MaxCut graph problem. It can be encoded with $\mathbf{J}=-\mathbf{A}$ of the graph adjacency matrix $\mathbf{A}$. A Lyapunov function exists for the phase macro-model that minimizes (1) when the phases are locked to 0 and $\pi$. This is achieved by perturbing the oscillators with a small injection signal at twice the fundamental frequency.

TODO Simulation here