+++
title = 'anduril bolt'
date = 2024-10-24T21:05:44-04:00
draft = false
+++

Notes on Anduril's Bolt drone. Shouldn't be hard to estimate battery and motor requirements.

[“In round numbers, typical Bolt configurations are in the low tens of thousands of dollars, depending on the exact payload and configuration."](https://www.forbes.com/sites/davidhambling/2024/10/16/andurils-bolt-m-is-the-future-for-us-forces-if-they-can-afford-it/)

- Swappable batteries for extending flight time
- Intelligence, Surveillance & Reconnaissance (ISR)
- Search & Rescue (SAR).
- Rectangular frame makes sense to accommodate larger battery. X frames are more common to FPV racing drones because of their similar rotational inertias.

**Bolt** \
+20km range \
+45min endurance \
~12lbs weight \
**Bolt-M** \
+20km \
+40min \
13-15lbs (Payload Dependent)

Assume \(\dot{m}\) to be the uniform mass flow rate of air through the propellers. The motors increase downwards air momentum resulting in upwards thrust \(T\). Subsitute \(\dot{m}\) to solve for air velocity. Neglect heat and drag losses to assume all motor power goes to the kinetic energy of air over time. 
$$ \begin{array}{cccc}
\dot{m} = \rho A v & \quad & T = \dot{m} v & \quad & v = \sqrt{\frac{T}{\rho A}} & \quad & P = \frac{1}{2} \dot{m}v^2 = \frac{T^{3/2}}{2 \sqrt{\rho A}} 
\end{array}
$$
Dimensional analysis shows \(P\) has expected units. The drone with total mass \(M\) hovers when \(T=Mg\). From [the Youtube video](https://www.youtube.com/watch?v=EEXI6r08908&t=29s), the propeller radius looks about 6 inches. The Bolt is ideally estimated to hover using about 650 W. [The equation on Wikipedia](https://en.m.wikipedia.org/wiki/Momentum_theory) is similar, but I think the 2 inside the sqaure root is an error.


**Motors**
https://www.getfpv.com/motors/mini-quad-motors/amax-competition-2808-motor-1400kv.html
![](/bolt.png)