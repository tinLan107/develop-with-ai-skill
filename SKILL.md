---
name: develop-with-ai
description: Plan, implement, test, integrate, release, operate, review, or improve software with a specification-driven, evidence-first, small-batch AI development workflow. Use for any software product, automation, API integration, data system, AI feature, website, internal tool, refactor, deployment, incident follow-up, or technical roadmap where Codex should reduce rework, validate uncertain capabilities early, preserve production stability, and deliver small testable reversible changes.
---

# Develop with AI

Build software through evidence, small vertical slices, automated feedback, controlled real-world validation, and reversible releases. Treat AI as an implementation accelerator, not the authority for product decisions, security boundaries, or high-impact external actions.

## Establish authority and context

Before planning or changing anything:

1. Read the nearest repository instructions completely, including `AGENTS.md` and files it requires.
2. Inspect the relevant source, tests, documentation, Git state, runtime boundaries, and existing releases.
3. Apply this authority order when sources conflict:
   - latest explicit user decision;
   - repository and directory instructions;
   - confirmed specifications and decision records;
   - current implementation and tests;
   - generic engineering guidance in this skill.
4. Stop and identify the conflict if resolving it would change confirmed business rules, data models, formulas, public behavior, security boundaries, or irreversible external state.
5. Preserve user changes and the currently working production version.

## Classify the request

Choose the smallest applicable mode:

- **Explain or audit**: inspect and report evidence; do not mutate.
- **Discover or plan**: define the problem, scope, workflow, risks, acceptance criteria, and excluded work; do not treat the plan as implementation authorization.
- **Implement or fix**: deliver the smallest complete vertical slice and verify it.
- **Integrate an external system**: prove credentials, scope, identity mapping, read capability, write capability, and readback separately.
- **Release or operate**: protect data, secrets, service availability, rollback, and traceability.
- **Review an incident or project**: separate symptom, root cause, contributing conditions, wasted effort, corrective action, and prevention.

For detailed phase gates and outputs, read [references/workflow.md](references/workflow.md).

## Start from one user outcome

Express the work in one sentence:

> A specific user can complete a specific job with a measurable improvement.

Define:

- current workflow and pain;
- desired outcome and success measure;
- one primary capability for this version;
- explicit non-goals;
- human decisions versus automated decisions;
- irreversible or high-impact actions;
- the smallest real example that proves value.

Do not begin with tables, frameworks, services, agents, or future feature lists. Keep work in progress to one primary feature unless independent parallel work is explicitly requested.

## Write a compact specification

Before implementation, establish:

- entry point and user journey;
- inputs, outputs, and source of truth;
- stable identities and cross-system mappings;
- states and allowed transitions;
- failure, partial-success, timeout, retry, cancellation, and recovery behavior;
- idempotency key or duplicate-prevention rule;
- security and privacy boundaries;
- acceptance criteria;
- excluded work.

Ask only questions whose answers materially change the result or risk. Otherwise state reasonable assumptions and continue.

Use the readiness checklist in [references/checklists.md](references/checklists.md) for complex or high-risk work.

## Attack uncertainty before volume

Identify the assumption most likely to invalidate the design and test it first.

Examples:

- Does the API endpoint actually work with this exact application and token?
- Is the required field type supported by the write API, not merely by the UI?
- Can the deployed host reach the file, database, callback, or device?
- Can names be mapped uniquely, or are platform IDs required?
- Is AI output accurate, stable, affordable, and safe enough for the intended role?

Use a disposable probe, test table, sample record, mock, sandbox, or one-object read-only call. Record sanitized evidence. Do not build the full system around an unverified assumption.

## Build a thin vertical slice

Prefer one end-to-end path over complete horizontal layers:

```text
one user
→ one object
→ one action
→ one persisted result
→ one visible outcome
→ one recoverable failure
```

Implement in this order when applicable:

1. pure business logic;
2. state and persistence;
3. external read adapter;
4. user preview;
5. internal task or command;
6. explicitly confirmed external write;
7. readback and reconciliation;
8. failure recovery and observability.

Expand to batches, multiple accounts, automation, AI, and optimization only after the single-object path is proven.

## Keep architecture proportional

