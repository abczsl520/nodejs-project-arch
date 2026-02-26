# SDK/Library Architecture

## Directory Structure

```
sdk-name/
  src/                    ← Source modules (each <200 lines)
    core.js               ← Event bus, utilities
    auth.js               ← Auth module
    share.js              ← Share module
    api.js                ← Fetch wrapper
    ui.js                 ← UI components (buttons, etc.)
  sdk-name.js             ← Build output (merged single file for consumers)
  server-sdk.js           ← Backend SDK (keep as single file for easy copy)
  build.js                ← Simple merge script (src/ → sdk-name.js)
  docs.html               ← API documentation page
  test/
    test.html             ← Local test page
  CHANGELOG.md            ← Version change log
```

## Key Principles

- **Dev time**: work in `src/` small modules, AI reads only what it needs
- **Publish time**: `build.js` merges `src/` into single file, consumer import unchanged
- **Backend SDK**: keep as single file (easy to copy into each project)
- **CHANGELOG.md**: track every change, consumers know what to upgrade

## build.js Pattern

```javascript
var fs = require('fs');
var path = require('path');

var files = ['core.js', 'auth.js', 'share.js', 'api.js', 'ui.js'];
var header = '/**\n * SDK Name v' + version + '\n * Built: ' + new Date().toISOString() + '\n */\n';
var output = header + '(function() {\n';

files.forEach(function(f) {
  output += '\n// === ' + f + ' ===\n';
  output += fs.readFileSync(path.join(__dirname, 'src', f), 'utf8');
});

output += '\n})();\n';
fs.writeFileSync(path.join(__dirname, 'sdk-name.js'), output);
console.log('Built sdk-name.js');
```

## When to Use SDK Architecture

- Multiple projects consume the same module
- Frontend: single `<script>` tag import required
- Backend: single `require()` import required
- Need version tracking across consumers
