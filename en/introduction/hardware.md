# Hardware Requirements

| Category | Component | Notes |
| --- | --- | --- |
| Compute | Raspberry Pi 5 (8 GB) + 32 GB microSD | Passive heat sink or fan hat required for heavy SLAM workloads |
| Control | Arduino Nano Every + BTS7960 motor driver | Any 43 A motor driver works if it accepts 5 V PWM |
| Power | 3S LiPo (5000 mAh), 5 V buck converter | Provide a dedicated rail for logic electronics |
| Sensors | RPLidar A1/A2, BNO055/BNO086 IMU, wide-angle camera | RPLidar sits at 18 cm height for best coverage |
| Chassis | 1/10 RC chassis (e.g., Redcat Lightning EPX) | Needs Ackermann steering and 540 brushed motor |

## Wiring Notes

- Keep USB cables below 1 m to avoid dropouts with RPLidar.
- Use twisted servo wires for the steering PWM line to reduce noise.
- Always share a common ground between Raspberry Pi, Arduino, and the motor driver.
- Attach the IMU close to the vehicle's center to reduce bias in the EKF.

## Optional Upgrades

- Intel RealSense D435 for depth sensing
- LTE router for field deployments
- Separate MCU for battery management

---

Continue with the [software setup](#/en/introduction/software) to prepare Ubuntu, ROS2, and supporting tools.
