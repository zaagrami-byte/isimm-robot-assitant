# Tests

| Suite | Command | Hardware / ROS required |
|---|---|---|
| AI (tools, assistant, audio, VAD, wake) | `cd ai && python -m unittest discover -s tests -t . -v` | None |
| AUTO/MANUAL/ESTOP arbiter | `cd ros2_ws/src/isimm_robot && PYTHONPATH=. python -m pytest test/test_cmd_vel_arbiter.py -q` | None |
| Operator console (logic, offscreen UI, consistency) | `QT_QPA_PLATFORM=offscreen python -m pytest operator_ui/tests -q` | None |
| ROS linters (flake8, pep257) | `colcon test --packages-select isimm_robot` | ROS 2 |
| Diagnostics of the running robot | `scripts/check_robot.sh navigation` | Robot powered on (read-only) |

## What is covered

- **Tool registry**: strict allowlist, invalid arguments, unknown or uncalibrated destination, alias resolution, navigation replacement, blocking e-stop, unreachable robot, priority stop even in dry-run.
- **Assistant**: LLM-free stop fast path, tool loop, hallucinated tool refused, LLM outage, bounded history, announcements coming only from Nav2.
- **Audio**: partial frames without byte loss, oversized frames, speech segmentation, wake word.
- **Arbiter**: limits, NaN/Inf, timeouts, end of session without resuming an old goal, e-stop and rearming, ramp and direction reversal.
- **Consistency**: mux topics ↔ interface constants, manual limits ↔ `robot_params.yaml`, absence of `/cmd_vel` in the interface.

## Robot trials (recommended order)

1. Wheels off the ground, `AI_DRY_RUN=true`.
2. `tool_probe` without `--live`, then robot on the ground with an operator at the stop button.
3. `--live` on a short, clear destination.
4. Record the results.
