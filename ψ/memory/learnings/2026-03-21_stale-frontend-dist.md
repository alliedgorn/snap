# Lesson: Stale Frontend Dist Causes Silent Failures

**Date**: 2026-03-21
**Context**: Thread #141 — remote panel broke after notification queue removal (commit 13f00bf)
**Source**: Live bug investigation

## The Pattern
When backend/API changes are committed alongside frontend code changes, the frontend dist must be rebuilt. If it isn't, the browser serves old JavaScript that references deleted endpoints.

## Why It's Dangerous
- `Promise.all()` rejects entirely if any single fetch fails (e.g., 404 on deleted endpoint)
- Silent `catch {}` blocks swallow the error — no console warning, no visible error
- The UI just shows nothing — empty state that looks like a data loading issue
- Build timestamps and commit timestamps diverge silently

## Detection
1. Compare `stat` on `dist/index.html` against `git log` timestamp of latest commit
2. Check asset hash in served HTML vs files in dist/assets/
3. `grep` the built JS bundle for references to deleted endpoints

## Prevention
- Always rebuild frontend after any commit that touches frontend code
- QA should verify build timestamp as part of deploy verification
- Consider adding a build hash/timestamp to the UI footer for quick visual checks
