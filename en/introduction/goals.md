# Project Goals

The project is engineered as a full-stack learning journey. Each milestone adds a measurable capability to the vehicle while giving you a specific technical takeaway.

## Learning Objectives

### 1. Foundations
- Understand ROS2 node graphs, topics, services, and parameters.
- Build reproducible workspaces with colcon and package overlays.
- Practice Git-based workflows for documentation and firmware.

### 2. Embedded Control
- Implement motor control firmware on Arduino.
- Stream encoder and IMU data to ROS2 through serial topics.
- Design watchdogs that park the vehicle after communication loss.

### 3. Perception + Navigation
- Configure SLAM Toolbox for indoor and outdoor maps.
- Use Nav2 planners (Smac, Theta*) for waypoint navigation.
- Tune local costmaps using LiDAR obstacles and footprint models.

### 4. Operations
- Create health dashboards with diagnostics updaters.
- Automate log capture and rosbag recording.
- Deploy the stack headless over SSH and tmux.

## Milestones

| Phase | Goal | Observable Result |
| --- | --- | --- |
| MVP | Manual teleoperation + live LiDAR stream | Web UI can move the car and display scans |
| Mapping | Consistent SLAM maps | Saved map reloads with <5 cm drift |
| Autonomy | Waypoint navigation | Nav2 follows a three-point mission indoors |
| Reliability | Fleet-ready | Health panel stays green for a 30 min burn-in |

## Evaluation Checklist

- [ ] Vehicle can be armed/disarmed from the dashboard.
- [ ] RPLidar publishes at least 3 Hz with <2% dropped frames.
- [ ] EKF outputs fused odometry at 50 Hz.
- [ ] Nav2 completes a square trajectory twice without intervention.
- [ ] All nodes restart automatically through systemd services.

## Suggested Study Path

1. Build the car physically and verify motor direction.
2. Install software and bring up the ROS2 workspace.
3. Run SLAM in a small room and export a map.
4. Calibrate PID gains for the steering servo.
5. Add higher-level behaviors such as multi-waypoint missions.

---

Next up: [Arduino Basics](/en/arduino/basics) to flash the control firmware.
