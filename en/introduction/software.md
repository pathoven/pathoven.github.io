# Software Requirements

## Operating System

### Ubuntu 24.04 Noble (Recommended)
- Official support for ROS2 Jazzy
- Optimized kernel for Raspberry Pi 5
- Long-term support until 2029

#### Installation
```bash
# Download Ubuntu 24.04 image
# Flash the microSD with Raspberry Pi Imager
```

## ROS2 Jazzy Jalisco

### Why Jazzy?
- Latest ROS2 LTS release
- Native C++17 and Python 3.10 support
- Fully compatible with Nav2 and SLAM Toolbox

### Installation
```bash
sudo apt update && sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt install ros-jazzy-desktop \
                 ros-jazzy-navigation2 \
                 ros-jazzy-slam-toolbox \
                 python3-colcon-common-extensions \
                 python3-rosdep
```

## Python Packages

```bash
pip3 install --user pyserial python-socketio aiohttp psutil pytest
```

## Node.js and Web Tools

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

cd ~/bozaw_web_socketio
npm install
```

## Development Utilities (Optional)

```bash
sudo apt install git
snap install code --classic
sudo apt install terminator htop
```

## Hardware Drivers

```bash
cd ~/ros2_ws/src
git clone -b ros2 https://github.com/Slamtec/rplidar_ros.git

sudo usermod -a -G dialout $USER
# Relogin required
```

## Summary Checklist

- [ ] Ubuntu 24.04 installed
- [ ] ROS2 Jazzy base + Nav2 + SLAM Toolbox installed
- [ ] Python and Node.js dependencies ready
- [ ] RPLidar driver cloned
- [ ] User added to `dialout`

---

Next: [Project Goals](/en/introduction/goals).
