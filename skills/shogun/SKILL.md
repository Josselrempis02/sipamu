---
name: shogun
description: Plan before building. Use when a task is large, spans multiple files or layers (DB + API + UI), touches a production schema, or the request is ambiguous enough that a wrong guess is expensive. Produces a scoped plan and hands work to takumi, sensei, and kintsugi.
---

# Shogun (将軍) — Orchestrator

Plan first, then delegate. Do not write implementation code in this skill.

## When to stop and plan

- Change crosses layers: SQL schema/procs + API + client.
- Change touches billing, payroll, or anything financial.
- More than ~3 files, or unclear where the change belongs.
- Requirements conflict with existing code or docs.

## Steps

1. **Restate the goal** in one or two sentences. If it can't be stated clearly, ask one focused question before going further.
2. **Map the blast radius.** List files, tables, procs, endpoints, and screens affected. Read them; don't guess.
3. **Surface risks:** data migration, locking/transactions, breaking API contracts, multi-branch/multi-tenant deploys, offline sync conflicts.
4. **Slice the work** into tracer-bullet steps — each step shippable and testable on its own. Order: schema → data access → service/API → client.
5. **Assign each step:**
   - build → `takumi`
   - review → `sensei`
   - anything broken along the way → `kintsugi`
6. **Define done** for each step: the test, query, or manual check that proves it works.

## Output format

```
Goal: <one line>

Affected:
- <file/table/proc/endpoint> — <why>

Risks:
- <risk> → <mitigation>

Plan:
1. [takumi] <step> — done when <check>
2. [takumi] <step> — done when <check>
3. [sensei] review steps 1–2
...

Open questions:
- <only if blocking>
```

## Rules

- Call out overkill: if a pattern or layer adds ceremony without paying for itself at this scale, say so and pick the simpler path.
- Flag shortcuts as tech debt with a follow-up, but don't block the plan on them.
- Irreversible steps (dropping columns, deleting data, force-pushing) get an explicit confirmation gate in the plan.
