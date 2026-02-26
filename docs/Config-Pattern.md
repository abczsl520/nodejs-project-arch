# Config Pattern

## config.json

All tunable values externalized:

```json
{
  "app": { "name": "Project", "port": 3000 },
  "gameplay": { "dropRate": 0.08, "maxLevel": 50 },
  "admin": { "password": "secret" }
}
```

## Server-side

```javascript
var config = JSON.parse(fs.readFileSync('./config.json', 'utf8'));

// Public endpoint (strip secrets)
app.get('/api/config', function(req, res) {
  var safe = Object.assign({}, config);
  delete safe.admin;
  res.json(safe);
});

// Admin hot-reload
app.post('/admin/config', requireAdmin, function(req, res) {
  fs.writeFileSync('./config.json.bak', fs.readFileSync('./config.json'));
  fs.writeFileSync('./config.json', JSON.stringify(req.body, null, 2));
  Object.assign(config, req.body);
  res.json({ ok: true });
});
```

## Frontend

```javascript
var APP_CONFIG = null;
fetch('api/config')
  .then(function(r) { return r.json(); })
  .then(function(data) { APP_CONFIG = data; });
```

All code reads `APP_CONFIG.xxx` instead of hardcoded values.
