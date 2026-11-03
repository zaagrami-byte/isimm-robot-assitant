# Installation — Raspberry Pi (robot)

Prerequisites: Ubuntu 24.04 + ROS 2 Jazzy, user in the `dialout` and `audio` groups.

## 1. Packages

```bash
sudo apt update
sudo apt install -y ros-jazzy-navigation2 ros-jazzy-nav2-bringup ros-jazzy-slam-toolbox \
  ros-jazzy-rplidar-ros ros-jazzy-xacro ros-jazzy-robot-state-publisher ros-jazzy-rviz2 \
  python3-serial python3-colcon-common-extensions alsa-utils libeigen3-dev libboost-dev
```

## 2. Repository and build

```bash
git clone https://github.com/zaagrami-byte/cpuisimm-assistant.git ~/isimm
cd ~/isimm/ros2_ws && source /opt/ros/jazzy/setup.bash
colcon build --symlink-install && source install/setup.bash
```

## 3. Devices

udev rules and groups: see [hardware.md](hardware.md). Check with: `ls -l /dev/isimm_*`.

## 4. ROS 2 network

Same `ROS_DOMAIN_ID` on the Pi and the PC (e.g. `export ROS_DOMAIN_ID=0` in `~/.bashrc`) and same subnet.

## 5. Mapping (once)

```bash
ros2 launch isimm_robot mapping.launch.py
# drive the robot slowly, then serialize the map:
mkdir -p ~/isimm/maps
ros2 service call /slam_toolbox/serialize_map slam_toolbox/srv/SerializePoseGraph \
  "{filename: '/home/<user>/isimm/maps/isimm_main'}"
```

⚠️ Stop mapping before launching navigation.

## 6. Navigating

```bash
export ISIMM_MAP_FILE=$HOME/isimm/maps/isimm_main     # or map_file:=...
ros2 launch isimm_robot navigation.launch.py
```

Place the robot at the map's starting point (`map_start_at_dock: true`) or use "2D Pose Estimate" in RViz.

## 7. Audio client (service)

```bash
sudo mkdir -p /etc/isimm
sudo tee /etc/isimm/audio.env <<'ENV'
AUDIO_PC_HOST=192.168.1.20
AUDIO_PORT=5005
AUDIO_CAPTURE_DEVICE=plughw:1,0
AUDIO_PLAYBACK_DEVICE=plughw:1,0
ENV
sudo cp deploy/systemd/isimm-audio.service /etc/systemd/system/
sudo systemctl daemon-reload && sudo systemctl enable --now isimm-audio
```

Choose the devices with `python3 -m robot_audio --list-devices`, then run `--test-mic` and `--test-speaker`.

## 8. Verify

```bash
scripts/check_robot.sh            # automatic mode detection; read-only
```
