# operator_ui — ISIMM Robot Operator Interface (PC)

Supervision (3 tabs: Status, LiDAR, Manual driving) and gamepad-based manual driving.
It touches neither `ai/` nor `/cmd_vel`: manual commands go through `/cmd_vel_teleop`
→ `cmd_vel_mux_node` (Pi) → Collision Monitor → `safety_node` → motors.

## Command chain (navigation mode)
```
velocity_smoother → /cmd_vel_smoothed ─┐
PC interface      → /cmd_vel_teleop  ──┴→ cmd_vel_mux_node → /cmd_vel_mux → collision_monitor
                                                → /cmd_vel → safety_node → /safe_cmd_vel → motor_controller → Arduino
```
Added topics: `/cmd_vel_mode` (String: AUTO | AUTO_WAIT | MANUAL | ESTOP), `/manual_session`
(Bool, 10 Hz heartbeat from the interface). See `ros2_ws/.../cmd_vel_mux_node.py`.

## Installation (PC, Ubuntu 24.04 + ROS 2 Jazzy)
```bash
source /opt/ros/jazzy/setup.bash
cd ~/isimm                      # repository root
python3 -m venv --system-site-packages operator_ui/venv
source operator_ui/venv/bin/activate
pip install -r operator_ui/requirements.txt
```
## Launch
```bash
source /opt/ros/jazzy/setup.bash && source operator_ui/venv/bin/activate
export ROS_DOMAIN_ID=0          # same as the Pi
python -m operator_ui
python -m operator_ui --check-gamepad     # check axes/buttons without ROS
```
## Configuration
- `config/teleop.yaml`: speeds (0.10 / 0.20 / reverse 0.10 m/s, 0.6 rad/s), dead zone 0.12, 20 Hz.
  The real limits are enforced on the robot side (`robot_params.yaml` → `cmd_vel_mux_node`).
- `config/gamepad.yaml`: gamepad mapping (CANDIDATE indices, to be verified). As long as
  `mapping_verified: false`, you must tick "I have verified the mapping" at every session.

## Tests
```bash
QT_QPA_PLATFORM=offscreen python -m pytest operator_ui/tests -q
cd ros2_ws/src/isimm_robot && PYTHONPATH=. python -m pytest test/test_cmd_vel_arbiter.py -q
```
