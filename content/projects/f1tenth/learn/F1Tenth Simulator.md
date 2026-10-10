---
title: F1Tenth Simulator
description: "Setting up the F1Tenth Gym ROS 2D simulator, plus our Docker and Rust additions"
tags:
  - project/f1tenth
  - simulation
date: 2025-02-05
---
[F1Tenth Gym ROS](https://github.com/f1tenth/f1tenth_gym_ros) is a lightweight 2D simulator built with ROS.

![[f1tenth_sim_ws.png]] 
>[!Note]
> Fast Prototyping, Motion Planning, Racing Algorithm Test

- More portable
- Faster setup
- Faster prototyping
- Safer driving
- Sim to Real

## [Building Our Sim_ws](https://github.com/JisangYun00/f1sim_ws)
I updated some of Docker file and added the package for Rust-ROS2 setup
so inside the `sim_ws/src` folder, you can also see the `rust_ws` package, which includes some code to interact with simulation environment
![[f1sim_ws_rust.png]]
## To-Do
- [x] [[Lab2 Automatic Emergency Braking]]
- [x] [[Lab3 Wall Following]]
- [x] [[Lab4 Follow the Gap]]

- [ ] SLAM
