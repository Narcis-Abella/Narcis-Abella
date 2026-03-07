# Hi, I'm Narcís 👋

I'm an Industrial Engineering student at IQS Barcelona who gets obsessed with hard problems until they're solved properly — not just "good enough."

Six months ago I had zero experience in autonomous systems. Today I'm Technical Lead of a robotics team running a full ROS 2 navigation stack on Nvidia Jetson hardware, and designing a metrological validation framework for Visual-LiDAR SLAM that uses an ABB YuMi industrial manipulator as ground truth. That's roughly how I operate: pick something difficult — ideally something nobody's done yet — go deep, and don't stop until I understand it better than expected. I'm building toward systems-level integration: robotics, data, and product, not just one layer.

-----

## What I'm working on

🔬 **[slam-sensor-metrological-validation](https://github.com/Narcis-Abella/slam-sensor-metrological-validation)** *(in progress)*
Metrological validation of IMU, LiDAR and RGB-D sensor models for SLAM. Uses an ABB YuMi (±0.02 mm repeatability) as kinematic ground truth to extract Allan Variance coefficients and build state-dependent noise models (M4). Compares real hardware, standard simulation and metrological simulation across multiple SLAM backends — designed to quantify the sim-to-real gap, not just describe it.

🤖 **[hospital-agv-sim](https://github.com/Narcis-Abella/hospital-agv-sim)**
ROS 2 / Ignition Gazebo simulation for an AgileX Tracer 2 AGV. Core contribution: replacing Gazebo's default noise primitives with per-sensor C++ nodes implementing Gauss-Markov bias, proportional range noise, and wheel slip models derived from real hardware datasheets.

🛠️ **[SocialTech_C_Setup](https://github.com/Narcis-Abella/SocialTech_C_Setup)**
Production-grade Infrastructure-as-Code pipeline for provisioning ROS 2 on Nvidia Jetson Orin. Solves JetPack 6 / RealSense kernel incompatibilities via automated L4T source compilation and custom fallback logic.

-----

## Stack

```
Robotics       ROS 2 Humble · Gazebo Fortress · FAST-LIO2 · Point-LIO · GLIM · ORB-SLAM3 · OpenVINS
Hardware       Nvidia Jetson Orin · Livox Mid-360 · RPLiDAR A2M12 · RealSense D455 · ABB YuMi
Languages      C++ · Python · Bash/Shell
Tools          Docker · Git · SolidWorks (CSWA) · Fusion 360 · PlotJuggler
Analysis       Allan Variance · evo (ATE/RPE) · Statistical modeling
```

-----

## A few things that define how I work

- I debug by understanding, not by trial and error.
- I document as I build — not after.
- If something doesn't work, I go one level deeper until I find out why.
- I've scripted custom kernel builds at 2am to keep a project moving. That's just Tuesday.

-----

## Background

🏆 1st Prize — Argó Research Award (UAB, Technology Category), 2022

🏆 IQS-URL Research Distinction (Engineering & Technology), 2022

📐 CSWA Certified (SolidWorks)

🇬🇧 Cambridge English C1

-----

📫 [LinkedIn](https://linkedin.com/in/narcis-abella) · narcisabellal@iqs.url.edu
