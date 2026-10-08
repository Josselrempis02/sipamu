---
name: kintsugi
description: Debug systematically. Use when something is broken, throwing, slow, or returning wrong results — stack traces, SQL Server error messages (Msg 8152, conversion failures, deadlocks), failing tests, "works on staging but not prod", or performance regressions.
---

# Kintsugi (金継ぎ) — Debugger

Repair with gold: find the real cause, fix it, and leave the code stronger than before.

## Loop

1. **Reproduce.** Get the exact error text, input, and environment. If it can't be reproduced, gather logs/data before touching code.
2. **Isolate.** Narrow to the smallest failing unit — one query, one request, one component. Bisect recent changes if it's a regression.
3. **Hypothesize.** State one cause and the evidence that would confirm or kill it. Test it. Don't stack multiple guesses.
4. **Fix the root cause**, not the symptom. A `TRY_CAST` or a null-check that hides bad data is a symptom fix — say so if it's the pragmatic choice now.
5. **Verify.** Re-run the reproduction. Add a test or guard that would have caught it.
6. **Report.** Cause → fix → how verified → follow-up.

## Common causes by stack

**SQL Server**
- `Msg 8152` / `2628` truncation → column width vs incoming data; check the specific column via the error text (2019+ names it).
- Conversion failed → implicit conversion in join/filter, bad data in a varchar column, date format/`DATEFORMAT` differences across servers.
- Deadlocks → inconsistent lock order across procs, missing indexes causing scans, long transactions. Pull the deadlock graph from `system_health`.
- Slow after deploy → parameter sniffing, stale statistics, plan regression. Compare actual execution plans.
- Linked-server issues → collation mismatch, MSDTC, remote query not pushing filters down.

**C#/.NET**
- Hangs → sync-over-async deadlock. Null refs after EF query → missing `Include`. Works locally, fails deployed → config/connection string/environment differences.

**React Native / Expo**
- Works on iOS, fails on Android (or reverse) → platform API differences, permissions, keyboard/layout behaviour.
- Infinite re-render → unstable effect deps. Stale data → closure capturing old state.

**Node**
- Silent failures → unhandled rejection. Intermittent → race condition or shared mutable state.

## Output format

```
Cause: <root cause, one line>
Evidence: <what confirmed it>
Fix: <code/SQL>
Verified by: <repro re-run / test>
Follow-up: <hardening or debt, if any>
```

## Rules

- Quote error messages exactly.
- Never claim fixed without re-running the reproduction.
