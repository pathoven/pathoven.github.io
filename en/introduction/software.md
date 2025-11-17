# Software Requirements

## Operating System

### Ubuntu 24.04 Noble (Recommended)
- Official support for ROS2 Jazzy
- Optimized for Raspberry Pi 5
- LTS (Long Term Support) release

### Installation
```bash
# Download Ubuntu 24.04 image
# Write to SD card using Raspberry Pi Imager
```

## ROS2 Jazzy Jalisco

### Why ROS2 Jazzy?
- Latest LTS release
- Full compatibility with Ubuntu 24.04
- Modern C++17 and Python 3.10+ support
- Complete Nav2 and SLAM Toolbox support

### Installation
```bash
# Add ROS2 repository
sudo apt update && sudo apt install software-properties-common
sudo add-apt-repository universe

# Install ROS2 Jazzy
sudo apt install ros-jazzy-desktop
sudo apt install ros-jazzy-navigation2
sudo apt install ros-jazzy-slam-toolbox

# Development tools
sudo apt install python3-colcon-common-extensions
sudo apt install python3-rosdep
```

## Python Packages

```bash
# Essential packages
pip3 install --user pyserial
pip3 install --user python-socketio
pip3 install --user aiohttp
pip3 install --user psutil

# Testing tools
pip3 install --user pytest
```

## Node.js and Web Tools

```bash
# Install Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Web GUI dependencies
cd ~/bozaw_web_socketio
npm install
```

## Development Tools (Optional)

```bash
# Git
sudo apt install git

# VS Code
snap install code --classic

# Terminator (multi-terminal)
sudo apt install terminator

# Htop (system monitoring)
sudo apt install htop
```

## Hardware Drivers

### RPLidar
```bash
cd ~/ros2_ws/src
git clone -b ros2 https://github.com/Slamtec/rplidar_ros.git
```

### Arduino Serial
- Arduino IDE or PlatformIO
- Add user to `dialout` group:
```bash
sudo usermod -a -G dialout $USER
# Logout/login required
```

## Summary Checklist

- [ ] Ubuntu 24.04 Noble installed
- [ ] ROS2 Jazzy installed
- [ ] Python 3.10+ and required packages
- [ ] Node.js 18+ installed
- [ ] Git configured
- [ ] RPLidar driver cloned
- [ ] Serial port permissions set

**Next:** [Project Goals →](#/en/introduction/goals)

