---
name: thermo-nuclear-plan
description: >-
  Produces an unusually strict implementation plan focused on structural
  simplicity, minimal moving parts, and maintainable execution. Use for a
  thermo-nuclear plan, plan quality audit, strict planning review, deep
  implementation plan, or when the user wants an ambitious plan that avoids
  complexity.
disable-model-invocation: true
---

# Thermo-Nuclear Plan

Use this skill when writing or revising an implementation plan — before coding. Push for **code judo**: reframings that achieve the same goal with fewer phases, workstreams, concepts, and special-case branches. Do not stop at making each step concise. Ask whether whole categories of work can disappear because the approach can be simplified.

## Core Prompt

> Produce a rigorous implementation plan for the requested idea or change. Rethink how to structure the work so the implementation stays simple, modular, and maintainable without changing the goal. Prefer fewer phases, clearer ownership boundaries, and plans that avoid incidental complexity instead of scheduling around it. Be ambitious: if a clearer path involves reframing scope, sequencing, or decomposition, propose it. Be thorough. Measure twice, cut once.

## Non-Negotiable Additional Standards

Apply the Core Prompt above, plus these explicit planning rules:

0. **Be ambitious about structural simplification.**
   - Do not stop at "we could combine these steps."
   - Look for reframings where whole phases, workstreams, migration paths, or feature flags disappear.
   - Prefer the plan that will feel inevitable in hindsight once someone implements it.
   - Assume there is often a **code judo** move: a sequencing or decomposition choice that uses the existing architecture and makes the work dramatically smaller and clearer.
   - If you see a path to **avoid** complexity rather than document and schedule around it, push hard for that path.

1. **Do not let a single phase grow past a healthy size without strong reason.**
   - Treat any phase with more than ~10 concrete steps as a planning smell by default.
   - Prefer splitting by ownership boundary, vertical slice, or mergeable PR sequence instead of one mega-phase.
   - If the plan necessarily needs a large phase, explicitly justify decomposition *before* the detailed steps.

2. **Do not allow random contingency growth in the plan.**
   - Be highly suspicious of scattered "unless", "if X then also Y", or one-off branches in unrelated steps.
   - If the plan needs many special cases, treat that as a **scope or model** problem, not something to paper over with extra steps.
   - Prefer a dedicated phase, spike, or decision point over sprinkling exceptions through the happy path.

3. **Bias toward a clean implementation shape, not just a complete task list.**
   - If the goal can be met with a structurally cleaner approach, plan for that version.
   - Do not rubber-stamp a plan that schedules messier code because it is easier to describe.
   - Strongly prefer plans that **remove** moving pieces over plans that merely spread the same complexity across more PRs.

4. **Prefer direct, boring, maintainable work over vague or magical steps.**
   - Treat steps like "refactor as needed", "handle edge cases", or "clean up types" as planning defects — replace with concrete outcomes.
   - Be skeptical of generic frameworks or new abstractions proposed before the simple path is written down.
   - Flag plan items that add indirection (new service layer, wrapper module, config-driven behavior) without a clear payoff.

5. **Push hard on boundaries and contracts in the plan.**
   - Name what each phase owns: APIs, tables, modules, user-visible behavior.
   - Prefer explicit interfaces, migration invariants, and rollback assumptions over hand-wavy integration steps.
   - If the plan relies on "we'll figure out the shape during implementation", add a spike or decision step first.

6. **Keep work in the canonical layer and reuse what exists.**
   - Call out plan steps that duplicate existing helpers, patterns, or platform capabilities.
   - Prefer extending the module that already owns the concept over introducing a parallel path "for this feature only."
   - Push cross-cutting concerns to the layer that already handles them (auth, caching, errors, etc.).

7. **Treat unnecessary serial orchestration and non-atomic rollout as planning smells when a simpler structure is obvious.**
   - If independent work is strictly sequenced with no dependency, ask whether it can run in parallel or in separate mergeable slices.
   - If the plan can leave production in a half-migrated state, spell out the atomic cutover (or feature-flag boundary) explicitly.
   - Do not over-index on calendar optimization, but do flag sequencing that makes rollback or reasoning harder than necessary.

## Primary Planning Questions

For every meaningful part of the plan, ask:

- Is there a **code judo** move that makes this dramatically smaller?
- Can this be reframed so fewer phases, workstreams, or concepts are needed?
- Does this plan improve or worsen the local architecture?
- Did the plan add branching complexity where a better abstraction or decision should come first?
- Will a previously cohesive module become more coupled, more stateful, or harder to scan?
- Is each chunk of work living in the right layer and owner?
- Does any phase exceed ~10 concrete steps without justification?
- Do repeated "if / unless / also" patterns signal a missing model or a missing spike?
- Are steps direct and legible, or do they rely on special cases and incidental sequencing?
- Does every proposed abstraction earn its keep, or is it a wrapper in the schedule?
- Did the plan leave interfaces, migrations, or success criteria fuzzy?
- Is work assigned to the canonical layer, or leaking across boundaries?
- Is sequencing more serial or less atomic than it needs to be?

