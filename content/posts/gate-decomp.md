+++
title = 'quantum gate decomposition'
date = 2024-09-12T00:15:22-04:00
draft = false
+++

This is my attempt to decompose an arbitrary N-qubit unitary using minimum number of CX gates. This all came about when I wrote [unitaryGate](https://www.mathworks.com/help/matlab/ref/unitarygate.html), which implements the best known method at twice the lower bound. This work explores my thoughts on improving the algorithm. To my knowledge there is no general algorithm for arbitrary matrices using the minimum number of CX gates.

Quantum circuits are executed using the primitive operations of the quantum computer. The matrices of this primitive set form a basis of the unitary group $U(2^N)$. Any 1 qubit rotation, a unitary matrix in U(2), can be decomposed into 3 rotations like Euler rotations. The figure below shows the smallest circuit for any 2-qubit unitary in the basis {CX,RY,RZ}. The circuit uses 3 2-qubit CX gates and 15 1-qubit rotation gates.

![abc](/circuit.png)

The Quantum Shannon Decomposition (QSD) is the best known general method, using about twice the lower bound. It uses SVD and EIG recursively to factor out single qubit rotations at each step.

![abc](/qsd.png)

The focus will be on the uniform controlled rotation (UCR) gates indicated by $R_Z and R_Y$. The [unitaryGate](https://www.mathworks.com/help/matlab/ref/unitarygate.html) function can remove 1-qubit rotations that allow CX gates to cancel. This gives an informal depth-accuracy tradeoff, and outlines my ideas towards a minimum CX gate count and approximate error bounds. I've looked into two papers:

1. [This](https://arxiv.org/pdf/2106.05649) compresses QSD down to the lower bound using iterative group Lasso regression. Forcing rotation angles to 0 is intuitive but the optimization is expensive. This research is formalized mainly in the language of Lie theory as a recursive sequence of Cartan decompositions. Despite the constructive and sound theory, the QSD decomposition does not have the flexibility of trading off precision and length. They consider different templates for hardware topology.

2. [This](https://arxiv.org/abs/2205.00081) proves an error bound on approximately block encoding a target matrix $A$ using UCR gates. 

We learn from (1.) that sparse angles can make QSD optimal through optimization. This hints towards a direct approach. I adapt the main result of (2.) to the UCR gates at each recursive step of QSD. The circuit for block encoding a real matrix $A$ is below. How can this fit into the QSD circuit? The proof starts by giving error on matrix oracle $O_A$ from thresholding rotation angles θˆ with cutoff δc,

$$
δθ = θ ̃ − θ = ( Hˆ(⊗2n) P G ) δ θˆ ,
$$

![abc](/block-encode2.png)


Balancing direct FABLE encodings of smaller terms in the Hamiltonian expression with a Linear Combination of Unitaries (LCU)