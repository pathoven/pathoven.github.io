# Pathoven Drive - English Documentation

> ROS2-Based Autonomous RC Car Project

## 🎯 About The Project

Pathoven Drive is an autonomous RC car project running on Raspberry Pi 5 with ROS2 Jazzy, featuring real-time mapping and navigation capabilities.

## ✨ Features

- 🤖 **ROS2 Jazzy** - Latest ROS2 LTS release
- 🍓 **Raspberry Pi 5** - Powerful processing
- 📡 **RPLidar A1** - 360° laser scanning
- 🗺️ **SLAM Toolbox** - Real-time mapping
- 🧭 **Nav2** - Autonomous navigation
- 🌐 **Web Interface** - Modern control panel
- ⚡ **Socket.IO Bridge** - Low-latency communication

## 🚀 Quick Start

### Requirements

**Hardware:**
- Raspberry Pi 5 (4GB+ RAM)
- Arduino Uno/Nano
- RPLidar A1
- RC Car (Ackermann steering, 15cm wheelbase)
- USB cables
- Power supply (5V 3A+)

**Software:**
- Ubuntu 24.04 Noble
- ROS2 Jazzy Jalisco
- Python 3.10+
- Node.js 18+

### Installation

```bash
# Create ROS2 workspace
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

# Clone packages (example)
git clone https://github.com/your-repo/motor_control_bridge
git clone https://github.com/your-repo/robot_description

# Install dependencies
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y

# Build
colcon build --symlink-install
source install/setup.bash
```

### First Run

```bash
# Terminal 1: Backend
ros2 launch robot_description headless_mapping.launch.py

# Terminal 2: Socket.IO Bridge
ros2 run motor_control_bridge ros2_socketio_bridge

# Terminal 3: Web GUI
cd ~/bozaw_web_socketio && npm run dev
```

Open in browser: **http://raspberrypi.local:3000**

## 📖 Documentation Structure

This documentation covers all project phases step-by-step:

1. **Introduction** - Project overview and requirements
2. **Arduino** - Motor control and serial communication
3. **Raspberry Pi** - ROS2 installation and configuration
4. **Motor Control** - ROS2 motor control package
5. **Robot Model** - URDF and TF system
6. **LiDAR** - RPLidar integration
7. **SLAM** - Mapping and localization
8. **Navigation** - Autonomous driving
9. **Web Interface** - User interface development

## 🎓 Learning Outcomes

After completing this project, you will:

- ✅ Understand ROS2 fundamentals
- ✅ Learn robotic kinematics and odometry
- ✅ Implement SLAM algorithms
- ✅ Develop autonomous navigation systems
- ✅ Build web-based robotics interfaces
- ✅ Master real-time data communication

## 🛠️ Technology Stack

| Layer | Technology |
|-------|-----------|
| **Hardware** | Raspberry Pi 5, Arduino, RPLidar A1 |
| **OS** | Ubuntu 24.04 Noble |
| **Framework** | ROS2 Jazzy Jalisco |
| **SLAM** | SLAM Toolbox |
| **Navigation** | Nav2 |
| **Communication** | Socket.IO, WebSocket |
| **Web** | React, Vite, JavaScript |
| **Language** | Python 3.10, C++, JavaScript |

## 📚 Resources

- [ROS2 Official Documentation](https://docs.ros.org/en/jazzy/)
- [Nav2 Documentation](https://navigation.ros.org/)
- [SLAM Toolbox](https://github.com/SteveMacenski/slam_toolbox)
- [RPLidar ROS](https://github.com/Slamtec/rplidar_ros)

## 🤝 Contributing

This project is open source and welcomes contributions! Visit the [Contributing Guide](extras/contributing.md) for details.

## 📞 Contact

- **Website**: [pathoven.github.io](https://pathoven.github.io)
- **GitHub**: [github.com/pathoven](https://github.com/pathoven)

---

<div style="text-align: center; margin-top: 30px;">
  <p><strong>Ready to start? Let's go! 🚀</strong></p>
  <p><a href="en/introduction/overview">📖 Project Overview →</a></p>
</div>

