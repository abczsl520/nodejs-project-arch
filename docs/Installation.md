# Installation

## For OpenClaw Users

Copy the skill folder to your skills directory:

```bash
# Clone the repo
git clone https://github.com/abczsl520/nodejs-project-arch.git

# Copy to OpenClaw skills
cp -r nodejs-project-arch ~/.openclaw/skills/
# or
cp -r nodejs-project-arch ~/.agents/skills/
```

Then restart OpenClaw. The skill auto-triggers when you create or refactor Node.js projects.

## For Other AI Agents

Copy the `SKILL.md` and `references/` folder into your agent's skill/knowledge directory. The agent reads `SKILL.md` first, then loads the relevant reference file based on project type.

## Manual Use

You can also just read the reference files directly:
- `references/game.md` — H5 game architecture
- `references/tool.md` — Non-game project architecture (data tools, dashboards, APIs)
- `references/sdk.md` — SDK/library architecture
