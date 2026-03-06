<div align="center">

# 🏗️ nodejs-project-arch

**AI-friendly Node.js project architecture standards**

*Keep every file under 400 lines. Save 70-93% tokens. Get 3x more productive AI rounds.*

[![ClawHub](https://img.shields.io/badge/ClawHub-nodejs--project--arch-blue?style=flat-square)](https://clawhub.com/skills/nodejs-project-arch)
[![GitHub stars](https://img.shields.io/github/stars/abczsl520/nodejs-project-arch?style=flat-square)](https://github.com/abczsl520/nodejs-project-arch/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

[Install](#-installation) · [Game Arch](#-h5-game) · [Tool Arch](#-data-tool--api--dashboard) · [SDK Arch](#-sdklibrary) · [Wiki](https://github.com/abczsl520/nodejs-project-arch/wiki)

</div>

---

## 🤯 The Problem

AI agents working with large codebases hit the context window wall fast:

```
3000-line server.js  →  ~40K tokens per read  →  20% of context gone
After 3-5 rounds     →  context compression   →  agent forgets everything
```

## ✅ The Solution

Split by function. Each file stays small. AI reads only what it needs:

```
200-line module      →  ~2.7K tokens per read  →  1.3% of context
After 10-15 rounds   →  still going strong     →  no compression needed
```

### Real-World Token Savings

| Scenario | Before | After | Savings |
|----------|--------|-------|---------|
| Read one game feature | 40K tokens | 2.7K tokens | **93%** |
| Full dev round (read→edit→verify) | 52K tokens | 4K tokens | **92%** |
| 4-round SDK session | 196K tokens | 69K tokens | **65%** |
| Large data tool module | 84K tokens | 8K tokens | **90%** |

## 📐 Core Rules

```
✅ Single file        ≤ 400 lines
✅ index.html         ≤ 200 lines
✅ server.js (entry)  ≤ 100 lines
✅ Tunable values     → config.json (hot-reloadable)
✅ Backend            → routes/ by domain + services/ for shared logic
✅ Frontend           → HTML skeleton + separate JS/CSS files
✅ Every project      → admin.html + routes/admin.js
```

## 🎮 H5 Game

```
game-name/
  server.js            ← Entry, routes only (<100 lines)
  config.json          ← Gameplay values (admin can edit live)
  routes/
    auth.js            ← Login/user
    game.js            ← Game data API
    social.js          ← Leaderboard/sharing
    admin.js           ← Admin API + config CRUD
  public/
    index.html         ← HTML skeleton (<200 lines)
    admin.html         ← Admin dashboard
    css/style.css
    js/
      config.js        ← Fetch config at startup
      game.js          ← Core game logic
      renderer.js      ← Rendering/visuals
      ui.js            ← UI interactions
      audio.js         ← Sound effects
    assets/
```

## 🔧 Data Tool / API / Dashboard

```
project-name/
  server.js            ← Entry (<100 lines)
  config.json          ← API keys, schedules, thresholds
  db.js                ← DB init + helpers
  routes/
    api.js             ← Query/export API
    task.js            ← Task management
    admin.js           ← Admin API
  services/
    crawler.js         ← Business logic
    scheduler.js       ← Cron/intervals
  public/
    index.html         ← HTML skeleton (<200 lines)
    admin.html         ← Admin dashboard
    css/style.css
    js/
      app.js           ← Main logic
      table.js         ← Table rendering
      api.js           ← Fetch wrapper
```

## 📦 SDK/Library

```
sdk-name/
  src/                 ← Source modules (<200 lines each)
    core.js / auth.js / api.js / ui.js
  sdk-name.js          ← Built output (merged single file)
  build.js             ← Simple merge script
  test/test.html       ← Test page
  CHANGELOG.md
```

Dev in `src/` (small files for AI) → build merges into single file for consumers.

## 🔑 config.json Pattern

```javascript
// Load + serve (strip secrets)
const config = JSON.parse(fs.readFileSync('./config.json', 'utf8'));
app.get('/api/config', (req, res) => {
  const safe = { ...config };
  delete safe.admin;
  res.json(safe);
});

// Admin hot-reload (no restart needed)
app.post('/admin/config', requireAdmin, (req, res) => {
  fs.writeFileSync('./config.json.bak', fs.readFileSync('./config.json'));
  fs.writeFileSync('./config.json', JSON.stringify(req.body, null, 2));
  Object.assign(config, req.body);
  res.json({ ok: true });
});
```

## 🛡️ Admin Dashboard

Every project gets `admin.html`:
- 🔐 Password login (`x-admin-password` header)
- ⚙️ Visual config editor with save + hot-reload
- 📊 Stats overview (users / data / uptime)
- 📋 Config backup history + one-click restore

## 🚀 Installation

### OpenClaw / ClawHub

```bash
clawhub install nodejs-project-arch
```

### Manual

```bash
git clone https://github.com/abczsl520/nodejs-project-arch.git
cp -r nodejs-project-arch ~/.agents/skills/
```

The skill **auto-activates** when you ask your AI to:
- *"Create a new Node.js project"*
- *"Refactor this large codebase"*
- *"Split this file, it's too big"*

## 📖 Documentation

Full docs on the [Wiki](https://github.com/abczsl520/nodejs-project-arch/wiki):

| Page | Description |
|------|-------------|
| [Game Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/Game-Architecture) | H5 game standards + WeChat compatibility |
| [Tool Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/Tool-Architecture) | Data tools, dashboards, API services |
| [SDK Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/SDK-Architecture) | Libraries with build step |
| [Config Pattern](https://github.com/abczsl520/nodejs-project-arch/wiki/Config-Pattern) | Universal config.json pattern |
| [Admin Dashboard](https://github.com/abczsl520/nodejs-project-arch/wiki/Admin-Dashboard) | Admin panel standards |
| [Token Savings](https://github.com/abczsl520/nodejs-project-arch/wiki/Token-Savings) | Detailed savings analysis |

## 🔗 Part of the AI Dev Quality Suite

| Skill | Purpose | Install |
|-------|---------|---------|
| [bug-audit](https://github.com/abczsl520/bug-audit-skill) | Dynamic bug hunting, 200+ pitfall patterns | `clawhub install bug-audit` |
| [codex-review](https://github.com/abczsl520/codex-review) | Three-tier code review with adversarial testing | `clawhub install codex-review` |
| [debug-methodology](https://github.com/abczsl520/debug-methodology) | Root-cause debugging, prevents patch-chaining | `clawhub install debug-methodology` |
| **nodejs-project-arch** (this) | AI-friendly architecture, 70-93% token savings | `clawhub install nodejs-project-arch` |
| [game-quality-gates](https://github.com/abczsl520/game-quality-gates) | 12 universal game dev quality checks | `clawhub install game-quality-gates` |

## 🤝 Works With

- [OpenClaw](https://openclaw.ai) — AI agent framework
- [ClawHub](https://clawhub.com) — Skill marketplace
- Any AI coding agent (Claude, GPT, Codex, Gemini...)

## 📄 License

MIT — use it, fork it, improve it.

---

<div align="center">

**If this saves you tokens, give it a ⭐**

</div>
