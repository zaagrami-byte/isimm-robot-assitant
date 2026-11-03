# 0001 — LiDAR Odometry (RF2O) Without Encoders

**Status**: Accepted

## Context
The hoverboard motors used in our setup do not have usable encoders. Nav2 requires odometry (`/odom`, TF `odom → base_link`).

## Decision
Estimate motion from consecutive LiDAR scans using RF2O; control the motors in open loop (velocity → PWM).

## Consequences
- (+) No additional sensors required; odometry is independent of wheel slippage.
- (−) Environment-dependent (long, smooth corridors degrade the estimation) and limited by the LiDAR scan rate (~5–10 Hz).
- (−) PWM calibration required (see `calibration.md`); the IMU is not yet fused.
- Nav2 thresholds adjusted (progress: 0.10 m in 20 s; low speeds).

