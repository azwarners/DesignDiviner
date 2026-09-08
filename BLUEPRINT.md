# DesignDiviner Blueprint

## 1. Product definition

DesignDiviner is a CLI-first architecture orchestrator that transforms one bounded architectural question into one evidence-grounded blueprint and phased implementation plan.

Permanent product promise:

> **One architectural question in. One evidence-grounded blueprint and implementation plan out.**

DesignDiviner is intentionally narrow. It is not a general-purpose coding agent, project manager, or implementation engine.

It does not directly own:

- a model client;
- an agent loop;
- general repository tools;
- general Git tools;
- generalized file tools;
- free-form shell execution;
- code implementation;
- pull-request creation;
- deployment or release management.

Reusable digital labor belongs to Redless. Reusable tools belong to Sidecaravan. Model transport belongs to Ysparr. DesignDiviner owns architecture-pass policy and deliverables.

## 2. Core principles

### Single responsibility

DesignDiviner exists to produce useful architectural decisions and implementation guidance.

### Evidence over assertion

Repository facts must come from observed repository content or tool output. Recommendations and inferences must be distinguishable from facts.

### Read-only by default

An architectural pass should not modify the target repository unless a future explicit mode is designed for writing architecture artifacts back into a controlled destination.

### Redless for labor

DesignDiviner must not grow its own agentic execution loop. It commissions bounded architecture work through Redless.

### Sidecaravan for reusable capabilities

Repository inspection, Git operations, indexing, search, filesystem access, code structure analysis, and similar reusable tools belong in Sidecaravan when they are broadly useful.

### CLI first

The primary interface is a command in `PATH`, usable by a person, Apmatia, scripts, or other applications.

### Durable artifacts

A successful architecture pass produces explicit files that can be reviewed, diffed, reused, handed to another agent, or checked into documentation later.

## 3. Intended users

DesignDiviner should support:

- a human developer or administrator at the shell;
- an Apmatia conversation agent;
- another local application;
- a planning workflow that later feeds ConjurePR;
- a future multi-repository architecture review workflow.

## 4. Architectural boundary

```text
Caller
  |
  v
DesignDiviner
  |
  +-- request validation
  +-- analysis workspace preparation
  +-- deliverable contract
  +-- Redless invocation
  +-- result validation
  +-- artifact persistence
  |
  v
Blueprint + implementation plan

Redless
  |
  +-- repository inspection
  +-- architecture reasoning
  +-- document generation
  +-- Sidecaravan tool use
```

DesignDiviner decides **what must be answered**.

Redless decides **how to investigate and produce the answer**.

## 5. Relationship to the rest of the stack

```text
Sidecaravan = reusable capabilities
Redless     = reusable digital labor
Ysparr      = reusable AI transport
Applications = policy + orchestration + purpose
```

DesignDiviner is an application in that model.

Apmatia may expose DesignDiviner conversationally, but DesignDiviner must remain independently usable from the shell.

ConjurePR is complementary:

```text
DesignDiviner
  -> architecture and implementation planning

ConjurePR
  -> one bounded implementation issue to one tested draft PR
```

A planned architecture may later be decomposed into bounded implementation items for ConjurePR.

## 6. Request model

A request should identify:

- target repository or workspace;
- architectural question or desired change;
- optional goals;
- optional constraints;
- optional non-goals;
- optional required deliverables;
- optional depth or scope limits.

Conceptual CLI:

```bash
designdiviner run \
  --repo /path/to/project \
  --request architecture-request.md
```

Machine callers should be able to request structured output:

```bash
designdiviner run ... --json
```

The exact CLI is deferred until implementation, but the product remains CLI-first.

## 7. Pass types

The initial implementation should support a small number of explicit architecture-pass intents rather than an open-ended plugin system.

Potential pass types:

### Target-state design

Given a current repository and requested outcome, describe the recommended target architecture.

### Current-state review

Describe the existing architecture, key boundaries, risks, duplication, coupling, and areas of concern.

### Refactor design

Plan a bounded architectural refactor without implementing it.

### Integration design

Plan how one system or reusable stack component should integrate with another.

### Multi-repository review

Deferred initially, but DesignDiviner should eventually be able to reason across several repositories and recommend shared abstractions, consolidation, or interface changes.

Avoid creating dozens of named pass modes. A small explicit set is preferable.

## 8. Required artifacts

A normal successful pass should produce:

```text
architecture-summary.md
blueprint.md
implementation-plan.md
risks.md
open-questions.md
sources-or-evidence.md
result.json
```

