# Feature X - User Mention Alerts
## Summary
This note describes Feature X, which sends real-time alerts to users when they are mentioned in comments across the platform. It covers user stories, acceptance criteria, and edge cases such as spam prevention and notification throttling. It also documents UI behavior on web and mobile, including notification center updates. Known limitations and follow-up ideas are listed at the end.
*Tags:* #product #feature_spec #notifications #frontend #backend

---

## Notes

### User Stories

- As a user, I receive an alert when someone @mentions me in a comment.
- As a user, I can configure which notification channels I want to use.
- As a user, I do not receive duplicate notifications for the same mention event.

### Acceptance Criteria

- Mentions are processed within 10 seconds.
- Duplicate notifications for the same event are not sent.
- Users can opt into email, push, or in-app notifications independently.

### Edge Cases

- **Spam prevention:** Users who are mentioned more than 20 times in one minute by the same actor are throttled.
- **Notification throttling:** At most 5 notifications per minute per user are delivered to avoid overload.
- **Deleted comments:** If a comment is deleted before delivery, the notification is suppressed.

### UI Behavior

- Web: notification bell icon increments; notification center shows mention preview.
- Mobile: push notification with sender name and truncated comment text.

### Known Limitations

- Group mentions (e.g., @team) are not supported in v1.
- Notification delivery SLA is best-effort; no guaranteed delivery retry beyond 3 attempts.

### Follow-up Ideas

- Add weekly digest for lower-priority mentions.
- Support @team and @role group mentions.

---

## Links

- [[Feature Specs Index]]
- [[Product Index]]
