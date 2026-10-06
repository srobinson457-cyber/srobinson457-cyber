# Reviewing my code

If you have an hour to judge my work, start here. The three repos on my profile are public. ParentPulse is private because it holds families' data, so the ParentPulse items below need read access, which I grant on request.

## A 30-minute reading path

1. **ParentPulse [`ARCHITECTURE.md`](https://github.com/srobinson457-cyber/ParentPulse/blob/main/ARCHITECTURE.md).** Twenty design decisions, most linked to the pull request that made them, and the debt I know about and have not fixed yet.
2. **PR [#690](https://github.com/srobinson457-cyber/ParentPulse/pull/690) and [`supabase/tests/92_kiosk_exit_requires_pin.sql`](https://github.com/srobinson457-cyber/ParentPulse/blob/main/supabase/tests/92_kiosk_exit_requires_pin.sql).** Leaving kiosk mode needs the admin PIN, checked on the server. A wrong PIN returns false instead of raising an error, so the failed attempt is recorded and the attempt limit actually counts.
3. **PR [#715](https://github.com/srobinson457-cyber/ParentPulse/pull/715).** A 2,420-line settings screen split into a coordinator and five sections, copied verbatim by a script and shown to render identically in 135 scenarios before it merged.
4. **PRs [#675](https://github.com/srobinson457-cyber/ParentPulse/pull/675) and [#676](https://github.com/srobinson457-cyber/ParentPulse/pull/676).** A mistake and how it was caught (below).
5. **[`supabase/tests/54_get_family_id_keystone_battery.sql`](https://github.com/srobinson457-cyber/ParentPulse/blob/main/supabase/tests/54_get_family_id_keystone_battery.sql).** Tests for the one function nearly every family-data access rule depends on. Each check is paired with a deliberate break that must make it fail, because a security test that cannot fail proves nothing.
6. **[`scripts/check-migration-safety.mjs`](https://github.com/srobinson457-cyber/ParentPulse/blob/main/scripts/check-migration-safety.mjs).** The CI gate that refuses destructive database migrations, and tests itself before it runs. A public version is [pg-migration-safety](https://github.com/srobinson457-cyber/pg-migration-safety).

## How this code is built

I don't write the code by hand. AI agents (Claude Code) write it. My part is deciding what gets built, which trade-offs are acceptable, and what counts as proof that a change works.

No person reads every line, and no person approves each merge. Instead, every change has to pass checks that run on their own: type checking, linting, unit tests and a production build; a database security suite that rebuilds the database from all of its migrations, starting empty, and tests who can read and change what; the migration-safety gate; and a local merge gate that refuses any merge whose required checks are not green. A new test only counts once it has been seen failing against the broken code.

Here is one that slipped. In #675 a security migration recreated five database access rules and dropped their restriction to signed-in users. It was not exploitable, but the schema comparison that runs after every deploy flagged the difference, and #676 restored the restriction fifteen minutes later. The slip was caught by a check, not by luck, and that is the design.

The weak spots are written down too: `ARCHITECTURE.md` lists the known debt, and the open ParentPulse issues labelled `qa:ai-code-audit` are the current clean-up plan.
