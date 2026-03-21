# Lesson: Always test auth as the resource creator, not just admin

**Date**: 2026-03-22
**Context**: DELETE /api/thread/:id auth check was using wrong field (existing.author vs existing.created_by)

## What Happened

Karo shipped DELETE endpoints with auth checks. Bertus verified the security model. But when I tested DELETE /api/thread/:id as `snap` (the thread creator), it rejected me with "Only the thread creator or Gorn can delete." Deleting as `gorn` worked fine.

The bug: the auth check compared `?as=snap` against `existing.author` (which doesn't exist on threads) instead of `existing.created_by`. The admin path (gorn) bypassed the comparison entirely, so it always worked.

## Lesson

Auth bugs hide in the non-admin path. Security reviews that only test admin access miss creator-permission bugs. Always test:
1. As the resource creator (the normal permission path)
2. As a non-creator non-admin (should be rejected)
3. As admin (should work but isn't the interesting test)

The creator path is where field name mismatches, null comparisons, and silent failures live.

## Applied To

- DELETE /api/thread/:id — fixed in f83aa88
- Pattern applies to all future endpoint QA
