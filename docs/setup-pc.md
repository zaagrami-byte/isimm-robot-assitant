# Installation — PC (AI and operator console)

Prerequisites: Ubuntu 24.04, ROS 2 Jazzy (for the ROS bridge), Python 3.12, [Ollama](https://ollama.com).

## AI assistant

```bash
source /opt/ros/jazzy/setup.bash
cd ~/isimm/ai
python3 -m venv --system-site-packages venv      # required for rclpy
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env                              # adapt PC_IP, ROBOT_IP, ROS_DOMAIN_ID
ollama pull qwen2.5:3b
mkdir -p models        # put fr_FR-siwis-medium.onnx and fr_FR-siwis-medium.onnx.json here (Piper voice)
```

> `numpy < 2` is enforced for ABI compatibility with `rclpy`.

### Run

| Need | Command |
|---|---|
| Test the LLM alone | `python main.py --text --no-ros` |
| Text + robot, **simulation** | `python main.py --text` |
| Voice + robot, simulation | `python main.py --voice` |
| **Real motion** | `python main.py --voice --live` |

`AI_DRY_RUN=true` by default: `navigate_to` does not move the robot. The stop command, however, is never simulated.

### Probes without the LLM

```bash
python -m scripts.tool_probe --list
python -m scripts.tool_probe get_robot_status
python -m scripts.tool_probe navigate_to '{"location":"accueil"}'        # add --live for real motion
python -m scripts.audio_probe record --seconds 5 --out /tmp/robot_mic.wav
python -m scripts.audio_probe speak "Bonjour, je suis le robot."
python -m scripts.speech_probe stt file.wav
python -m scripts.speech_probe tts "Bonjour" --out /tmp/t.wav
```

### User service (automatic start)

```bash
mkdir -p ~/.config/systemd/user && cp deploy/systemd/isimm-ai.service ~/.config/systemd/user/
# adapt WorkingDirectory / ExecStart to the actual repository path
systemctl --user daemon-reload && systemctl --user enable --now isimm-ai
```

## Operator console

```bash
source /opt/ros/jazzy/setup.bash
python3 -m venv --system-site-packages operator_ui/venv && source operator_ui/venv/bin/activate
pip install -r operator_ui/requirements.txt
export ROS_DOMAIN_ID=0
python -m operator_ui --check-gamepad    # check axes / buttons first
python -m operator_ui
```

See [operator-ui.md](operator-ui.md).
