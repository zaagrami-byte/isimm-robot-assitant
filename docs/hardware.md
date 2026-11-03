# Hardware

## Bill of materials

| Item | Detail (source: code and URDF) |
|---|---|
| Robot computer | Raspberry Pi 4 (ROS 2 Jazzy, Ubuntu 24.04) |
| AI computer | CPU PC (Whisper, Ollama, Piper, operator console) |
| LiDAR | RPLIDAR, 115200 baud (A1 according to `slam_params.yaml`; **exact model to be confirmed** — a C1 uses 460800) |
| Motor controller | Arduino UNO (VID:PID `2341:0043`) |
| IMU | ESP32 + MPU6050 (I²C 100 kHz) |
| Drive | 2 hoverboard motors, wheel radius 0.0825 m, track width 0.48 m |
| Chassis | 0.57 × 0.40 × 0.30 m; LiDAR at the center, 0.24 m above the ground |
| Audio | ALSA microphone and speaker on the Pi |

## Arduino UNO pinout

| Function | Left | Right |
|---|:--:|:--:|
| PWM | 5 | 10 |
| DIR | 6 | 9 |
| BRAKE | 7 | 8 |

| Other | Pin |
|---|:--:|
| Emergency stop button (to GND, `INPUT_PULLUP`, interrupt) | 2 |
| Status LED (on = safe / stopped) | 4 |

The right motor's direction is inverted in the firmware (`INVERT_RIGHT_DIR = true`). All outputs use `analogWrite()` (0 = low, 255 = high).

Serial protocol (115200 baud, `\n`):
`M,<lpwm>,<ldir>,<lbrk>,<rpwm>,<rdir>,<rbrk>` · `S` emergency stop · `C` rearm · `P` ping (→ `OK`) · startup: `READY`.

## ESP32 / MPU6050

| MPU6050 | ESP32 |
|---|---|
| VCC | 3.3 V |
| GND | GND |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

Serial output: `I,ax,ay,az,gx,gy,gz,temp` at 20 Hz (m/s², rad/s, °C). Planned ultrasonic sensors: `U,fl,fr,rl,rr`.
The MPU6050 is raw: orientation is not estimated (`covariance[0] = -1`) and the IMU is not fused into the odometry.

## USB ports (udev)

Rules in `ros2_ws/src/isimm_robot/udev/99-isimm-robot.rules`:

| Link | Identification |
|---|---|
| `/dev/isimm_arduino` | VID:PID `2341:0043` |
| `/dev/isimm_esp32` | CP2102 on physical USB port `1-1.2` |
| `/dev/isimm_rplidar` | CP2102 on physical USB port `1-1.3` |

The ESP32 and the RPLIDAR use the same chip: **do not swap the cables**. Access is through the `dialout` group.

```bash
sudo cp ros2_ws/src/isimm_robot/udev/99-isimm-robot.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
sudo usermod -aG dialout $USER && sudo usermod -aG audio $USER   # then log out and back in
```
