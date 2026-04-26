# CLAUDE.md — OpenClaw Context Engineering
# Claude Code reads this file automatically on workspace open.
# Version control: GitHub  →  UnicornStartUp-project/openclaw-context (or your repo name)
# Deploy target:   ~/.openclaw/agents/default/

---

## Repo Structure

```
openclaw-context/
├── CLAUDE.md          ← this file (Claude Code reads first)
├── IDENTITY.md        ← injection-resistance anchor (loads first in OpenClaw)
├── SOUL.md            ← agent personality & hard limits
├── AGENTS.md          ← session-start protocol + operating rules
├── USER.md            ← operator context (Minkuk, Mac Mini M4, stack)
├── TOOLS.md           ← environment, commands, troubleshooting ref
├── MEMORY.md          ← curated long-term memory (agent updates this)
├── HEARTBEAT.md       ← scheduled tasks (08:00 check, 22:00 memory capture)
├── DEPLOY.md          ← step-by-step deployment guide
├── scripts/
│   ├── deploy.sh      ← copies files → ~/.openclaw/agents/default/
│   └── sync-memory.sh ← pulls MEMORY.md from OpenClaw → repo
└── memory/
    └── YYYY-MM-DD.md  ← daily note template (git-ignored in production)
```

---

## Git Workflow for Claude Code

### Initial setup (run once)
```bash
git init
git remote add origin https://github.com/UnicornStartUp-project/openclaw-context.git
git branch -M main
git add .
git commit -m "init: openclaw context engineering architecture"
git push -u origin main
```

### Pull latest from GitHub → local
```bash
git pull origin main
```

### Deploy local → OpenClaw (after pull or edit)
```bash
bash scripts/deploy.sh
```

### After OpenClaw updates MEMORY.md → sync back to repo
```bash
bash scripts/sync-memory.sh
git add MEMORY.md
git commit -m "memory: sync from OpenClaw session $(date +%Y-%m-%d)"
git push origin main
```

### Edit a file and push
```bash
# Edit the file in VS Code, then:
git add <filename>
git commit -m "update: <what changed and why>"
git push origin main
```

### View change history for a file
```bash
git log --oneline -- AGENTS.md
git diff HEAD~1 AGENTS.md
```

---

## Deploy Commands (also in scripts/deploy.sh)

```bash
DEST=~/.openclaw/agents/default

# Backup
cp -r $DEST $DEST.backup.$(date +%Y%m%d-%H%M)

# Copy context files
cp IDENTITY.md  $DEST/IDENTITY.md
cp SOUL.md      $DEST/SOUL.md
cp AGENTS.md    $DEST/AGENTS.md
cp USER.md      $DEST/USER.md
cp TOOLS.md     $DEST/TOOLS.md
cp MEMORY.md    $DEST/MEMORY.md
cp HEARTBEAT.md $DEST/HEARTBEAT.md
mkdir -p $DEST/memory

# Restart gateway
openclaw gateway stop && openclaw gateway start
sleep 10 && openclaw gateway status
```

---

## openclaw.json Required Settings

```json
{
  "model": "ollama/gemma4:e4b-it-q4_K_M",
  "bootstrapMaxChars": 12000,
  "bootstrapTotalMaxChars": 16000
}
```

---

## Smoke Test

Send to Discord bot DM after deploy:
> "Run your session-start protocol and confirm context loaded."

Expected first line: `🔍 Context loaded: [USER ✓] [MEMORY ✓] [TOOLS ✓] [Daily notes: none/found]`

If missing: AGENTS.md wasn't loaded → check `bootstrapTotalMaxChars` limit → run `du -c ~/.openclaw/agents/default/*.md`
