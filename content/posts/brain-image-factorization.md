+++
title = 'image factorization in the brain'
date = 2024-09-17
draft = false
+++

Interested in perception and signaling systems, particularly in humans. [Earlier project](https://github.com/bowrango/dashcam-misalignment/tree/main) using basic RNN to predict dash camera misalignment. Now reading about visual processing models inspired by the brain.

todos
- Draw these images myself
- [6] Section 2.6.2 generating exact solutions to the subset sum problem
- Note quantum applications using residue numbers. Just saw some factoring papers

[1] [Invariance and equivariance in brains and machines](https://www.youtube.com/watch?v=xnhhp916JNU&list=LL&index=18)\
[2] [Disentangling images with Lie group transformations and sparse coding](https://arxiv.org/abs/2012.12071)\
[3] [Analog Memory and High-Dimensional Computation](http://www.rctn.org/bruno/public/nature-neuromorphic-talk.pdf)\
[4] [Neuromorphic Visual Scene Understanding with Resonator Networks](https://arxiv.org/pdf/2208.12880)\
[5] [Hippocampal memory, cognition, and the role of sleep](https://www.youtube.com/watch?v=c2_rnYdUMiM)\
[6] [Computing with Residue Numbers in High-Dimensional Representation](https://www.researchgate.net/publication/375793530_Computing_with_Residue_Numbers_in_High-Dimensional_Representation)\
[7] [Visual scene analysis via factorization of HD vectors](https://redwood.berkeley.edu/wp-content/uploads/2022/11/HDC-scene-analysis-factorization.pdf)

The brain computes and represents
- geometric transforms -> equivariance
- shapes independent from variations in pose, motion or lighting -> invariance

Cones on the back of your eye perceive scene information.

![](/eye-cones.png)

Set of points lives on nonlinear manifold. Halfway point corresponds to superposition of both patterns at half contrast so manifold has to be curved. 

![](/image-on-manifold.png)

Different patterns trace out different manifolds but importantly the Lie operator is the same. Theory of Lie groups tells us all matrices moving on this manifold belong to same group. Some other examples include the groups of n-dimensional rotations SO(n) (special orthogonal) and rigid motions SE(n) (special Euclidean).

Images are explained as product of transformation and shape. It is reminiscent of Plato's theory of forms, that physical objects and matter are merely imitations of the non-physical ideas. Elaborate more.

Formally, [2] combines Lie Group transformation learning and sparse coding within a Bayesian model. Images become a sparse superposition of shape components followed by a transformation that is parameterized by n continuous variables. The generative model is

$$
I = T(s) Φα + ε
$$

- transformation parameter s ∈ Rn is a random vector with i.i.d elements  
- Φ ∈ RD×K is the dictionary with each column having unit L2 norm
- sparse code α ∈ RK is a random vector with i.i.d elements in exponential distribution
- random noise ε is i.i.d.

The transformations T(s) are parameterized as actions of compact, connected, commutative Lie groups on images. They can be decomposed using Peter-Weyl theorem to get

$$
I = e^{A s}Φα + ε
$$
$$
I = We^{\sum s}W^T Φα + ε
$$
$$
I = WR(s)W^T Φα + ε
$$

- R(s) is the learned block diagonal matrix of complex phasors.
- W is learned orthogonal matrix of the transform group

This imposes theoretical constraints on learnable transformations, but they can still be approximately learned in practice.
- Compactness enforces the transformations to be periodic 
- Connectedness precludes discrete transformations like reflections 
- Commutativity excludes non-commutative transformations such as 3D rotations

The block diagonal structure allows for efficient inference and learning. Reformulate as vector factorization

$$
W^TI = R(s)W^T Φα + ε
$$

$$
\overset{\sim}{I} = z(s) .* \overset{\sim}{O}(\alpha)
$$

- equivariant part z(s) is diagonal elements of R. Elements are complex phasors
- invariant part O(\alpha) is shape model

Can't gradient descent because of too many local minima. Performs gradient ascent on their log-likelihood

$$
\nabla_{\theta} \ln P_{\theta}(\mathbf{I}) \approx \mathbb{E}_{s \sim P_{\theta}(s | \mathbf{I}, \hat{\alpha})} \left[ \nabla_{\theta} \ln P_{\theta}(\mathbf{I} | s, \hat{\alpha}) \right]
$$

optimization uses Riemannian ADAM

Set of values {x} can be represented in weighted superposition. Product(N) to sum(N) representation complexity.

![](/mnist-results-sparse-lie.png)
