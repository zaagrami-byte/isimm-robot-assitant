# Operator Console

Three tabs: **Robot status**, **LiDAR**, **Manual driving**, plus an always-visible emergency stop bar (Esc shortcut).

## Manual command chain

```
gamepad ─► operator_ui ─► /cmd_vel_teleop + /manual_session (10 Hz) ─► cmd_vel_mux_node ─► Collision Monitor ─► safety_node ─► motors
```

The interface **never** publishes `/cmd_vel`.

## Enabling manual driving

Conditions (all checked, no assumptions): ROS 2 available, robot connected, no emergency stop, **navigation mode**, protection components at OK level (`required_components`), mux responding, `safety_state = OK`, motor `OK`, gamepad connected and mapping verified.

On activation: all Nav2 goals are canceled, the interface verifies that none is active, then the mux switches to `MANUAL`. The robot only moves while **LB (dead-man switch)** is held.

| Setting | Value |
|---|---|
| Slow / normal speed | 0.10 / 0.20 m/s |
| Max reverse | 0.10 m/s |
| Rotation | 0.6 rad/s |
| Stick dead zone | 0.12 |
| Rate | 20 Hz |

The real limits are enforced on the robot side (`cmd_vel_mux_node`).

## Configuring the gamepad

```bash
python -m operator_ui --check-gamepad     # live display of axes and buttons
```
Edit `operator_ui/config/gamepad.yaml` (axes `forward`, `turn`, button `deadman`). As long as `mapping_verified: false`, tick "I have verified the mapping" at every session.

## Emergency stop and rearming

The red button (or Esc) stops locally and publishes `/emergency_stop` in a loop until the `safety_node` confirms. **Rearm** calls `clear_estop`, verifies safety and the motor controller, and restarts no movement. If the hardware stop is pressed, it cannot be rearmed through ROS.

## Tests

```bash
QT_QPA_PLATFORM=offscreen python -m pytest operator_ui/tests -q
```
