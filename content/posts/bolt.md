+++
title = 'anduril bolt'
date = 2024-10-24T21:05:44-04:00
draft = false
+++

Notes on the Anduril Bolt drone for estimating battery and motor requirements.

[“In round numbers, typical Bolt configurations are in the low tens of thousands of dollars, depending on the exact payload and configuration."](https://www.forbes.com/sites/davidhambling/2024/10/16/andurils-bolt-m-is-the-future-for-us-forces-if-they-can-afford-it/)

- Intended for intelligence, surveillance, reconnaissance, search and rescue
- Swappable batteries for extending flight time
- Rectangular frame makes sense to accommodate larger battery. X frames are more common to FPV racing drones because of their similar rotational inertias.

**Bolt** \
+20 km range \
+45 min endurance \
~12 lbs weight \
**Bolt-M** \
+20 km \
+40 min \
13-15 lbs (Payload Dependent)

The power required to hover can be derived from basic momentum theory. Assume \(\dot{m}\) to be the uniform mass flow rate of air through the rotors covering an area \(A\). The motors increase downwards air momentum resulting in upwards thrust \(T\), which can be expressed by the pressure drop across the rotors. This assumes all motor power goes to the kinetic energy of air over time. Dimensional analysis shows \(P\) has expected units. The drone with total mass \(M\) hovers when \(T=Mg\).

$$ \begin{array}{cccc}
\dot{m} = \rho A v & \quad & T = \frac{1}{2} \rho A v^2 & \quad & v = \sqrt{\frac{2T}{\rho A}} & \quad & P = \frac{1}{2} \dot{m}v^2 = \frac{T^{3/2}}{\sqrt{2\rho A}} 
\end{array}
$$

The motors in [Bolt's Youtube video](https://www.youtube.com/watch?v=EEXI6r08908&t=29s) resemble the Antigravity Type by T-Motor, which are best suited for ISR. Four of the [MN6007 KV160 motors](https://store.tmotor.com/product/mn6007-kv160-motor-antigravity-type.html) with P21*6.3" propellers produce ~5.4 kg of thrust at ~450 W. This configuration allows the drone to hover around the most efficient motor speed. The ideal hover power using the area covered by these propellers is ~260 W.

These motors are rated for a 6-12S LiPo battery. The specific loadout depends on the mission, but a [10900mAh 12S 44.4v battery](https://maxamps.com/collections/12s-lipo-battery-44-4v/products/lipo-10900-12s-44-4v-battery-pack) is a rough guideline. A small discharge rate is desirable for an extended flight.