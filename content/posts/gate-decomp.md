+++
title = 'quantum gate decomposition'
date = 2024-09-12T00:15:22-04:00
draft = false
+++

Quantum circuits are executed using the primitive operations of the quantum computer. The matrices of this primitive set form a basis of the unitary group $U(2^N)$. A general 1 qubit rotation can be decomposed into 3 rotations analgous to Euler rotations. Using the basis set {CX,RY,RZ}, this is the smallest quantum circuit for any unitary transformation on 2 qubits. The circuit uses 3 two-qubit CX gates and 15 single-qubit rotation gates. I find this pattern appealing because the CX gates are orientated as a SWAP gate.

Intuitive rotation on RO(3) as a factorization of transform in U(4)

![abc](/circuit.png)

The minimum number of CX gates for the general case of $N$ qubits is known, but implementations for this lower bound are not. The Quantum Shannon Decomposition (QSD) is the best known current method for the general case. It's based on recursive SVD and EIG factorizations, using about twice the lower bound.

![abc](/qsd.png)

My implementation of QSD is [unitaryGate](https://www.mathworks.com/help/matlab/ref/unitarygate.html). The focus will be on the uniform controlled rotation (UCR) gates indicated by $R_Z and R_Y$. They factor out a single qubit rotation at each recursive step. Throughout the recursion, the rotation threshold removes single qubit rotation gates allowing CX gates to cancel. This gives an informal depth-accuracy tradeoff. I wonder about bounding the error on the approximate unitary after thresholding. Two ideas:

1. [This](https://arxiv.org/pdf/2106.05649) compresses QSD down to the lower bound using iterative group Lasso regression. Forcing rotation angles to 0 is intuitive but the optimization is expensive. This research is formalized mainly in the language of Lie theory as a recursive sequence of Cartan decompositions. Despite the constructive and sound theory, the QSD decomposition does not have the flexibility of trading off precision and length. They consider different templates for hardware topology.

2. [This](https://arxiv.org/abs/2205.00081) proves an error bound on approximately block encoding a target matrix $A$ using UCR gates. 

We learn from (1.) that sparse angles can make QSD optimal through optimization. This hints towards a direct approach. Conceptually, I'd like to apply the proof of (2.) to the UCR gates at each recursive step of QSD. The circuit for block encoding a real matrix $A$ is below. How can this fit into the QSD circuit? Neglect the H and SWAP gates for now, which maybe can drop out.

The proof starts by giving error on matrix oracle $O_A$ from thresholding rotation angles θˆ with cutoff δc,

$$
δθ = θ ̃ − θ = ( Hˆ(⊗2n) P G ) δ θˆ ,
$$

![abc](/block-encode2.png)