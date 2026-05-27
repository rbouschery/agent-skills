---
name: bug-investigate-fix
description: >-
  Systematically reproduces, hypothesizes, tests, plans, fixes, and verifies bugs
  from a user bug description. Use when the user reports a bug, regression,
  unexpected behavior, crash, or asks to debug, reproduce, root-cause, or fix an
  issue. Prefer local dev server and browser verification over code-only guesses.
disable-model-invocation: true
---

# Bug Investigate and Fix

End-to-end workflow from bug description to verified fix. Do not skip reproduction or hypothesis testing to jump straight to a patch.

## Inputs

- **Required**: Bug description (symptoms, expected vs actual, where it appears, when it started if known).
- **Infer from repo**: How to run the app (Agents.md, `package.json`, README, existing dev scripts).

If reproduction is blocked (missing env, credentials, steps), state blockers and ask only what unblocks repro — do not fabricate a fix plan.

## Workflow

Copy this checklist and update as you go:

```
- [ ] 1. Reproduce
- [ ] 2. Hypotheses (1–4)
- [ ] 3. Test hypotheses
- [ ] 4. Fix plan (thermo-nuclear-plan, short)
- [ ] 5. Implement fix
- [ ] 6. Verify repro is gone
```

---

### 1. Reproduce

**Goal**: Observe the bug in a running environment, not only by reading code.

1. Start or use the **local dev server** (or the project’s documented local run command).
2. Reproduce using the **same surface** as the report when possible:
   - Web/UI → browser (navigation, clicks, network tab, console)
   - API → `curl`/HTTP client against local base URL
   - CLI → run the command with the reported flags/env
   - Background job → trigger locally or via documented test hook
3. Record **minimal repro steps**: prerequisites, exact actions, inputs, and observable failure (message, status code, screenshot-level description).
4. If you cannot reproduce, document what you tried and what differs from the report before continuing.

**Do not** treat static code reading alone as reproduction.

---

### 2. Form hypotheses

Produce **1–4 ranked hypotheses** (most likely first). Each hypothesis must be:

- **Specific** — names module, function, route, state, or data path
- **Falsifiable** — states what evidence would confirm or reject it
- **Tied to symptoms** — links expected observation if true

Use a short table:

| # | Hypothesis | If true, we'd see… | Quick test |
|---|------------|-------------------|------------|
| 1 | … | … | … |
| 2 | … | … | … |

Avoid vague hypotheses ("something wrong with auth"). Prefer one concrete mechanism per row.

---

### 3. Test hypotheses

Test **in order of likelihood** until one is confirmed or the set is exhausted.

- Prefer **fast, discriminating checks**: logs, breakpoints, targeted `grep`, one-off script, unit/integration test, network payload inspection.
- For each test, record: **ran → result → confirms / rejects / inconclusive**.
- Stop when a hypothesis is **confirmed** with evidence from repro environment or a direct code path trace.
- If all are rejected, add 1–2 new hypotheses from new evidence and repeat (one small loop only — do not spiral).

**Do not** implement a fix before a hypothesis is confirmed or strongly supported.

---

### 4. Fix plan (thermo-nuclear-plan, short)

Read and apply the **thermo-nuclear-plan** skill (`/thermo-nuclear-plan`), but **compress output for a single bug fix**.

**If thermo-nuclear-plan is not available** (not installed or not attached), plan from this summary instead: strict pre-coding planning that favors **code judo** — the simplest fix that still meets the goal, with fewer files, phases, and special cases rather than scheduling around incidental complexity. Prefer concrete outcomes per step, canonical modules over new wrappers, and explicit out-of-scope. Then use the compressed format below (do not produce a full mega-plan).

1. **Goal** — one sentence: bug gone, behavior restored, no regressions named by user.
2. **Simplest viable approach** — the code-judo fix (fewest files, no new abstractions unless necessary).
3. **Plan** — at most **3 phases** or **one PR slice**, each with **≤5 concrete steps** (outcome per step, no "refactor as needed").
4. **Out of scope** — explicit nice-to-haves to skip.
5. **Verification** — how you'll prove repro steps fail after the fix.

If root cause is still unclear, output **open questions** and a **labeled provisional plan** — do not implement yet.

---

### 5. Implement fix

- Implement only what the short plan requires; minimize diff scope.
- Match existing project conventions (types, error handling, tests only if the repo expects them for this area).
- Do not expand scope (drive-by refactors, unrelated cleanup).

---

### 6. Verify repro is gone

1. Re-run the **same minimal repro steps** from step 1 on the local dev server / browser / CLI.
2. Confirm **expected behavior** and absence of the original failure.
3. If the bug was environmental, note what changed (env var, seed data, cache).
4. Report: repro steps before → after, hypothesis confirmed, files touched, residual risk.

If verification fails, return to step 2 with new evidence — do not claim the bug is fixed.

---

## Output shape (for the user)

Keep the final summary compact:

1. **Repro** — steps + what you observed
2. **Root cause** — confirmed hypothesis in plain language
3. **Fix** — what changed and why
4. **Verification** — repro re-run result

