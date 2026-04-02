---
title: Lab2_Automatic_Emergency_Braking
draft: false
tags:
  - F1Tenth
aliases:
  - AEB
---
## AEB(Automotic Emergency Braking)
> [Lecture Note](https://docs.google.com/presentation/d/1HQSLPLE-fZ3EN-9PyqMd7-ti7JSLKL7qlrouGFEprU0/edit#slide=id.g10a2bc3b756_0_295) / [Our Code](https://github.com/JisangYun00/f1sim_ws/tree/master/src/lecture_ws/f1tenth_lab2_aeb)

Emergency braking based on the parameter 'Time to collision'
$$
TTC_i(t) = \frac{r_i(t)}{[-\dot{r}_i(t)]_+}
$$
TTC is defined as the time it would take for the ego-vehicle and on object to intercept one another given that they each maintain their current heading and velocity.

$$
\begin{aligned}
r_i(t) &: \text{Range between vehicle and object} \\
\dot{r}_i &= v_x \cos(\theta_i)
\end{aligned}
$$
![[AEB_terminal.png]]

**- Next -**
[[Lab3_Wall_Following]]

---
### Reference
[F1Tenth_Lab2_Template](https://github.com/f1tenth/f1tenth_lab2_template)
