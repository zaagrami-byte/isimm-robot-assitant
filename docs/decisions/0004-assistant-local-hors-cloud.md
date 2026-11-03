# 0004 — 100% Local Assistant

**Status**: accepted

## Context
The robot is deployed in an institution, with a sometimes unstable network and visitor conversations.

## Decision
STT (faster-whisper), LLM (Qwen2.5 via Ollama) and TTS (Piper) run on a local PC; the robot only sends audio over the local network.

## Consequences
- (+) No Internet dependency, no data sent to a third party, zero running cost.
- (−) Latency and quality limited by the CPU (3B model, Whisper `small`) — measured by `make_figures.py`.
