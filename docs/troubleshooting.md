# Troubleshooting

Start with `scripts/check_robot.sh` (read-only, PASS / WARN / FAIL).

| Symptom | Likely cause | Action |
|---|---|---|
| `/dev/isimm_*` missing | udev rules not installed, cable moved | Reload the rules; do not swap the ESP32 / LiDAR cables |
| `motor_status = NO_ARDUINO_REPLY` or `NO_SERIAL` | USB cable, `dialout` group, Uno resetting (~2 s) | Check the port and `groups`, then relaunch |
| `motor_status = ARDUINO_ESTOP` | Serial stop or physical button active | Release the button, then rearm (`clear_estop`) |
| `safety_state = SCAN_TIMEOUT` | LiDAR stopped | Check `/scan`, power supply, port |
| `HARD_STOP_FRONT` / `REAR` | Obstacle inside the protection zone | Clear it; rotation in place remains possible |
| Duplicate node / "Robot base already running" | Mapping and navigation launched together | Stop the other launch |
| Map not found at launch | Wrong `map_file` | `.posegraph` and `.data` files are required; the path must have no extension |
| `TF map->odom` missing | Localization not active / robot badly positioned | Use "2D Pose Estimate" in RViz |
| AI: `Ollama unreachable` | Service stopped | `ollama serve`, check `OLLAMA_HOST` |
| AI: `Model missing` | Model not downloaded | `ollama pull qwen2.5:3b` |
| AI: "ROS 2 unavailable" | ROS not sourced, venv created without `--system-site-packages` | `source /opt/ros/jazzy/setup.bash` or use `--no-ros` |
| No voice | Audio client not connected | `journalctl -u isimm-audio`, `AUDIO_PC_HOST`, port 5005 open |
| Microphone almost silent | Wrong device, gain too low | `python3 -m robot_audio --test-mic`, `alsamixer` |
| Robot interrupts itself / echo | Microphone not muted during playback | Adjust `AUDIO_MUTE_TAIL_MS` |
| Manual driving unavailable | Base/mapping mode, protection in fault | Read the message in the "Manual driving" tab |
| Gamepad not detected | Mapping / pygame | `python -m operator_ui --check-gamepad` |
| Multiple robots / duplicate topics | Shared `ROS_DOMAIN_ID` | Use a dedicated domain |
