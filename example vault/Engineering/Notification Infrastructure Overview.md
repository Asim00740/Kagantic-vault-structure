# Notification Infrastructure Overview
## Summary
This file documents the backend pipeline responsible for delivering user notifications across email, push, and in-app channels. It describes the queue-based architecture, retry logic, deduplication strategy, and third-party integrations used. This is the primary reference for engineers working on Feature X or any notification-adjacent system. It also documents known failure modes and on-call escalation paths.
*Tags:* #architecture #notifications #backend #engineers #internal

---

## Pipeline

1. **Event published** - Core Service emits a `mention.created` event to the `notifications` queue.
2. **Worker picks up event** - Notification Worker dequeues and validates the event payload.
3. **Deduplication check** - Redis cache is checked for a duplicate event key (TTL: 60s).
4. **Channel routing** - User preferences determine which channels receive the notification.
5. **Delivery** - Each channel adapter sends the notification (in-app, push via FCM/APNs, email via SendGrid).
6. **Retry on failure** - Up to 3 retries with exponential backoff; dead-letter queue after final failure.

---

## Third-Party Integrations

| Service   | Purpose                  |
|-----------|--------------------------|
| SendGrid  | Transactional email      |
| FCM       | Android push             |
| APNs      | iOS push                 |

---

## Known Failure Modes

- **Queue backlog:** High comment volume can cause delivery delays; monitor queue depth.
- **APNs token expiry:** Stale device tokens cause silent failures; purge on 410 response.
- **SendGrid rate limits:** Burst sends may be throttled; implement send-time smoothing for digests.

---

## Links

- [[Engineering Index]]
- [[Architecture Overview]]
- [[Feature X - User Mention Alerts]]
