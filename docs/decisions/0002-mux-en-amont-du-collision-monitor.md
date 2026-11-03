# 0002 — Multiplexer Upstream of the Collision Monitor

**Status**: accepted

## Context
Manual driving adds a second source of commands. It must not bypass the protections.

## Decision
`cmd_vel_mux_node` receives `/cmd_vel_smoothed` (Nav2) and `/cmd_vel_teleop` (interface) and publishes `/cmd_vel_mux`, which is read by the Collision Monitor. The mux never publishes `/cmd_vel`. Manual driving is only allowed in navigation mode.

## Consequences
- (+) Manual commands go through the Collision Monitor and then the `safety_node`.
- (+) Arbitration is testable without ROS (`cmd_vel_arbiter.py`), and an old goal is never resumed (`AUTO_WAIT`).
- (−) One more dependency to monitor (`cmd_vel_mux` in `/robot_status`, required to enable manual driving).
