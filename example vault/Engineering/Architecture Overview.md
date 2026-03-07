# Architecture Overview
## Summary
This file describes the high-level architecture of the platform, covering the major services, their responsibilities, and how they communicate. It documents the primary data flows between the frontend, API layer, background workers, and storage systems. This is the recommended starting point for any engineering question about system structure or inter-service dependencies. It is kept at a high level; deeper detail lives in service-specific notes linked below.
*Tags:* #architecture #engineering #backend #internal #v1

---

## System Components

| Component         | Role                                              |
|-------------------|---------------------------------------------------|
| Web Frontend      | React SPA; communicates with API via REST/WS      |
| Mobile App        | React Native; shares API layer with web           |
| API Gateway       | Routes requests; handles auth and rate limiting   |
| Core Service      | Business logic; orchestrates background jobs      |
| Notification Worker | Async job processor for alerts and emails       |
| Database          | PostgreSQL; primary store for all user data       |
| Cache             | Redis; session state, rate limit counters         |
| Message Queue     | RabbitMQ; decouples Core Service from workers     |

---

## Data Flow (Mention Alert Example)

1. User posts a comment via Web Frontend.
2. API Gateway authenticates and forwards to Core Service.
3. Core Service detects @mentions and publishes an event to Message Queue.
4. Notification Worker consumes the event and delivers push/email/in-app alerts.

---

## Links

- [[Engineering Index]]
- [[Notification Infrastructure Overview]]
