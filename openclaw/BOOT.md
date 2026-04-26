# BOOT.md - Silent Startup

## 1. Task State
Read `~/openclaw/task_state.json`:
- Incomplete step found → "Resuming `<task_id>`: on `<current_step>`. Last: `<last_result>`. Continue?"
- All done or empty → silent
- Missing → create blank template silently

## 2. Git Check
Run `git -C ~/Github/OpenClaw_General status --short`:
- Uncommitted work → "Uncommitted work in `<project>`. Commit and push before continuing?"
- Clean → silent

## Active Projects
- `~/Github/OpenClaw_General` → github.com/UnicornStartUp-project/OpenClaw_General.git