Not every request requires every file, but `blueprint.md`, `implementation-plan.md`, and `result.json` should be core outputs.

## 9. Blueprint expectations

The blueprint should include only sections relevant to the request, selected from areas such as:

- problem statement;
- observed current state;
- architectural goals;
- non-goals;
- proposed target state;
- subsystem boundaries;
- responsibilities;
- interfaces and contracts;
- data flow;
- control flow;
- persistence or state implications;
- dependency implications;
- security implications;
- failure behavior;
- observability implications;
- migration strategy;
- compatibility concerns;
- testing strategy;
- operational concerns;
- deferred work.

Do not mechanically produce giant templates filled with irrelevant headings.

## 10. Implementation-plan expectations

The implementation plan translates the blueprint into ordered work.

It should contain:

- phases or milestones;
- intended outcome of each phase;
- dependencies between phases;
- likely repository areas affected;
- validation or exit criteria;
- migration or compatibility requirements;
- risks that should be handled before advancing;
- opportunities to split work into bounded tasks.

Where useful, the plan should identify units suitable for ConjurePR without requiring DesignDiviner itself to create pull requests.

## 11. Evidence model

Architecture recommendations should be traceable to evidence.

Useful evidence may include:

- repository paths;
- source files;
- configuration;
- dependency declarations;
- tests;
- documentation;
- code structure or symbol relationships;
- Git history when intentionally requested;
- Sidecaravan-generated repository summaries;
- user-supplied constraints.

`sources-or-evidence.md` should make it possible to distinguish:

```text
Observed fact
Inference
Recommendation
User-supplied requirement
Unknown / unresolved question
```

This is important because architecture documents often accidentally turn assumptions into facts.

## 12. Workspace model

DesignDiviner should operate against a controlled analysis workspace.

For a local repository, initial implementation may use the supplied repository read-only when practical, or create a temporary analysis copy if stronger isolation is needed.

For remote repository support, a later phase may create disposable clones.

The architecture pass must not modify application source by default.

DesignDiviner-owned output belongs outside the target source tree unless the user explicitly requests a controlled destination.

## 13. Redless contract

DesignDiviner should invoke Redless with a bounded architecture task containing:

- the architectural question;
- target workspace;
- goals;
- constraints;
- non-goals;
- required artifacts;
- read-only expectations;
- evidence requirements;
- output location;
- runtime or labor limits where supported.

Conceptually:

```text
DesignDiviner request
      |
      v
bounded Redless architecture task
      |
      v
Redless performs investigation and writing
      |
      v
DesignDiviner validates artifacts
```

DesignDiviner should not depend on Redless's internal reasoning format. The boundary should be task input plus observable result artifacts/status.

## 14. Result validation

DesignDiviner must independently validate structural outcomes that do not require architectural judgment.

Examples:

- required artifact files exist;
- files are non-empty;
- requested sections or deliverables are represented;
- output paths remain within the designated result directory;
- result JSON parses and matches the pass status;
- obvious unresolved placeholders are reported;
- the target repository has not been modified during a read-only pass;
- Redless execution completed successfully.

DesignDiviner should not pretend that deterministic validation can prove an architecture is correct. Human review remains authoritative.

## 15. Result model

Conceptual machine-readable result:

```json
{
  "status": "completed",
  "pass_id": "dd-0042",
  "target": "/path/to/project",
  "request": "Redesign model transport boundaries",
  "artifacts": {
    "blueprint": ".../blueprint.md",
    "implementation_plan": ".../implementation-plan.md",
    "risks": ".../risks.md",
    "evidence": ".../sources-or-evidence.md"
  },
  "open_questions": 3
}
```

Human output should summarize completion and point directly to artifacts.

## 16. Failure behavior

A pass may end as:

```text
completed
needs_clarification
insufficient_evidence
too_broad
failed
cancelled
interrupted
```

`too_broad` and `needs_clarification` are valid architecture outcomes, not crashes.

If evidence is insufficient, preserve partial artifacts and state the missing information rather than fabricating certainty.

## 17. Scope control

DesignDiviner should resist architectural scope creep.

A request like:

```text
Review the whole company architecture and tell me everything that should change.
```

should normally be narrowed.

A request like:

```text
Review how model request transport is duplicated across these three applications and propose one reusable boundary.
```

is appropriate.

Multi-repository analysis may be broad in file count while still bounded by a specific architectural question.

## 18. Sidecaravan usage

