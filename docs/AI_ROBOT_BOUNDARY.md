# AI / Robot Core Boundary

    AI = understanding + conversation + high-level tools
    ROBOT CORE = physical control + safety + ROS 2 + Nav2
    AI → requests        ROBOT CORE → decides how to execute safely

## The AI can
Understand, answer, speak, read state/pose, request `NavigateToPose`, cancel, request a stop
(`/emergency_stop`), observe and report anomalies.

## The AI can never
Publish `/cmd_vel`; touch PWM, gains, footprint, Nav2, costmaps, collision_monitor, safety_node, TF,
odom, LiDAR or SLAM; run a shell command; rearm the emergency stop; call a tool outside the
allowlist (`ai/tools/__init__.py`).

## Calibration
"Calibrate the robot" → the AI explains that this is an engineering operation and changes NOTHING.
It may relay observations (e.g. measured speed < requested speed); the correction is made
only in `ros2_ws/` (`robot_params.yaml`, `nav2_params.yaml`…) by an engineer.

## Contribution rule
No code under `ai/` may import a `geometry_msgs/Twist` message for publishing, or write
into `ros2_ws/`. A new tool = one explicit `Tool` in the registry + one test in `ai/tests/`.
