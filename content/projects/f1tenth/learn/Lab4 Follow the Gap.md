---
title: "Lab 4: Follow the Gap"
description: "Follow the Gap with adaptive scan thresholds, tested in simulation and on the real car"
tags:
  - project/f1tenth
date: 2025-02-10
---
> [Lecture_Note](https://docs.google.com/presentation/d/1icJ0SHz2Q6JybGRywRTIHEpZbuQ_K2lmvEoJ-sYO4C4/edit#slide=id.p22) / [Our Code](https://github.com/JisangYun00/f1sim_ws/tree/master/src/lecture_ws/f1tenth_lab4_template)
## Method
![[Follow the gap Tweak1.png]]
## Adaptive Scan-Threshold
> **Adjusting Scan Threshold with minimum range**
``` cpp
            {0.15, 0.4},  // Min
            {0.2, 0.8},   
            {0.5, 1.4},   
            {0.8, 1.8},
            {1.1, 2.4},
            {1.4, 2.8},   // Middle range
            {1.7, 3.2},
            {2.0, 3.6},
            {2.3, 3.9},
            {2.6, 4.2},
            {3.0, 4.5}    // Max

```

> **Limit the View angle**
```cpp
    double left_wing = M_PI / 2.7;
    double right_wing = -(M_PI / 2.7);

```
## Simulation Workspace
This is with Bubble Method
![[Follow-the-Gap-levine-obs.mp4]]
![[Gap_follow_sim(obs).mp4]]

**Race Track Map**
![[Gap_Follow_sim(2025.02.09).mp4]]
## Real World Test
![[gap-follow-test.mp4]]

## References
- [F1Tenth_Lab4_Template](https://github.com/f1tenth/f1tenth_lab4_template)
- [Disparity Node Method](https://web.archive.org/web/20250212014645/https://nathanotterness.com/2019/04/the-disparity-extender-algorithm-and.html)
- [disparity Node Github](https://github.com/pmusau17/Platooning-F1Tenth?tab=readme-ov-file)
