# AI-assisted development workflow

## Contents

1. Delivery loop
2. Phase gates
3. Request modes
4. Environment and release model
5. Measurement and review

## Delivery loop

Use this sequence for product features, automations, integrations, data systems, AI capabilities, refactors, and releases:

```text
Problem
→ Outcome
→ Specification
→ Risk probe
→ Thin vertical slice
→ Automated feedback
→ Controlled real-world validation
→ Progressive release
→ Observation
→ Continue, change, or remove
```

Do not force every task through heavyweight documents. A small reversible fix can express each gate in a few lines. Increase rigor with ambiguity, blast radius, irreversibility, security sensitivity, financial impact, and external dependencies.

## Phase gates

### Gate 1: Problem and outcome

Required:

- current user or operator workflow;
- concrete pain or risk;
- measurable desired outcome;
- primary user;
- evidence that the problem exists.

Stop when the request is only a broad solution idea without a confirmed problem.

### Gate 2: Compact specification

Required:

- user journey;
- inputs and outputs;
- source of truth;
- business rules;
- human and automation boundary;
- failure and recovery behavior;
- acceptance criteria;
- non-goals.

Prefer examples over abstract prose. One valid example and one failure example often reveal missing rules.

### Gate 3: Feasibility and risk

Rank unknowns by:

```text
risk = probability of being wrong × cost of discovering it late
```

Probe the highest-risk assumption first. For external systems, verify each endpoint and token type independently. For AI, evaluate representative inputs before designing automation around the model.

### Gate 4: Minimal design

Confirm:

- stable identity for each entity;
- ownership of every datum;
- state transitions;
- idempotency;
- compatibility;
- security boundary;
- observability;
- rollback.

Reject data or components that do not serve the current vertical slice.

### Gate 5: Vertical implementation

Deliver one complete path. Avoid building all screens, tables, endpoints, or adapters in advance.

Use ports and adapters:

- keep business logic pure and independently testable;
- isolate database, file, network, platform, and model I/O;
- normalize external failures at the boundary while retaining sanitized raw evidence;
- keep irreversible actions behind explicit commands.

### Gate 6: Verification

Use the fastest feedback first:

1. formatting and static checks;
2. unit tests;
3. state and database tests;
4. contract and integration tests;
5. UI critical-path tests;
6. production build;
7. one-object real validation;
8. full regression.

Do not use an end-to-end test to compensate for untested business logic.

### Gate 7: Controlled rollout

Use the smallest meaningful blast radius:

- one record before a batch;
- one account before all accounts;
- one device before the network;
- one user before general access;
- recommendation before automatic action.

Expose freshness and uncertainty. Do not represent eventual synchronization as real time or estimated values as settled truth.

### Gate 8: Operate and learn

Observe:

- adoption and completed jobs;
- manual steps removed;
- lead time from idea to validated value;
- change failure rate;
- recovery time;
- duplicate or uncertain operations;
- support and explanation burden;
- rework caused by late discovery.

If a feature is not used, simplify or remove it rather than expanding it.

## Request modes

### Planning

Output:

- objective;
- user journey;
- scope and non-goals;
- data ownership;
- dependencies and assumptions;
- failure paths;
- acceptance criteria;
- staged implementation order;
- blocking decisions.

Do not change code or external state unless separately authorized.

### Implementation

Output before changing:

- intended behavior;
- affected components;
- verification plan;
- assumptions within scope.

Then implement the smallest complete slice, test it, inspect the diff, and report evidence.

### External integration

Maintain a capability matrix:

| Capability | Application | Credential type | Scope | Read | Write | Readback | Last verified |
| --- | --- | --- | --- | --- | --- | --- | --- |

Treat authentication, authorization, resource visibility, write permission, and cross-account sharing as separate claims.

### AI feature

Define:

- role: draft, classify, extract, recommend, or act;
- representative evaluation set;
- quality threshold;
- abstention and fallback;
- cost and latency ceiling;
- sensitive data policy;
- human review boundary;
- monitoring for model or prompt drift.

Start with offline evaluation, then shadow or recommendation mode, then bounded action only when justified.

### Automation

Define:

- trigger;
- immutable input snapshot;
- idempotency key;
- states and transitions;
- partial-success semantics;
- remote reconciliation;
- retry ownership;
- cancellation;
- audit trail;
- human takeover.

### Release or migration

Use expand-and-contract for incompatible data changes:

1. add backward-compatible structures;
2. deploy code that handles old and new;
3. migrate and verify;
4. switch reads and writes;
5. remove old structures in a later release.

Never bundle an unrelated external business-data migration into a routine software upgrade.

## Environment and release model

Recommended minimal model:

```text
main                  always releasable
short-lived branch    one bounded change
development           isolated ports, data, config, logs
pilot                 smallest real target
production            immutable versioned release
```

Keep mutable state outside release directories:

```text
releases/
current
data/
secrets/
logs/
backups/
```

For solo or local software, a repeatable local verification command is sufficient before adding hosted CI. Add infrastructure only when it removes a demonstrated bottleneck.

## Measurement and review

Use metrics to improve the system, not to reward output volume:

- validated user outcomes;
- cycle time;
- deployment frequency;
- change failure rate;
- recovery time;
- escaped defects;
- rework ratio;
- manual steps and minutes removed;
- feature usage and retention.

Run blameless reviews. Separate:

- direct cause;
- contributing conditions;
- why detection was late;
- recovery quality;
- corrective action;
- prevention;
- work to stop doing.
