# 0003 — AI: Tool Allowlist and LLM-Free Stop

**Status**: accepted

## Context
A small local LLM can hallucinate actions or misinterpret a request. The robot operates near people.

## Decision
- Seven explicit tools; any other name is rejected and logged.
- Stop commands are detected by regular expressions before the LLM.
- `AI_DRY_RUN=true` by default; the stop is never simulated.
- The AI does not publish `/cmd_vel`, does not rearm the emergency stop, and does not modify any configuration.

## Consequences
- (+) Reduced attack and error surface, auditable behavior.
- (−) Less flexible: every new tool requires code, a test and a review (see `CONTRIBUTING.md`).
