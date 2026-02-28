# nodejs-project-arch

**Node.js project architecture standards for AI-assisted development.**

An [OpenClaw](https://openclaw.ai) / AI agent skill that enforces modular file structures, keeping every file small enough for AI to read and edit without blowing the context window.

## ✨ Why?

When AI agents work with large single-file codebases, things go wrong fast:

| Scenario | Tokens per read | Context usage (200K) |
|----------|----------------|---------------------|
| 3000-line `server.js` | ~40K | 20% |
| 200-line module | ~2.7K | 1.3% |

**Result:** Splitting files means **10-15 productive rounds** before context compression, vs 3-5 with monolithic files. That's **70-93% token savings** per file read.

## 📐 Core Rules

- **Single file ≤ 400 lines** / `index.html` ≤ 200 lines / `server.js` entry ≤ 100 lines
- **All tunable values** in `config.json` — loaded at runtime, editable via admin dashboard
- **Backend:** `routes/` by domain, `services/` for shared logic, `db.js` for database init
- **Frontend:** HTML skeleton only, JS/CSS in separate files under `public/`
- **Every project** gets `admin.html` + `routes/admin.js` for config hot-reload + stats

## 🎮 Supported Project Types

| Type | Signals | Reference |
|------|---------|-----------|
| **H5 Game** | Canvas, Phaser, Matter.js, game loop, sprites | [references/game.md](references/game.md) |
| **Data Tool** | Crawler, scraper, scheduler, data sync, analytics | [references/tool.md](references/tool.md) |
| **Content/Utility** | Generator, library, publisher, file processing | [references/tool.md](references/tool.md) |
| **Dashboard/Monitor** | Charts, real-time, alerts, metrics | [references/tool.md](references/tool.md) |
| **API Service** | REST endpoints, middleware, microservice | [references/tool.md](references/tool.md) |
| **SDK/Library** | Shared module, build step, multi-consumer | [references/sdk.md](references/sdk.md) |

## 📁 Typical Structure

```
project-name/
  server.js          ← Entry point, only mounts routes (<100 lines)
  config.json        ← All tunable values (admin dashboard can edit)
  db.js              ← Database init + helpers
  routes/
    auth.js          ← Authentication routes
    game.js          ← Core game/business routes
    admin.js         ← Admin API (config CRUD, stats)
  services/          ← Shared business logic
  public/
    index.html       ← Pure HTML skeleton (<200 lines)
    admin.html       ← Admin dashboard
    css/style.css
    js/              ← Split by responsibility, each <400 lines
    assets/
```

## 🚀 Installation

### For OpenClaw users

Copy the skill to your agents skill directory:

```bash
# Clone and copy
git clone https://github.com/abczsl520/nodejs-project-arch.git
cp -r nodejs-project-arch ~/.agents/skills/

# Or if you have clawhub:
# clawhub install nodejs-project-arch
```

The skill auto-activates when you ask your AI agent to:
- Create a new Node.js project
- Refactor a large single-file codebase
- Split oversized files

### For manual reference

Browse the reference docs directly:
- [Game Architecture](references/game.md) — H5 games with Canvas/Phaser
- [Tool Architecture](references/tool.md) — Data tools, dashboards, API services
- [SDK Architecture](references/sdk.md) — Shared libraries and SDKs

## 📖 Documentation

Full documentation is available on the [Wiki](https://github.com/abczsl520/nodejs-project-arch/wiki):

- [Home](https://github.com/abczsl520/nodejs-project-arch/wiki) — Overview
- [Installation](https://github.com/abczsl520/nodejs-project-arch/wiki/Installation) — Setup guide
- [Game Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/Game-Architecture) — H5 game standards
- [Tool Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/Tool-Architecture) — Non-game project standards
- [SDK Architecture](https://github.com/abczsl520/nodejs-project-arch/wiki/SDK-Architecture) — Library/SDK standards
- [Config Pattern](https://github.com/abczsl520/nodejs-project-arch/wiki/Config-Pattern) — Universal config.json pattern
- [Admin Dashboard](https://github.com/abczsl520/nodejs-project-arch/wiki/Admin-Dashboard) — Admin panel standards
- [Token Savings](https://github.com/abczsl520/nodejs-project-arch/wiki/Token-Savings) — Token savings analysis

## 🔧 Quick Example: config.json Pattern

```javascript
// server.js — load and serve config
const config = JSON.parse(fs.readFileSync('./config.json', 'utf8'));

app.get('/api/config', (req, res) => {
  const safe = { ...config };
  delete safe.admin;
  res.json(safe);
});

// Admin hot-reload
app.post('/admin/config', requireAdmin, (req, res) => {
  fs.writeFileSync('./config.json.bak', fs.readFileSync('./config.json'));
  fs.writeFileSync('./config.json', JSON.stringify(req.body, null, 2));
  Object.assign(config, req.body);
  res.json({ ok: true });
});
```

## 📊 Real-World Token Savings

| Scenario | Before | After | Savings |
|----------|--------|-------|---------|
| Read game feature code | ~40K tokens | ~2.7K tokens | **93%** |
| One dev round (read→edit→verify) | ~52K tokens | ~4K tokens | **92%** |
| 4-round SDK session | ~196K tokens | ~69K tokens | **65%** |
| Large data tool module | ~84K tokens | ~8K tokens | **90%** |

## License

MIT
