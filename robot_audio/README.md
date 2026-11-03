# robot_audio — Robot Audio Client (Raspberry Pi)

No AI model, **standard library only**. The microphone (`arecord`) is streamed to the PC, and the voice (`aplay`) is received from the PC.

```bash
python3 -m robot_audio                  # service
python3 -m robot_audio --list-devices   # arecord -l / aplay -l
python3 -m robot_audio --test-mic       # microphone level (peak / RMS)
python3 -m robot_audio --test-speaker   # 440 Hz beep
```

Configured through `AUDIO_*` variables (see `/etc/isimm/audio.env`): `AUDIO_PC_HOST` (required), `AUDIO_PORT` (5005), `AUDIO_CAPTURE_DEVICE`, `AUDIO_PLAYBACK_DEVICE`, `AUDIO_MIC_RATE` (16000), `AUDIO_FRAME_MS` (20), `AUDIO_MUTE_TAIL_MS` (400), `AUDIO_PING_INTERVAL_S`, `AUDIO_PEER_TIMEOUT_S`, `AUDIO_ROBOT_ID`.

Automatic reconnection with backoff (1 → 5 s), heartbeat, microphone muted while the robot speaks.
Protocol: [`docs/architecture.md`](../docs/architecture.md#audio-protocol-robot--pc).