Prefer the existing stack and the fewest moving parts that satisfy current requirements.

- Put a capability in the existing project when it shares users, master data, workflow, deployment, and lifecycle.
- Prototype an uncertain API, model, or data source in an isolated module or sandbox, then integrate it.
- Create a separate project or service only when ownership, data, security, scaling, deployment, or lifecycle is genuinely independent.
- Do not add microservices, queues, caches, orchestrators, agents, or configuration systems without a demonstrated constraint.
- Reuse stable modules; do not duplicate master data merely for interface convenience.

## Separate development from production

Keep the stable release usable while developing:

- production runs a versioned, immutable release;
- development uses an isolated branch or worktree, ports, configuration, data, and logs;
- development disables real external writes by default;
- secrets and live data remain outside source and release artifacts;
- schema changes are backward compatible until the new version is verified;
- rollback is designed before deployment.

Use short-lived branches and small self-contained changes. Keep the default branch releasable.

## Verify continuously

Match verification to risk:

- pure logic: normal, empty, invalid, boundary, and divide-by-zero cases;
- state: transitions, duplicates, cancellation, retries, restarts, and concurrency;
- database: constraints, migrations, rollback compatibility, and representative data;
- external APIs: sanitized contract fixtures, permission errors, expiry, rate limits, timeouts, partial success, and response drift;
- UI: critical user actions, loading, empty, blocked, success, partial-success, and failure states;
- security: input validation, secret redaction, least privilege, and dependency review;
- release: clean build, checksums, configuration templates, health checks, backup, restore, and rollback.

Run the narrow tests during iteration and the full relevant regression before release. Never claim a real integration or write succeeded based only on mocks, static checks, or a successful HTTP transport code.

## Control consequential actions

For money, publishing, deletion, messaging, account permissions, production data, or external platform writes:

```text
prepare
→ show exact preview
→ obtain explicit confirmation
→ execute once
→ read back
→ classify as succeeded, failed, partial, or uncertain
```

Use stable idempotency keys. If a write times out or its result is uncertain, query the remote system before retrying. Never silently broaden the target set, change parameters, repair review failures, or retry irreversible work.

Pause for confirmation when the work would:

- change confirmed business meaning or scope;
- add, delete, rename, or reinterpret important data;
- modify formulas, permissions, security boundaries, or public interfaces;
- perform external writes, deletions, spending, publishing, or messaging not already authorized;
- choose among ambiguous business objects;
- retry an uncertain external result;
- expose services or data beyond the confirmed trust boundary.

## Release progressively

Release in increasing scopes:

1. local automated verification;
2. isolated development integration;
3. one user, account, record, device, or tenant;
4. readback and observation;
5. wider rollout.

Before release:

- back up state;
- record version, source revision, dependencies, build time, tests, migrations, and artifact digest;
- verify health and readiness;
- confirm secrets and business data are excluded;
- define rollback;
- preserve the previous release.

After release, measure actual use, manual steps removed, failure rate, recovery time, rework, and support burden. Expand only when evidence shows value.

## Use AI with explicit guardrails

Give AI bounded tasks with context, examples, acceptance criteria, allowed files, forbidden changes, and required tests.

Use AI freely for:

- repository research;
- implementation planning;
- repetitive code;
- tests and fixtures;
- local diagnostics;
- documentation aligned to verified behavior;
- review and refactoring inside confirmed boundaries.

Reserve human judgment for:

- problem selection and priority;
- ambiguous business rules;
- security and privacy tradeoffs;
- credentials and permission expansion;
- financial or reputational actions;
- destructive or irreversible changes;
- acceptance of real-world results.

Validate all AI-generated work. A plausible explanation, generated test, or green mock is not evidence of production correctness.

## Finish with a decision-ready handoff

Report:

- outcome delivered;
- scope and explicit non-goals;
- changed artifacts;
- tests and validation tier;
- external reads and writes;
- data or schema impact;
- release and rollback status;
- remaining risks or blockers;
- the single next recommended step.

Use [references/checklists.md](references/checklists.md) for the final definition of done. Read [references/standards.md](references/standards.md) only when explaining, reviewing, or updating the engineering principles behind this workflow.
