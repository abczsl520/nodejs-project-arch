# H5 Game Architecture

## Directory Structure

```
game-name/
  server.js              ← Entry, mount routes only (<100 lines)
  config.json            ← All tunable gameplay values
  routes/
    auth.js              ← Login/user
    game.js              ← Game data API
    social.js            ← Leaderboard/sharing
    admin.js             ← Admin API (read/write config.json)
  public/
    index.html           ← HTML skeleton only (<200 lines)
    admin.html           ← Admin dashboard
    css/style.css
    js/
      config.js          ← Fetch config from server at startup
      game.js            ← Core game logic
      renderer.js        ← Rendering/visuals
      ui.js              ← UI interactions
      audio.js           ← Sound effects
      auth.js            ← Login
      social.js          ← Leaderboard/sharing
    assets/              ← Images/audio
```

## config.json Structure

```json
{
  "game": { "name": "Game Name", "version": "1.0.0" },
  "gameplay": {
    "// game-specific tunable parameters"
  },
  "difficulty": {
    "// difficulty curves"
  },
  "rewards": {
    "// economy/reward system"
  },
  "social": {
    "enableRanking": true,
    "enableShare": true
  }
}
```

## Frontend Config Loading

```javascript
var GAME_CONFIG = null;
var DEFAULT_CONFIG = { /* fallback values */ };

function loadGameConfig() {
  return fetch('api/config')
    .then(function(r) { return r.json(); })
    .then(function(data) { GAME_CONFIG = data; })
    .catch(function() { GAME_CONFIG = DEFAULT_CONFIG; });
}
```

All hardcoded gameplay values read from `GAME_CONFIG.xxx`.

## Admin API (routes/admin.js)

```
GET  /admin/config          ← Read current config
POST /admin/config          ← Update config (with auth)
GET  /admin/config/history  ← View change history
POST /admin/config/restore  ← Restore from backup
GET  /admin/stats           ← Game statistics
```

Auth: `x-admin-password` header.

## server.js Entry Pattern

```javascript
var express = require('express');
var { DatabaseSync } = require('node:sqlite');
var app = express();
var db = new DatabaseSync('./game.db');

app.use(express.json());
app.use(express.static('public'));

// Mount routes
app.use('/api/auth', require('./routes/auth')(db));
app.use('/api/game', require('./routes/game')(db));
app.use('/api/social', require('./routes/social')(db));
app.use('/admin', require('./routes/admin')(db));

app.listen(3000);
```

## Splitting Rules

| Source | Split Into | Max Lines |
|--------|-----------|-----------|
| Monolithic `index.html` | `index.html` (skeleton) + `css/style.css` + `js/*.js` | 200 / 400 each |
| Monolithic `server.js` | `server.js` (entry) + `routes/*.js` | 100 / 400 each |
| Hardcoded values | `config.json` + `js/config.js` loader | - |
| No admin | Add `routes/admin.js` + `admin.html` | 400 / 200 |

## WeChat WebView Compatibility

- No `onclick` navigation, use `<a href="absolute-URL">`
- No ES6+ syntax (template literals, optional chaining, arrow functions)
- Use `var` and `function`, not `const`/`let`/`=>`
- No 302 redirects with params (use localStorage)
- Images: JPEG format (PNG with large blank areas fails in WeChat)
- SDK: protocol-adaptive `//domain/path` for HTTPS pages
- Sub-path APIs: use relative paths `api/xxx`, not absolute `/api/xxx`
