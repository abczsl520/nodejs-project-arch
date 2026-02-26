# Game Architecture

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

## Rules

- `server.js` < 100 lines (entry + route mounting only)
- Each `routes/*.js` < 400 lines
- `index.html` < 200 lines (HTML skeleton, no inline JS/CSS)
- Each `js/*.js` < 400 lines
- All gameplay values in `config.json`
- Frontend loads config via `fetch('api/config')` at startup

## WeChat WebView Compatibility

- No ES6+ syntax (use `var`, `function`, no arrow functions)
- No `onclick` navigation (use `<a href>`)
- JPEG format for images (PNG with blank areas fails)
- Relative API paths under sub-paths (`api/xxx` not `/api/xxx`)
