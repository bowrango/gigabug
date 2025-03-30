+++
title = 'Home'
date = 2024-09-04T21:28:27-04:00
draft = false
+++

### What We Do

Gigabug Computer researches and develops novel printed circuit boards to make physics-based computing more accessible and practical. Modern advances in artificial intelligence drive the need for specialized hardware to solve optimization problems and accelerate machine learning. The search to scale compute “beyond Moore” has focused research on coupled oscillator systems [1-2]. Our goal is to unlock new possibilities in optimization and AI by creating the first commercially viable oscillatory computing systems.

The key motivation is ultra low-power consumption by utilizing physics as a natural compute resource. This requires a new hardware architecture fundamentally different than modern computers used today. 

We're developing technology that leverages natural dynamics of coupled electronic oscillators to find optimal solutions to combinatorial optimization problems. These so-called Ising machines [3] are physical devices designed to minimize the function

$$H = \mathbf{s}^T \mathbf{J} \mathbf{s}$$

where the physical system variables $\mathbf{s}$ reach equilibrium through an [annealing](https://en.wikipedia.org/wiki/Simulated_annealing) process. This simple function represents some of the most difficult problems for modern computers [4] and is extensible to machine learning through probabilistic energy-based models [5]. The physical variables are made analogous to the logical variables of the input problem. In essence, the problem is embedded onto the device as an energy potential where solutions correspond to low-energy states at equilibrium.

![Overview of Ising-based Computing](/overview.png)

This physics-based approach has the potential to be orders of magnitude more energy-efficient than traditional computing methods for these problems. Many various hardware architectures have been proposed yet there remains fundamental software and hardware challenges. There are currently no oscillator-based computing systems available on the market -- they only exist in research labs as prototypes. 

At Gigabug Computer, we are making this breakthrough technology accessible outside of research labs by developing the first commercially available oscillator-based computing system. Our approach leverages continuous, physics-driven computation to solve complex optimization problems. We're very excited about this emerging class of technology.

### References

[1] [T. N. Theis and H.-S. P. Wong, "The End of Moore's Law: A New Beginning for Information Technology", Computing in Science and Engineering, vol. 19, no. 2, pp. 41-50, 2017.](https://ieeexplore.ieee.org/document/7878935)

[2] [P. Kumar et al., "Coupled oscillators for computing: A review and perspective", Appl. Phys. Rev. 7, 011302, 2020.](https://pubs.aip.org/aip/apr/article-abstract/7/1/011302/997386/Coupled-oscillators-for-computing-A-review-and?redirectedFrom=fulltext)

[3] [Mohseni, N., McMahon, P.L. & Byrnes, T., "Ising machines as hardware solvers of combinatorial optimization problems", Nat Rev Phys 4, 363–379, 2022.](https://www.nature.com/articles/s42254-022-00440-8)

[4] [A. Lucas, "Ising formulations of many NP problems," Frontiers in Physics, vol. 2, p. 5, 2014.](https://www.frontiersin.org/journals/physics/articles/10.3389/fphy.2014.00005/full)

[5] [D. Melanson et al., "Thermodynamic Computing System for AI Applications", arXiv preprint arXiv:2312.04836, 2023.](https://arxiv.org/pdf/2312.04836)

[6] [T. Wang, "Oscillator-based Ising Machine", arViv preprint arXiv:1709.08102v2, 2017.](https://arxiv.org/abs/1709.08102)

[7] [W. Moy et al., "A 1968-spin Ising processor for solving combinatorial optimization problems", Nature Electronics, vol. 5, no. 12, pp. 864-873, 2022.](http://www.ee.umn.edu/groups/VLSIresearch/papers/2022/NatureElectronics22_Ising.pdf)