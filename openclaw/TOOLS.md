# TOOLS.md

## Task State
Path: `~/openclaw/task_state.json`
Blank template:
```json
{
  "schema_version": "1.0",
  "task_id": "",
  "plan": [],
  "current_step": "",
  "completed_steps": [],
  "context_summary": "",
  "last_result": "",
  "metadata": {"created_at": "", "last_updated": "", "session_count": 0}
}
```
completed_steps[] is source of truth — never re-run a step in that array.

## Environment Notes
_(Add: camera names, SSH hosts, TTS voices, device nicknames)_

## Memory Sync
When major topic concludes in a channel: synthesize key decisions → update MEMORY.md → done.
