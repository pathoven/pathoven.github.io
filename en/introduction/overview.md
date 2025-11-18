# Project Overview

Pathoven Drive is our open hardware testbed that demonstrates how ROS2 Jazzy can control a small Ackermann-steered vehicle. The documentation focuses on reproducibility so that you can rebuild the exact stack without guesswork.

## Architecture at a Glance

| Layer | Components | Notes |
| --- | --- | --- |
| Perception | RPLidar A1/A2, IMU, camera | Provides 2D scans for SLAM and short-range obstacle detection |
| Compute | Raspberry Pi 5 (8 GB) | Runs Ubuntu 24.04, ROS2 nodes, and the web dashboard backend |
| Control | Arduino Nano Every | Handles motor driver, steering servo, and failsafe PWM passthrough |
| Communication | Ethernet + Wi-Fi | Primary link is Ethernet for reduced latency; Wi-Fi used for remote shells |

## Core Features

- Real-time teleoperation with joystick or web UI
- SLAM toolbox with persisted map management
- Nav2 autonomous waypoint navigation
- Diagnostics panel that surfaces CPU, temperature, and voltage

## Documentation Philosophy

1. **Pragmatic** – every page ends with actionable next steps.
2. **Hardware-first** – we validate each sensor before adding more software.
3. **Bilingual** – Turkish pages mirror the English instructions so mixed teams can collaborate.

---

Continue with the [hardware requirements](#/en/introduction/hardware) when you are ready to source parts.
