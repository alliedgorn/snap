# Lesson: "Active" ≠ "Delivered"

**Date**: 2026-03-20
**Source**: Thread #107 — @real-broker team mention verification
**Context**: Pip verified team mentions by checking tmux sessions were active. Gorn pushed back — active sessions don't prove notifications arrived.

## The Lesson

Checking that infrastructure is running is not the same as verifying message delivery. A tmux session being active proves the session exists, not that a notification was received and rendered in it.

**Proper verification layers:**
1. **API layer** — server returns correct notified list
2. **Delivery layer** — notification queue has entries for each recipient
3. **Receipt layer** — each recipient confirms they actually received it

Gorn's instinct: always verify end-to-end. Don't stop at "the system is up."

## Application

When QA-ing any notification, webhook, or message delivery system:
- Don't just check the sender confirms it sent
- Don't just check the infrastructure is running
- Verify the recipient actually received the payload
- Get confirmation from the destination, not just the source
