# Raspberry Pi Setup

## Base Image

1. Flash Ubuntu 24.04 Server to the microSD card.
2. Boot and complete the first-run wizard.
3. Update packages:
```bash
sudo apt update && sudo apt upgrade -y
```

## System Tweaks

```bash
sudo timedatectl set-timezone Europe/Istanbul
sudo systemctl enable ssh
sudo apt install net-tools vim tmux bash-completion
```

Add these lines to `~/.bashrc` for ROS2:
```bash
source /opt/ros/jazzy/setup.bash
source ~/ros2_ws/install/setup.bash
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
```

## Workspace

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build --symlink-install
```

## Autostart Services

Create `/etc/systemd/system/pathoven_bringup.service`:
```ini
[Unit]
Description=Pathoven Bringup
After=network-online.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/ros2_ws
ExecStart=/usr/bin/tmux new-session -s pathoven -d 'source /home/ubuntu/.bashrc && ros2 launch pathoven_bringup base.launch.py'
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now pathoven_bringup.service
```

## Verification

```bash
ros2 topic list
ros2 node list
```

You should see topics such as `/scan`, `/ekf/odom`, and `/cmd_vel`.