DesignDiviner should prefer Sidecaravan capabilities when they exist for reusable analysis tasks.

Examples may eventually include:

```text
repository tree summaries
symbol relationships
Git metadata
changed-file history
text search
structural search
codebase indexing
classical retrieval
architecture-oriented repository summaries
```

DesignDiviner-specific policy and deliverable logic stays in DesignDiviner.

General tooling does not.

## 19. No direct model integration

DesignDiviner should not have an OpenAI-compatible model configuration of its own merely to perform architecture reasoning.

Redless owns model-backed digital labor.

Ysparr may be involved underneath Redless or elsewhere in the stack, but DesignDiviner should not become another model client.

## 20. CLI model

Target commands may eventually include:

```text
designdiviner run
designdiviner show
designdiviner artifacts
designdiviner blueprint
designdiviner plan
designdiviner cancel
```

Initial implementation should remain smaller if possible.

A single foreground `run` command plus structured output may be enough for the first usable release.

Detached execution can be added later without requiring a permanent server.

## 21. Storage

DesignDiviner should use simple durable filesystem artifacts before considering any database.

Conceptual layout:

```text
~/.local/share/designdiviner/
└── passes/
    └── dd-0001/
        ├── request.md
        ├── state.json
        ├── architecture-summary.md
        ├── blueprint.md
        ├── implementation-plan.md
        ├── risks.md
        ├── open-questions.md
        ├── sources-or-evidence.md
        ├── result.json
        └── redless/
```

Exact paths are implementation details.

## 22. Security and safety

Initial principles:

- do not run as root;
- do not modify the target repository during read-only passes;
- do not expose credentials to Redless unnecessarily;
- keep outputs within controlled directories;
- treat repository content as untrusted data;
- avoid arbitrary host-management capabilities;
- rely on Sidecaravan and Redless safety boundaries rather than duplicating unsafe general tools locally;
- record failures and uncertainties truthfully.

## 23. Implementation phases

### Phase 1: CLI and artifact skeleton

Deliver:

- Python package;
- `designdiviner` command;
- request loading;
- pass IDs;
- result directories;
- state and artifact model;
- human and JSON output;
- tests without Redless dependency.

Success means a request can be accepted and a durable pass workspace can be created.

### Phase 2: Redless invocation

Add:

- Redless executable/config discovery;
- bounded task construction;
- foreground execution;
- cancellation and timeout behavior where practical;
- Redless stdout/stderr capture;
- result status handling;
- mocked tests.

No custom agent loop.

### Phase 3: Architecture artifact contract

Add:

- required blueprint structure;
- implementation-plan structure;
- evidence classification;
- open-question handling;
- structural validation;
- clear distinction between observed facts and recommendations.

### Phase 4: Repository-analysis integration

Add or adopt Sidecaravan capabilities useful for architectural passes.

DesignDiviner should orchestrate these through Redless rather than duplicate general tooling.

### Phase 5: Reliability and deeper passes

Add:

- interruption handling;
- detached execution if useful;
- richer diagnostics;
- configurable pass depth;
- explicit pass types;
- end-to-end tests;
- artifact retention/cleanup policy.

### Phase 6: Multi-repository architecture review

Add bounded multi-repository workspaces for questions about:

- duplicated functionality;
- shared abstractions;
- interface alignment;
- DRY opportunities;
- stack consolidation;
- compounding reuse across applications.

Keep the question bounded even when several repositories are involved.

## 24. Explicitly deferred

Do not initially implement:

- a web server;
- a GUI;
- a second agent loop;
- direct model-provider integrations;
- autonomous code implementation;
- pull-request creation;
- automatic execution of the implementation plan;
- broad project-management systems;
- distributed workers;
- external databases;
- plugin frameworks;
- autonomous architecture approval;
- automatic handoff of every plan item to ConjurePR.

## 25. Definition of done for the first complete release

A user or agent can:

1. invoke `designdiviner` from the shell;
2. provide a repository and bounded architectural request;
3. have DesignDiviner prepare an analysis pass;
4. have Redless perform the investigative digital labor;
5. receive an evidence-grounded blueprint;
6. receive a phased implementation plan;
7. see explicit risks and unresolved questions;
8. consume the result as human-readable files or structured JSON;
9. hand bounded implementation items to another workflow such as ConjurePR;
10. trust that DesignDiviner did not quietly become another coding-agent framework.

Permanent promise:

> **Give DesignDiviner one bounded architectural question. It will return an evidence-grounded blueprint and implementation plan suitable for human review and downstream implementation.**