## Clarify before you plan

Before producing steps, resolve ambiguity in the request. Do not paper over gaps with assumptions buried in the plan.

**Push back when input is unclear**

- Scope is fuzzy ("improve", "clean up", "make it better") without a measurable outcome
- Success criteria are missing ("done" is undefined)
- Constraints conflict (timeline vs full rewrite, zero downtime vs big-bang migration)
- Ownership is unstated (which module, team, or surface owns the change)
- The request implies a solution before the problem is stated
- Load-bearing decisions are deferred ("we'll figure out schema/API/flag strategy during implementation")
- Terms of art or acronyms are used without a stable meaning in this codebase
- Multiple goals are bundled without priority

When you push back, be direct and specific: name what is unclear, why it blocks a good plan, and what decision or fact would unblock it.

**Ask questions to reach the core**

- What outcome must be true when this is finished? What is explicitly out of scope?
- What must not regress (behavior, SLA, compat, security, cost)?
- What is the smallest change that satisfies the outcome?
- What already exists that this should extend instead of replace?
- What is the real constraint: time, risk, review surface, rollout, or learning?
- If we could delete one category of work and still hit the goal, what would it be?
- What would make this plan feel inevitable in hindsight?

Prefer a short numbered question list over a long questionnaire. Ask only what is needed to remove ambiguity — not discovery for its own sake.

**Gate**

- If load-bearing answers are still missing, output **open questions** and a **provisional framing** (assumptions labeled), not a final step-by-step plan.
- Once the goal and constraints are clear, proceed to the Core Prompt and the rest of this skill.

## Output Expectations

**Prioritize the plan in this order:**

1. Goal, constraints, and explicit out-of-scope (one tight block)
2. Simplest viable approach (short narrative — the code judo candidate)
3. Phases or PR slices with clear owners and boundaries
4. Concrete steps per phase (each step: outcome + owner; no vague "clean up" items)
5. Decisions / spikes that must happen before implementation detail
6. Rollout, migration, or rollback only where load-bearing
7. Test and verification tied to outcomes, not generic "add tests"

**Form**

- Prefer fewer, stronger sections over a long flat checklist
- No phase with more than ~10 concrete steps without upfront decomposition rationale
- Call out assumptions explicitly; do not smuggle them into step wording
- If open questions remain, list them first — do not bury them at the end

**Visuals (when they earn their place)**

- Use charts, diagrams (e.g. mermaid), and tables when they materially aid understanding — not as decoration
- Keep visuals **MECE**: mutually exclusive, collectively exhaustive relative to the point they support
- Tie each visual directly to a plan section (scope, sequencing, ownership, rollout, decision) — if it does not change how someone executes, omit it

**Do not**

- Flood the plan with low-value detail when structural choices are still unsettled
- List cosmetic or nice-to-have work before the critical path is clear
- Duplicate the same work across phases without saying why it is split
- Add diagrams or tables that repeat prose, overlap categories, or illustrate "nice to know" context

## Approval Bar

Do not treat a plan as ready to implement merely because it lists tasks.

**Ready only when:**

- The goal and out-of-scope are explicit; no load-bearing ambiguity left unasked or unlabeled
- There is a credible simplest-viable approach, not only a longer schedule
- No clear structural choice schedules messier code when a simpler framing is visible
- No phase exceeds ~10 concrete steps without upfront decomposition rationale
- No happy-path step list sprinkled with unresolved special cases (those live in decisions/spikes)
- No vague load-bearing steps ("refactor as needed", "handle edge cases")
- No unjustified new layers, wrappers, or parallel paths when the canonical owner exists
- Sequencing and rollout are as atomic and parallel as the constraints allow; half-states are explicit if unavoidable
- Assumptions are stated; open questions are listed first, not buried
- Visuals are MECE, tied to execution, and omitted when prose is enough

**Presumptive blockers** (justify clearly or revise the plan):

- The plan preserves incidental complexity when a plausible code-judo move would avoid it
- A single phase is a mega-checklist without decomposition
- Feature work is routed through shared/general paths without isolation
- Load-bearing shape (API, schema, flags, ownership) is deferred without a spike/decision step
- The plan duplicates existing patterns or splits the same complexity across PRs without reducing concepts

If blockers remain, output **revised framing**, **open questions**, or a **labeled provisional plan** — not a final implementation schedule.
