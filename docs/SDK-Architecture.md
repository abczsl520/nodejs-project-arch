# SDK Architecture

## Directory Structure

```
sdk-name/
  src/                    ← Source modules (each <200 lines)
    core.js, auth.js, share.js, api.js, ui.js
  sdk-name.js             ← Build output (merged single file)
  server-sdk.js           ← Backend SDK (single file, easy to copy)
  build.js                ← Merge script
  CHANGELOG.md
  test/test.html
```

## Workflow

1. **Develop** in `src/` small modules
2. **Build** with `node build.js` → merges into single file
3. **Consume**: `<script src="sdk-name.js">` (unchanged)
4. **Backend SDK**: stays as single file for easy `require()`

## When to Use

- Multiple projects consume the same module
- Need single `<script>` or `require()` import
- Need version tracking across consumers
