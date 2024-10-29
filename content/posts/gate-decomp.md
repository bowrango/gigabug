+++
title = 'quantum gate decomposition'
date = 2024-09-12T00:15:22-04:00
draft = false
+++

This is my attempt to decompose an arbitrary N-qubit unitary using the minimum number of CX gates. It's been my puzzle since I wrote [unitaryGate](https://www.mathworks.com/help/matlab/ref/unitarygate.html), which implements the best known method that uses twice the lower bound. This work explores my thoughts on improving the algorithm. To my knowledge there is no general algorithm for arbitrary matrices using the minimum number of CX gates.

Quantum circuits are executed using the primitive operations of the quantum computer. The matrices of this primitive set form a basis of the unitary group $U(2^N)$. Any 1 qubit rotation, a unitary matrix in U(2), can be decomposed into 3 rotations like Euler rotations. The figure below shows the smallest circuit for any 2-qubit unitary in the basis {CX,RY,RZ}. The circuit uses 3 2-qubit CX gates and 15 1-qubit rotation gates.

![abc](/circuit.png)

The Quantum Shannon Decomposition (QSD) is the best known general method, using about twice the lower bound. It uses SVD and EIG to recursively factor out single qubit rotations at each step. The 1- and 2-qubit unitary matrices at the leaf nodes are handled using the algorithms above.

![abc](/qsd.png)

The focus will be on the uniform controlled rotation (UCR) gates indicated by $R_Z$ and $R_Y$. The [unitaryGate](https://www.mathworks.com/help/matlab/ref/unitarygate.html) function can remove 1-qubit rotation gates with small angles to allow CX gates to cancel. This gives an informal depth-accuracy tradeoff, and motivates my idea towards a minimum CX gate count and approximate error bound. I'd like to bound the error on an approximate decomposition based on the user provided angle threshold, hoping that leads to an exact decomposition with some insight how to make the angles sparse. Two papers to help solidify this idea:

1. [This](https://arxiv.org/pdf/2106.05649) compresses QSD down to the lower bound using iterative group Lasso regression. Forcing rotation angles to 0 is intuitive but the optimization is expensive. This research is formalized mainly in the language of Lie theory as a recursive sequence of Cartan decompositions. They consider different templates for hardware topology.

2. [This](https://arxiv.org/abs/2205.00081) proves an upper error bound on approximately block encoding a target matrix $A$ using UCR gates. It shows that for common matrices the error is well below the bound.

We learn from (1.) that sparse angles can make QSD optimal through optimization. This hints towards a direct approach as a proof of concept. I apply the main result of (2.) to the UCR gates at each recursive step of QSD. The circuit for block encoding a real matrix $A$ is below. The proof starts by giving the error on the matrix oracle $O_A$ from thresholding rotation angles θˆ with cutoff δc,

$$
δθ = θ ̃ − θ = ( Hˆ(⊗2n) P G ) δ θˆ ,
$$

![abc](/block-encode2.png)
