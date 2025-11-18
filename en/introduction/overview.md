# Project Overview

## What is Pathoven Drive?

Pathoven Drive is an autonomous RC car project developed using **ROS2 (Robot Operating System 2)**, featuring real-time mapping and navigation capabilities. This project serves as a comprehensive learning platform for robotics programming, SLAM (Simultaneous Localization and Mapping), and autonomous navigation.

## 🎯 Project Goals

### 1. Education & Learning
- Understand the ROS2 ecosystem
- Enhance robotics software development skills
- Hands-on learning of SLAM and navigation algorithms
- Develop real-time systems

### 2. Technical Excellence
- Low-cost yet powerful platform (Raspberry Pi 5)
- Modular and extensible architecture
- Production-ready code quality
- Well-documented and maintainable

### 3. Community & Sharing
- Open source project
- Detailed documentation
- Reproducible
- Open to contributions

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│           Raspberry Pi 5 (ROS2 Jazzy)               │
│                                                      │
│  ┌────────────────┐        ┌──────────────────┐    │
│  │  Motor Control │◄───────┤   Arduino Uno    │    │
│  │     Bridge     │ Serial │  (Motor Driver)  │    │
│  └────────┬───────┘        └──────────────────┘    │
│           │                                          │
│  ┌────────▼────────┐       ┌──────────────────┐    │
│  │   Odometry      │       │   RPLidar A1     │    │
│  │   Publisher     │       │  (Laser Scanner) │    │
│  └────────┬────────┘       └────────┬─────────┘    │
│           │                          │              │
│           │      ┌───────────────────▼──┐           │
│           └──────►   SLAM Toolbox      │           │
│                  │   (Mapping)         │           │
│                  └──────────┬──────────┘           │
│                             │                       │
│                  ┌──────────▼──────────┐           │
│                  │    Nav2 Stack       │           │
│                  │   (Navigation)      │           │
│                  └──────────┬──────────┘           │
│                             │                       │
│                  ┌──────────▼──────────┐           │
│                  │  Socket.IO Bridge   │           │
│                  │   (WebSocket)       │           │
│                  └──────────┬──────────┘           │
└───────────────────────────┬─┬────────────────────────┘
                            │ │
                    ┌───────▼─▼────────┐
                    │   Web Browser    │
                    │  (Control GUI)   │
                    └──────────────────┘
```

## 🔧 Key Components

### 1. Hardware Layer

**Raspberry Pi 5**
- CPU: Quad-core ARM Cortex-A76 @ 2.4GHz
- RAM: 4GB+ (8GB recommended)
- OS: Ubuntu 24.04 Noble
- Role: Main computing unit, ROS2 host

**Arduino Uno/Nano**
- CPU: ATmega328P
- Role: Motor control, low-level hardware interface
- Tasks: PWM control, encoder reading, serial communication

**RPLidar A1**
- Scanning range: 0.15m - 12m
- Scan rate: 5.5Hz (360° / 5.5 = ~2000 points/sec)
- Resolution: ~0.5°
- Interface: USB Serial
- Role: Environment sensing, data source for SLAM

**RC Car Chassis**
- Steering type: Ackermann
- Wheelbase: ~15cm
- Motor: DC motor + encoder
- Servo: Steering control

### 2. Software Layer

**ROS2 Jazzy Jalisco**
- LTS release (Long Term Support)
- Optimized for Ubuntu 24.04
- Modern C++17 and Python 3.10 support

**SLAM Toolbox**
- Graph-based SLAM
- Online and offline mapping
- Loop closure
- Lifelong mapping support

**Nav2 (Navigation2)**
- Behavior Trees
- Regulated Pure Pursuit Controller
- Obstacle avoidance
- Dynamic replanning

**Socket.IO Bridge**
- Custom Python implementation
- Low latency (~20-40ms)
- Data throttling and compression
- Real-time communication via WebSocket

## 📊 Performance Metrics

| Metric | Value |
|--------|-------|
| **CPU Usage** | 25-40% (Raspberry Pi 5) |
| **RAM Usage** | 1.5-1.7 GB |
| **Latency** | 20-40ms (Web GUI) |
| **Map Update Rate** | 1 Hz |
| **Scan Rate** | 10 Hz |
| **Odometry Rate** | 10-20 Hz |
| **Position Accuracy** | ±5cm (with laser matcher) |
| **Heading Accuracy** | ±3° (with laser matcher) |

## 🌟 Key Features

### 1. Headless Operation
- Full control via SSH
- No X11 forwarding needed
- Low CPU and bandwidth usage

### 2. Modern Web GUI
- React + Vite
- Responsive design
- Real-time map visualization
- Keyboard and touch control

### 3. Optimized Performance
- Custom Socket.IO bridge (60% less CPU)
- Data throttling and downsampling
- Async I/O
- QoS optimizations

### 4. Production Ready
- Comprehensive error handling
- Logging and monitoring
- Performance profiling tools
- Detailed documentation

## 🎓 Learning Path

This project has a modular structure, and each module can be learned independently:

```
Level 1: Basics
├── Arduino programming
├── Motor control
└── Serial communication

Level 2: ROS2 Fundamentals
├── ROS2 installation
├── Topics and Messages
├── Nodes and Packages
└── Launch files

Level 3: Sensing
├── LiDAR integration
├── Sensor fusion
├── Odometry
└── TF (Transform) system

Level 4: SLAM & Navigation
├── SLAM Toolbox
├── Map creation
├── Localization (AMCL)
└── Path planning (Nav2)

Level 5: Advanced
├── Web interface development
├── Socket.IO bridge
├── Performance optimization
└── Custom ROS2 node development
```

## 🔄 Development Status

| Module | Status | Note |
|--------|--------|------|
| **Motor Control** | ✅ Complete | Production ready |
| **Odometry** | ✅ Complete | With calibration tools |
| **LiDAR Integration** | ✅ Complete | RPLidar A1 driver |
| **SLAM** | ✅ Complete | SLAM Toolbox, mapping mode |
| **Web GUI** | ✅ Complete | Socket.IO + React |
| **Navigation** | 🚧 In Progress | Nav2 integration |
| **Obstacle Avoidance** | 📅 Planned | - |
| **Multi-Robot** | 📅 Planned | - |

## 🚀 Next Steps

Ready to get started?

1. [Required Hardware](hardware.md) - What do you need?
2. [Software Requirements](software.md) - Software installations
3. [Project Goals](goals.md) - Detailed learning outcomes

---

**💡 Tip:** This documentation is designed so you can start even without any ROS knowledge. It's recommended to follow each topic sequentially.

