# Tool Architecture (Non-Game Projects)

## Subtypes

### Data Tool (Crawler/Scraper)
```
project/
  server.js, config.json, db.js
  routes/    ← api.js, task.js, data.js, admin.js
  services/  ← crawler.js, parser.js, scheduler.js, sync.js
  public/    ← index.html, admin.html, css/, js/
```

### Content/Utility
```
project/
  server.js, config.json, db.js
  routes/    ← api.js, content.js, upload.js, admin.js
  services/  ← generator.js, storage.js
  public/
```

### Dashboard/Monitor
```
project/
  server.js, config.json, db.js
  routes/    ← api.js, monitor.js, alert.js, admin.js
  services/  ← collector.js, analyzer.js, notifier.js
  public/    ← js/dashboard.js, chart.js, realtime.js
```

### API Service
```
project/
  server.js, config.json
  routes/v1/ ← resource-a.js, resource-b.js
  services/  ← core.js, queue.js
  middleware/← auth.js, rateLimit.js
```

## Key Difference from Games

Non-game projects have a `services/` layer for business logic shared across routes, and `db.js` for centralized database operations.
