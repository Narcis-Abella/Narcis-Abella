# Hi, I'm Narcís 👋

Most robotics teams don't have a simulation problem. They have a *fidelity* problem.

You know your SLAM stack works in Gazebo. You know what the real sensor should look like. But between a clean simulated point cloud and actual deployment on a hospital floor, there's a gap that kills navigation stacks (and it almost always comes from sensor models that were never grounded in real hardware behaviour).

That's the problem I am currently focused on.

---

## The project

It started as a university challenge (November 2025): build an autonomous wheelchair for people with mobility impairments. That problem pulled me deep enough into autonomous systems that the scope kept expanding — from wheelchair to AGV, from AGV to the question of why simulation-to-real transfer keeps failing, and from that question to building the tooling to actually measure and close that gap.

The result is an open-source simulation framework for mobile robot SLAM validation, built on ROS 2 and Ignition Gazebo. The goal is not to replicate what tools like Isaac Sim do. The gap this fills is different: Gazebo ships with configurable noise models, but their parameters are placeholders — and there is no standardised pipeline for deriving those parameters from real hardware characterisation, validating them against a metrological ground truth, and using the result to benchmark SLAM transfer before deployment. Most teams either tune by intuition or skip the sensor models entirely.

---
## Why sensor metrology — and why better models are not enough

The standard response to the sim-to-real gap is domain randomisation: randomise physics parameters broadly enough and hope the real world falls inside the training distribution. It's a legitimate technique, but it has a well-documented failure mode — the robot learns to exploit the simulator's inconsistencies rather than solve the actual task. Muratore et al. formalised this as *Simulation Optimization Bias*; the field's most comprehensive recent survey on the reality gap (Aljalbout et al., *Annual Review of Control, Robotics, and Autonomous Systems*, 2026) identifies unvalidated sensor models as one of its core unresolved sources.

Higher-fidelity models don't fix this. A more detailed sensor model that has never been validated against real hardware gives the policy a richer set of simulation-specific artefacts to overfit to. The navigator doesn't learn robustness — it learns to assume the artefacts.

Sensor metrology changes the question. Instead of *"did we add enough noise?"*, the question becomes *"are the simulated and real sensor statistically equivalent under a defined operating envelope?"* — a claim that can actually be tested, bounded, and reported. That's what `slam-sensor-metrological-validation` is building toward.

---

## Repositories

The framework is built in layers. Each layer feeds the next.

### 🏥 [hospital-agv-sim](https://github.com/Narcis-Abella/hospital-agv-sim) — *active*
The core simulator. ROS 2 / Ignition Gazebo hospital environment (~2,500 m²) for an AgileX Tracer 2 AGV. The central contribution is replacing Gazebo's default noise primitives with per-sensor C++ nodes implementing physics-grounded stochastic models — Gauss-Markov IMU bias, range-dependent LiDAR noise, and a velocity-dependent wheel odometry error model. This is where all calibration and validation results eventually land.

### 📐 [tracer-odom-calibration](https://github.com/Narcis-Abella/tracer-odom-calibration) — *data collection in progress*
The first calibration layer. Characterises velocity-dependent yaw error in the Tracer 2 using an ArUco-based visual ground truth pipeline (iPhone XR, calibrated intrinsics, RMS=0.05px). Full factorial design: 810 measurements across angular velocity, acceleration, target angle and direction. Key finding so far: odometry error is not constant — it follows a U-shaped velocity-dependent curve driven by drivetrain stiction at low speed and wheel slip at high speed. The fitted model will replace the placeholder odometry noise node in `hospital-agv-sim`.

### 🔬 [slam-sensor-metrological-validation](https://github.com/Narcis-Abella/slam-sensor-metrological-validation) — *research proposal, design complete*
The validation framework. Uses an ABB YuMi collaborative arm (±0.02mm repeatability) as kinematic ground truth to characterise IMU, 2D LiDAR, 3D LiDAR (Livox Mid-360) and RGB-D (RealSense D455) noise under controlled dynamic conditions. Builds a family of sensor noise models from static Allan Variance up to a kinematic-state and thermal-state dependent covariance model (M4). The goal: quantify how much of the sim-to-real gap is attributable to sensor model fidelity, and demonstrate that a properly grounded simulator produces SLAM behaviour that transfers to real hardware. Target venue: *Measurement* / IEEE TIM.

### 📊 [livox3d-slam-benchmark](https://github.com/Narcis-Abella/livox3d-slam-benchmark) — *early stage*
Reproducible 3D SLAM benchmark on Livox Mid-360. GLIM mapping complete; shared trajectory pipeline, ATE/RPE metrics and backend-agnostic comparisons across FAST-LIO2, GLIM and RTAB-Map in progress. Blocked on a custom Ignition Gazebo plugin for the non-repetitive scan pattern.

---

## Stack

```
Robotics       ROS 2 Humble · Ignition Gazebo · FAST-LIO2 · Point-LIO · GLIM · ORB-SLAM3 · OpenVINS
Hardware       NVIDIA Jetson Orin · Livox Mid-360 · RealSense D455 · RPLiDAR A2 · AgileX Tracer 2 · ABB YuMi
Languages      Python · C++ · Bash
Tools          Docker · Git · PlotJuggler · SolidWorks (CSWA) · Fusion 360
Analysis       Allan Variance · evo (ATE/RPE) · ArUco pose estimation · Statistical modelling
```

---

## How I work

- I go one level deeper than required until I understand the failure mode, not just the fix.
- I document as I build — the READMEs are part of the work, not an afterthought.
- Infrastructure is a first-class concern: reproducible environments, automated pipelines, no "works on my machine."
- The wheelchair problem is still the one I'm solving. Everything else is tooling.

---

## Background

🏆 1st Prize — Argó Research Award (UAB, Technology Category), 2022

🏆 IQS-URL Research Distinction (Engineering & Technology), 2022

📐 CSWA Certified (SolidWorks Associates)

🇬🇧 Cambridge English C1

---

📫 [LinkedIn](https://linkedin.com/in/narcis-abella) · narcisabellal@iqs.url.edu
