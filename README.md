# Samuel Robinson

Patrol sergeant in a Georgia sheriff's office. Self-taught developer.

Nine years sworn. I am the supervisor my shift brings records-validation failures to, and the
final approval on every report before it becomes a permanent record. So most of what I build
ends up being about the same thing from a different angle: making it hard for bad data to get
written, and making systems recoverable by someone who cannot physically reach them.

Everything here is my own work, built and shipped independently. None of it was paid.

---

### [android-device-owner-kiosk](https://github.com/srobinson457-cyber/android-device-owner-kiosk)

A working Android Device Owner implementation: full device lockdown, lock task, an HOTP-style
challenge/response admin gate, and an HMAC-signed remote policy channel. Extracted from a build
running unattended on real hardware at a site I cannot drive to.

Java, no dependencies. The README is nine field notes on where the documented Android behaviour
is not the actual behaviour, including the one that cost the most: an escape hatch whose only
setting is "destroy everything" is a fuse, not an escape hatch.

### [pg-migration-safety](https://github.com/srobinson457-cyber/pg-migration-safety)

A CI gate for destructive SQL migrations. Migration tests replay against an empty database, so
a `DROP`, `TRUNCATE` or unqualified `DELETE` that destroys real production rows passes CI green.
The suite cannot catch it, because there is no data in the database it tests against.

One file, zero dependencies, 24 self-tests. The repo runs the gate against its own deliberately
destructive example and fails if that stops being flagged.

### [node-mail-no-deps](https://github.com/srobinson457-cyber/node-mail-no-deps)

SMTP and IMAP written directly against `node:tls`, so no package in the dependency tree ever
holds the password to a real mailbox.

The more useful half is the bounded retrying connect, which documents measured Node behaviour
rather than documentation summaries. The headline: `AbortSignal.timeout()` does not bound a
connect. The signal stays attached for the socket's whole life and destroys the live session
later. I shipped that, and it killed an IMAP session at exactly ten seconds while typical runs
took nine.

---

Elsewhere: a family organisation PWA live on Google Play (React, TypeScript, Supabase,
PostgreSQL) behind 221 migrations, 87 automated database tests and row-level security, and an
Android parental-control app. Those are private, because they hold other people's data.
