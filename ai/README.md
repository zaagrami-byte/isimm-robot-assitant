# AI — Voice Assistant (PC)

Local pipeline: **Robot microphone → VAD → Whisper → Qwen (tools) → Piper → Robot speaker**

```bash
python main.py --text --no-ros     # Development without a microphone or robot
python main.py --voice             # Voice over the network (simulation by default)
python main.py --voice --live      # Real robot motion
```

| Directory | Contents |
|---|---|
| `assistant/` | LLM/tool loop, allowlist registry, prompt, short-term memory, voice loop |
| `tools/` | 7 tools: status, pose, destinations, navigation, stop |
| `ros2_interface/` | `RobotInterface` contract, ROS 2 bridge, Nav2 client |
| `audio/` | TCP audio server, VAD, protocol (linked to `robot_audio/protocol.py`) |
| `speech/` | faster-whisper, Piper |
| `knowledge/` | `locations.yaml`, `isimm_knowledge.yaml` |
| `config/` | `assistant.yaml` (`.env` takes precedence) |
| `scripts/` | Probes: tools, audio, speech, position recording |
| `tests/` | Tests without ROS or hardware |

**Rules:** See [`docs/AI_ROBOT_BOUNDARY.md`](../docs/AI_ROBOT_BOUNDARY.md) and [`docs/ai-assistant.md`](../docs/ai-assistant.md).

**Installation:** [`docs/setup-pc.md`](../docs/setup-pc.md).
