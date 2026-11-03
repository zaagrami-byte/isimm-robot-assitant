# ros2_ws — ROS 2 Jazzy Workspace

| Package | Type | Role |
|---|---|---|
| `isimm_robot` | `ament_python` | Robot nodes, launch files, configuration, URDF, RViz, udev rules |
| `rf2o_laser_odometry` | `ament_cmake` (C++17) | LiDAR odometry (GPL v3, see `THIRD_PARTY.md`) |

## Build

```bash
source /opt/ros/jazzy/setup.bash
cd ros2_ws && colcon build --symlink-install && source install/setup.bash
```

## Launches

| Command | Mode | Notes |
|---|---|---|
| `ros2 launch isimm_robot robot.launch.py` | base | Diagnostics; `mode:=base\|mapping\|navigation` |
| `ros2 launch isimm_robot mapping.launch.py` | mapping | Asynchronous SLAM Toolbox |
| `ros2 launch isimm_robot navigation.launch.py map_file:=<path without extension>` | navigation | `ISIMM_MAP_FILE` supported; `use_rviz:=true` |

Never launch *mapping* and *navigation* together (`check_duplicates` safeguard).

## Configuration

| File | Contents |
|---|---|
| `config/robot_params.yaml` | LiDAR, RF2O, motors, safety, mux, status |
| `config/nav2_params.yaml` | Nav2, costmaps, Collision Monitor |
| `config/slam_params.yaml` / `slam_localization_params.yaml` | Mapping / localization |
| `urdf/robot.urdf.xacro` | Model (0.57 × 0.40 m, centered LiDAR) |

## Main topics

| Topic | Type | Producer |
|---|---|---|
| `/scan` | `LaserScan` | `rplidar_node` |
| `/odom` + TF `odom→base_link` | `Odometry` | RF2O |
| `/imu` | `Imu` | `esp32_sensor_node` |
| `/cmd_vel_smoothed` → `/cmd_vel_mux` → `/cmd_vel` → `/safe_cmd_vel` | `Twist` | smoother → mux → collision monitor → safety |
| `/cmd_vel_teleop`, `/manual_session` | `Twist`, `Bool` | `operator_ui` |
| `/cmd_vel_mode` | `String` | `cmd_vel_mux_node` (AUTO, AUTO_WAIT, MANUAL, ESTOP) |
| `/emergency_stop` | `Bool` | AI / interface |
| `/safety_state`, `/motor_status` | `String` | `safety_node`, `motor_controller_node` |
| `/robot_status` | `DiagnosticArray` | `robot_status_node` |

## Tests

```bash
cd ros2_ws/src/isimm_robot && PYTHONPATH=. python -m pytest test/test_cmd_vel_arbiter.py -q
colcon test --packages-select isimm_robot
```

Documentation: [`docs/architecture.md`](../docs/architecture.md) · [`docs/safety.md`](../docs/safety.md).
