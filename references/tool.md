# Non-Game Project Architecture

## Project Subtypes

### Data Tool (Crawler/Scraper/Analytics)

```
project-name/
  server.js              ← Entry (<100 lines)
  config.json            ← API keys, schedule, thresholds
  db.js                  ← DB init + utility functions
  routes/
    api.js               ← Query/export/stats API
    task.js              ← Task management (CRUD/schedule)
    data.js              ← Data ops (import/export/sync)
    admin.js             ← Admin API
  services/
    crawler.js           ← Crawl/scrape logic
    parser.js            ← Data parsing/cleaning
    scheduler.js         ← Cron/interval scheduling
    sync.js              ← Third-party sync (Feishu/Excel)
  public/
    index.html           ← HTML skeleton (<200 lines)
    admin.html           ← Admin dashboard
    css/style.css
    js/
      app.js             ← Main logic + routing
      table.js           ← Table render/pagination/filter
      chart.js           ← Charts (if needed)
      api.js             ← Fetch wrapper
```

### Content/Utility (Generator/Library/Publisher)

```
project-name/
  server.js              ← Entry (<100 lines)
  config.json
  db.js
  routes/
    api.js               ← Core business API
    content.js           ← Content CRUD
    upload.js            ← File upload/processing
    admin.js
  services/
    generator.js         ← Content generation
    storage.js           ← File storage
  public/
    index.html, css/, js/
```

### Dashboard/Monitor

```
project-name/
  server.js              ← Entry (<100 lines)
  config.json            ← Targets/thresholds/alert rules
  db.js
  routes/
    api.js               ← Data query API
    monitor.js           ← Monitor/collect API
    alert.js             ← Alert management
    admin.js
  services/
    collector.js         ← Data collection
    analyzer.js          ← Analysis/aggregation
    notifier.js          ← Alert notifications
  public/
    index.html, css/
    js/
      dashboard.js       ← Dashboard rendering
      chart.js           ← Charts
      realtime.js        ← WebSocket updates (if needed)
      api.js
```

### API Service (Pure Backend)

```
project-name/
  server.js              ← Entry (<100 lines)
  config.json
  routes/
    v1/
      resource-a.js
      resource-b.js
    admin.js
  services/
    core.js              ← Core business logic
    queue.js             ← Task queue (if needed)
  middleware/
    auth.js              ← Auth middleware
    rateLimit.js         ← Rate limiting
```

## Backend Splitting Rules

1. **Entry file = mounting only**: express init + middleware + route mounting + listen
2. **Routes by domain**: not by HTTP method, by business domain (user/data/task/admin)
3. **Shared logic → services/**: crawlers, parsers, third-party API calls
4. **DB ops centralized**: `db.js` handles table creation + common queries
5. **Scheduled tasks isolated**: all cron/interval logic in `services/scheduler.js`

## Frontend Splitting Rules

1. **HTML = skeleton only**: structure tags + container divs + script/link refs
2. **CSS in separate file**: `css/style.css`, split by page for large projects
3. **JS by function**:
   - `app.js` — init + page routing/tab switching
   - `api.js` — all fetch requests
   - `table.js` — table rendering (required for data tools)
   - `chart.js` — chart rendering (if using ECharts/Chart.js)
   - Business modules in separate files
4. **No JS template literals** (backticks): use string concatenation

## config.json Structure

```json
{
  "app": { "name": "Project Name", "port": 3456 },
  "schedule": {
    "enabled": true,
    "interval": "*/30 * * * *",
    "retryCount": 3
  },
  "api": { "rateLimit": 100, "timeout": 30000 },
  "thirdParty": {
    "feishu": { "appId": "", "appSecret": "" }
  },
  "admin": { "password": "xxx" }
}
```

Rules:
- All tunable params in config.json
- Server loads at startup, serves `/api/config` (strip secrets)
- Admin dashboard hot-reloads config (no restart needed)
- Secrets never exposed to frontend

## Admin Dashboard Standard

Every `admin.html` includes:
1. Password login (`x-admin-password` header auth)
2. Runtime status: uptime, memory, request stats
3. Config editor: visual edit + save = hot-reload
4. Data overview: key metrics (users/data/tasks)
5. Operation log: recent admin actions
