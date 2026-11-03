# Contributing

## Principles
1. **Safety first**: no code under `ai/` publishes `/cmd_vel`, modifies `ros2_ws/` or runs a shell.
2. A new AI tool = one explicit `Tool` in `ai/tools/` + one test in `ai/tests/`.
3. Any change to the command chain updates `docs/safety.md` and `operator_ui/tests/test_consistency.py`.
4. Measured or calibrated values are never invented: state the conditions and the number of trials.

## Workflow
```bash
git checkout -b feature/<topic>
pip install pre-commit && pre-commit install
# develop, then:
cd ai && python -m unittest discover -s tests -t .
QT_QPA_PLATFORM=offscreen python -m pytest operator_ui/tests -q
```
Commit messages: `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`.
Open a Pull Request using the provided template; CI must be green.

## Style
- Python 3.12, lines ≤ 120 characters (`ruff`), type annotations on public APIs.
- C++17, builds without warnings (`-Wall -Wextra -Wpedantic`).
- Documentation in English; code identifiers in English. (Existing code comments and the assistant's spoken prompts are in French.)

## License of contributions
By submitting a Pull Request, you agree that your contribution is distributed under the
PolyForm Noncommercial License 1.0.0, and you grant the author (Rami ZAAG) the right to
redistribute it, including under a separate commercial license.
