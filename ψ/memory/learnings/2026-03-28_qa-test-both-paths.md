# QA Lesson: Always Test Both Create and Edit Paths

**Date**: 2026-03-28
**Source**: T#430 meal item logging — PATCH auto-sum bug

## What Happened
I QA'd the POST endpoint for meal items and confirmed auto-sum works. But I didn't test PATCH (edit). Pip found that editing a meal with items didn't recompute the top-level macros — only POST did.

## Lesson
When testing CRUD features, always test both create AND edit paths. They often share validation logic but not always — different code paths can have different bugs.

## Checklist for Future QA
1. POST (create) — does it validate and compute correctly?
2. PATCH (edit) — does it re-validate and recompute?
3. DELETE — does it clean up properly?
4. GET — does it return the right format for both old and new data?
