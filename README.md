# Hi, I'm Narcís 👋

Most robotics teams don't have a simulation problem. They have a *fidelity* problem.

You know your SLAM stack works in Gazebo. You know what the real sensor should look like. But between a clean simulated point cloud and actual deployment on a hospital floor, there's a gap that kills navigation stacks (and it almost always comes from sensor models that were never grounded in real hardware behaviour).

That's the problem I am currently focused on.

---

## The project

It started as a university challenge (November 2025): build an autonomous wheelchair for people with mobility impairments. That problem pulled me deep enough into autonomous systems that the scope kept expanding — from wheelchair to AGV, from AGV to the question of why simulation-to-real transfer keeps failing, and from that question to building the tooling to actually measure and close that gap.

The result is an open-source simulation framework for mobile robot SLAM validation, built on ROS 2 and Ignition Gazebo. The goal is not to replicate what tools like Isaac Sim do. The gap this fills is different: Gazebo ships with configurable noise models, but their parameters are placeholders — and there is no standardised pipeline for deriving those parameters from real hardware characterisation, validating them against a metrological ground truth, and using the result to benchmark SLAM transfer before deployment. Most teams either tune by intuition or skip the sensor models entirely.

---
## Why sensor metrology — and why better simulation models are not enough

The standard objection to simulation-based SLAM validation goes like this: *modern simulators are already highly capable — Isaac Sim, Gazebo Fortress, MuJoCo — and there are well-established techniques like domain randomization to cover the gaps. Why does sensor metrology matter on top of that?*

It's the right question. Here's the honest answer.

---

**The problem isn't that simulators are too simple. It's that their errors are uncharacterised.**

Domain randomization — randomising friction, mass, noise levels, visual textures — is the field's dominant response to the reality gap. It works by hoping that if you randomise broadly enough, the real world falls somewhere inside the training distribution. The approach has produced genuine results in locomotion and manipulation.

But it has a well-documented failure mode: *the robot learns to exploit the simulator, not to navigate reality.* Muratore et al. coined this the *Simulation Optimization Bias* — running policy search on a slightly faulty simulator tends to produce policies that maximise reward by exploiting the model's inconsistencies, not by solving the underlying task. Baker et al. observed this directly: agents trained in simulation learned to abuse the physics engine to gain an advantage that doesn't exist in the real world. The community's own review literature (Muratore et al., 2022, *Frontiers in Robotics and AI*) states it plainly: the learner is free to overfit to simulator artefacts, and the most competitive solutions in simulation are often the ones that rely most heavily on poorly-modelled dynamics.

Higher-fidelity models make this worse, not better. A more detailed sensor model that is still unvalidated gives the policy a richer set of simulation-specific artefacts to exploit. The downstream application — SLAM, odometry, loop closure — is sensitive to the geometry and statistical structure of the sensor output, not just its magnitude. A simulated LiDAR that produces perfect ray-cast returns with additive noise has the wrong spatial structure for a solid-state sensor whose non-repetitive scan pattern creates time-varying coverage gaps. A simulated IMU with fixed σ doesn't capture the bias instability and rate random walk that drive VIO divergence over long integration windows. The navigator trained on these models doesn't learn to be robust; it learns to assume the artefacts.

The field's most thorough recent survey on this problem — Aljalbout et al., *Annual Review of Control, Robotics, and Autonomous Systems*, 2026 — identifies sensor modeling as one of the core unresolved sources of the reality gap, alongside dynamics parameterisation and collision sensing. They note that simulation-based approaches commonly simplify or omit the physical details that matter most in real deployment: manufacturing tolerances, thermal effects, material-dependent return characteristics, actuation delays. The survey covers the full landscape of mitigation strategies — domain randomisation, real-to-sim transfer, co-training — and reaches a sobering conclusion: none of these close the gap if the underlying sensor model is structurally wrong.

The automotive and ADS community has reached the same conclusion from a different direction. Ngo et al. (ITSC 2021) showed that sensor model fidelity must be validated both explicitly (does the simulated data match real data statistically?) and implicitly (does a downstream perception model trained on simulation perform the same way on real data?). Without both, a high-fidelity simulation is just a confident-sounding source of misleading training signal. Wu et al. (2025) extended this to scenario validation, demonstrating that without formal equivalence bounds, synthetic test scenarios cannot be said to represent real-world behaviour — even when generated by sophisticated learned models.

**What metrology adds is the ability to know what you don't know.**

The goal of characterising sensors against sub-millimetre ground truth — Allan Variance decomposition for IMUs, range-dependent and material-dependent error budgets for LiDARs, thermal drift profiles for RGB-D cameras — is not to build a perfect simulator. It's to replace uncharacterised errors with bounded, interpretable ones.

A simulator built from metrological characterisation lets you make a specific claim: *under this operating envelope, the simulated and real sensor are statistically equivalent to within this tolerance.* That is a fundamentally different claim from "we added noise and randomised the parameters." It's the difference between a calibrated instrument and a well-intentioned guess — and in a hospital environment, navigating around patients, that distinction has consequences.

This is what `slam-sensor-metrological-validation` is building toward.
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
