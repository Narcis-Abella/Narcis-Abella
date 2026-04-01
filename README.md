# Hi, I'm Narcís 👋

Industrial Engineering student at IQS Barcelona and Technical Lead of my university robotics team.  
I'm currently working on robotics simulation fidelity, with a specific focus on sensor-noise metrology for sim-to-real validation.

Most robotics stacks don't fail because of planners. They fail earlier: the simulated sensor residuals do not match real hardware behavior.

---

## What I'm focused on now

- Building a metrological pipeline to validate sensor residual models (IMU, LiDAR, RGB-D) against controlled ground truth.
- Leading a university robotics team and shipping reproducible ROS 2 simulation infrastructure.
- Connecting research-grade validation to deployment-ready SLAM benchmarking.

---

## Featured repositories

### 🔬 [sensor-noise-metrology](https://github.com/Narcis-Abella/sensor-noise-metrology) *(active research)*  
Metrological validation framework for sensor residuals, designed to close the sensor-specific sim-to-real gap.  
Core approach: TOST-based equivalence testing, residual-level analysis, and physically grounded noise modeling.

### 🏥 [hospital-agv-sim](https://github.com/Narcis-Abella/hospital-agv-sim) *(active)*  
ROS 2 / Ignition Gazebo hospital-scale AGV simulation framework.  
Includes custom per-sensor noise nodes (IMU bias dynamics, range-dependent LiDAR noise, velocity-dependent odometry error).

### 📐 [tracer-odom-calibration](https://github.com/Narcis-Abella/tracer-odom-calibration) *(in progress)*  
Odometry calibration pipeline for AgileX Tracer 2 using visual ground truth (ArUco-based).  
Focus: modeling non-constant yaw error as a function of motion regime.

### 📊 [livox3d-slam-benchmark](https://github.com/Narcis-Abella/livox3d-slam-benchmark) *(early stage)*  
Reproducible 3D SLAM benchmark for Livox Mid-360 (ATE/RPE pipeline, backend comparisons, transfer-oriented evaluation).

---

## Technical stack

`ROS 2 Humble` · `Ignition Gazebo` · `Python` · `C++` · `Bash` · `Docker` · `Jetson Orin`  
`Livox Mid-360` · `RealSense D455` · `RPLiDAR A2` · `ABB YuMi` · `AgileX Tracer 2`

---

## How I work

- I chase failure modes, not surface fixes.
- I document while building. READMEs are part of the engineering work.
- I optimize for reproducibility first, then speed.
- I choose depth over shortcuts, and I call out problems early when something breaks.

---

## Background

🏆 1st Prize — Argó Research Award (UAB, Technology), 2022  
🏆 IQS-URL Research Distinction (Engineering & Technology), 2022  
📐 CSWA Certified  
🇬🇧 Cambridge English C1

---

📫 [LinkedIn](https://linkedin.com/in/narcis-abella) · narcisabellal@iqs.url.edu
