> The project's original code is under PolyForm Noncommercial 1.0.0 (see `LICENSE`). The components below keep their original license.
# Third-party components

| Component | Use | License |
|---|---|---|
| [RF2O](http://mapir.isa.uma.es/work/rf2o) (MAPIR, UMA) — `ros2_ws/src/rf2o_laser_odometry` | LiDAR odometry | **GPL v3** (the folder keeps its `LICENSE`) |
| [ROS 2 Jazzy](https://docs.ros.org/en/jazzy/) | Middleware | Apache 2.0 |
| [Nav2](https://nav2.org) | Navigation | Apache 2.0 |
| [SLAM Toolbox](https://github.com/SteveMacenski/slam_toolbox) | Mapping / localization | LGPL |
| [rplidar_ros](https://github.com/Slamtec/rplidar_ros) | LiDAR driver | BSD |
| [faster-whisper](https://github.com/SYSTRAN/faster-whisper) | Speech recognition | MIT |
| [Piper](https://github.com/rhasspy/piper) + `fr_FR-siwis-medium` voice | Speech synthesis | To be verified |
| [Ollama](https://ollama.com) + Qwen2.5 | Local LLM | To be verified |
| [webrtcvad](https://github.com/wiseman/py-webrtcvad) | Speech detection | MIT |
| [PySide6](https://doc.qt.io/qtforpython-6/) | Interface | LGPL |
| [pygame](https://www.pygame.org) | Gamepad input | LGPL |
| Adafruit MPU6050 | IMU firmware | BSD |

## Modifications made to RF2O

- Port to ROS 2 Jazzy (C++17), namespace properly closed.
- Valid initial quaternion (identity) and normalization.
- Sensor QoS (`SensorDataQoS`) compatible with the RPLIDAR driver.
- TF `base_link → laser` required before initialization.
- Throttled logging.
- Fix: `laser_oldpose_ = laser_oldpose_` → `laser_oldpose_ = laser_pose_` in `init()`.

> The licenses of the models (Piper voice, Qwen) must be verified before any redistribution.
