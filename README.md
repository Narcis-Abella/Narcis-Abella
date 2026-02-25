# Hi, I’m Narcís 👋

I’m an Industrial Engineering student at IQS Barcelona who gets obsessed with hard problems until they’re solved properly — not just “good enough.”

Six months ago I had zero experience in autonomous systems. Today I’m Technical Lead of a robotics team running a full ROS 2 navigation stack on Nvidia Jetson hardware, and designing a metrological validation framework for Visual-LiDAR SLAM that uses an ABB YuMi industrial manipulator as ground truth. That’s roughly how I operate: pick something difficult, go deep, don’t stop until I understand it better than expected.

-----

## What I’m working on

🔬 **[visual-lidar-slam-validation](https://github.com/Narcis-Abella/visual-lidar-slam-validation)** *(in progress)*  
Metrological characterization of IMU, LiDAR and RGB-D sensors using a 6-DOF industrial manipulator as kinematic ground truth. Extracting Allan Variance coefficients to build physics-grounded Gazebo sensor models. Designed to close the sim-to-real gap quantitatively — not qualitatively.

🤖 **[hospital_sim](https://github.com/Narcis-Abella/hospital_sim)**  
ROS 2 / Ignition Gazebo simulation for an AgileX Tracer 2 AGV. Core contribution: replacing Gazebo’s default noise primitives with per-sensor C++ nodes implementing Gauss-Markov bias, proportional range noise, and wheel slip models derived from real hardware datasheets.

🛠️ **[SocialTech_C_Setup](https://github.com/Narcis-Abella/SocialTech_C_Setup)**  
Production-grade Infrastructure-as-Code pipeline for provisioning ROS 2 on Nvidia Jetson Orin. Solves JetPack 6 / RealSense kernel incompatibilities via automated L4T source compilation and custom fallback logic.

-----

## Stack

```
Robotics       ROS 2 Humble · Gazebo Fortress · FAST-LIO2 · GLIM · EKF
Hardware       Nvidia Jetson Orin · Livox Mid-360 · RPLiDAR A2M12 · RealSense D455 · ABB YuMi
Languages      C++ · Python · Bash/Shell
Tools          Docker · Git · SolidWorks (CSWA) · Fusion 360 · PlotJuggler
Analysis       Allan Variance · ATE/RPE · Statistical modeling
```

-----

## A few things that define how I work

- I debug by understanding, not by trial and error.
- I document as I build — not after.
- If something doesn’t work, I go one level deeper until I find out why.
- I’ve scripted custom kernel builds at 2am to keep a project moving. That’s just Tuesday.

-----

## Background

🏆 1st Prize — Argó Research Award (UAB, Technology Category), 2022  
🏆 IQS-URL Research Distinction (Engineering & Technology), 2022  
📐 CSWA Certified (SolidWorks)  
🇬🇧 Cambridge English C1

-----

📫 [LinkedIn](https://linkedin.com/in/narcis-abella) · narcisabellal@iqs.url.edu
