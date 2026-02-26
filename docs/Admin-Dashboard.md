# Admin Dashboard

Every project includes `admin.html` with:

1. **Password login** — `x-admin-password` header auth
2. **Config editor** — Visual form auto-generated from config.json structure
3. **Hot-reload** — Save = immediate effect, no restart
4. **Stats overview** — Users, data volume, uptime
5. **Backup history** — Auto-backup before each save, one-click restore

## API Endpoints

```
GET  /admin/config          ← Read current config
POST /admin/config          ← Update (auto-backup old)
GET  /admin/config/history  ← List backups
POST /admin/config/restore  ← Restore from backup
GET  /admin/stats           ← Project statistics
```
