# DesignDiviner

**One architectural question in. One evidence-grounded blueprint and implementation plan out.**

DesignDiviner is a CLI-first architecture orchestrator. It prepares a bounded analysis workspace, delegates repository inspection and architectural reasoning to [Redless](https://github.com/azwarners/Redless), verifies that the resulting artifacts address the requested design problem, and returns a durable blueprint and implementation plan for human or downstream-agent use.

DesignDiviner is intentionally not a second general-purpose agent framework. It does not implement its own coding loop, model client, repository-analysis tools, file tools, or free-form agent runtime.

## Architecture

```text
User / Apmatia / another application
              |
              v
        DesignDiviner
              |
              +-- validate architectural request
              +-- prepare bounded analysis workspace
              +-- define required deliverables
              +-- invoke Redless
              +-- verify blueprint completeness and evidence
              +-- persist architecture artifacts
              +-- return human and machine-readable results
                         |
                         v
              blueprint + implementation plan

Redless
   |
   +-- performs repository inspection and architectural reasoning
   +-- may use Sidecaravan for reusable agent tools
```

The project boundaries are deliberate:

- **Sidecaravan** owns reusable machine-oriented capabilities such as Git, filesystem access, codebase search, repository indexing, and other general tools.
- **Redless** performs reusable digital labor and owns the agentic execution loop.
- **Ysparr** handles reusable OpenAI-compatible model request/response transport.
- **DesignDiviner** owns architectural-pass policy, requested deliverables, orchestration, and result validation.
- **ConjurePR** turns one bounded implementation issue into one tested draft pull request.
- **Apmatia** may expose DesignDiviner, ConjurePR, Redless, Sidecaravan, and other applications to conversational agents and users.

## Why a CLI?

DesignDiviner should work equally well for a person at a shell prompt, an Apmatia agent, a script, or another local application. Its primary interface is therefore a command in `PATH`, with machine-readable output available for callers.

A permanent HTTP service is not part of the target architecture. Long-running or detached passes can be added later without requiring a continuously running server.

## Intended workflow

```text
Architectural request
  -> validate target and requested scope
  -> prepare analysis workspace
  -> capture repository and project context
  -> define required blueprint sections
  -> invoke Redless
  -> inspect generated architecture artifacts
  -> verify evidence and requested coverage
  -> produce final blueprint
  -> produce phased implementation plan
  -> persist artifacts
  -> return result
```

Redless decides **how to investigate and reason about the system**. DesignDiviner decides **what architectural question is being answered, what artifacts must be produced, and whether the result is complete enough to hand to a human or implementation orchestrator**.

## Typical outputs

A DesignDiviner pass should normally produce:

```text
architecture-summary.md
blueprint.md
implementation-plan.md
risks.md
open-questions.md
sources-or-evidence.md
result.json
```

Depending on the request, the blueprint may include:

- current-state architecture;
- target-state architecture;
- subsystem boundaries;
- data and control flows;
- interfaces and contracts;
- dependency changes;
- migration strategy;
- security and operational implications;
- testing implications;
- phased implementation milestones;
- explicit non-goals and deferred work.

## Relationship to ConjurePR

DesignDiviner and ConjurePR sit at different levels of the stack:

```text
DesignDiviner
    |
    +-- "How should this system change?"
    +-- produces blueprint + implementation plan

ConjurePR
    |
    +-- "Implement this one bounded issue."
    +-- produces tested draft PR
```

A future workflow may use DesignDiviner to create a multi-phase implementation plan and then submit individual bounded implementation items to ConjurePR.

## Permanent rules

DesignDiviner must not:

- implement a second agent loop when Redless already provides reusable digital labor;
- duplicate reusable repository or analysis tools that belong in Sidecaravan;
- silently modify the target repository during a read-only architecture pass;
- present unsupported architectural claims as facts;
- fabricate repository evidence, test results, or constraints;
- turn an architectural pass into unbounded implementation work;
- merge planning and implementation into one opaque autonomous job.

Architecture should be evidence-grounded. Where a recommendation is an inference rather than an observed repository fact, the output should say so.

## Status

DesignDiviner is currently at the architecture-definition stage. See [BLUEPRINT.md](BLUEPRINT.md) for the implementation roadmap.
