# Calibration (engineering operation)

> The AI assistant **calibrates nothing**: it can only relay observations. Every correction is made in `ros2_ws/` by an engineer.

Parameters to calibrate in `ros2_ws/src/isimm_robot/config/robot_params.yaml` (`motor_controller_node`):

| Parameter | Role | Current value | Status |
|---|---|---|---|
| `left_min_pwm`, `right_min_pwm` | Minimum PWM that actually turns the wheel (static friction) | 25 | To calibrate |
| `wheel_speed_min_effective` | Wheel speed associated with `min_pwm` | 0.05 m/s | To calibrate |
| `wheel_speed_at_max_pwm` | Wheel speed at `max_pwm` | 0.64 m/s | To calibrate |
| `left_gain`, `right_gain` | Static left/right compensation (bounded to [0.5, 1.5]) | 1.0 | To calibrate |
| `max_pwm` | Safety ceiling | 100 | Validated for first tests, **do not increase** without validation |
| `wheel_separation` | Track width (must equal the URDF) | 0.48 m | Validated |

## Suggested protocol (to be adapted)

1. **Wheels off the ground**, check the rotation direction (`left_invert`, `right_invert`, or `INVERT_*_DIR` in the firmware).
2. Find each wheel's breakaway PWM (increase in steps of 1 until it moves) → `*_min_pwm`.
3. **On the ground**, drive a 2 m straight line at constant speed: measure the real distance against the RF2O odometry.
4. If the robot drifts, adjust `left_gain` / `right_gain` in small steps (≈ 0.01).
5. Rotate in place by 360°: compare the real angle with the estimated angle.
6. Record the results (date, floor, battery, number of trials).

Diagnostic aid: the `observations` field of `get_robot_status` reports when the measured speed is below 50% of the requested speed.
