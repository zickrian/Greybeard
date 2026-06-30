---
name: domain-driven-design
description: "Use when modeling a complex business domain: defining bounded contexts, aggregates, entities, value objects, domain events, ubiquitous language, or deciding on CQRS/event-sourcing/sagas. For general architecture choices use the architecture skill instead."
risk: safe
source: self
tags: "[ddd, domain, bounded-context, architecture]"
date_added: "2026-02-27"
---

# Domain-Driven Design

## Use this skill when

- You need to model a complex business domain with explicit boundaries.
- You want to decide whether full DDD is worth the added complexity.
- You need to connect strategic design decisions to implementation patterns.
- You are planning CQRS, event sourcing, sagas, or projections from domain needs.

## Do not use this skill when

- The problem is simple CRUD with low business complexity.
- You only need localized bug fixes.
- There is no access to domain knowledge and no proxy product expert.

## Instructions

1. Run a viability check before committing to full DDD.
2. Produce strategic artifacts first: subdomains, bounded contexts, language glossary.
3. Route to specialized skills based on current task.
4. Define success criteria and evidence for each stage.

### Viability check

Use full DDD only when at least two of these are true:

- Business rules are complex or fast-changing.
- Multiple teams are causing model collisions.
- Integration contracts are unstable.
- Auditability and explicit invariants are critical.

### Focus areas (work these phases inline — no external sub-skills required)

- **Strategic model & boundaries**: identify subdomains (core/supporting/generic), draw bounded contexts, write the ubiquitous-language glossary.
- **Context mapping**: define integration relationships between contexts (partnership, customer/supplier, conformist, anti-corruption layer).
- **Tactical modeling**: aggregates, entities, value objects, domain events, repository contracts.
- **CQRS** (only if read/write loads diverge): separate command and query models with an explicit rationale.
- **Event sourcing** (only if event history must be the source of truth): event schema + versioning, event store design, projection/read-model rebuild strategy.
- **Sagas**: compensation matrix for long-running, multi-context workflows.
- **Decision log**: record each non-obvious choice as an ADR — use the `architecture` skill's ADR template.

If templates are needed, open `references/ddd-deliverables.md`.

## Output requirements

Always return:

- Scope and assumptions
- Current stage (strategic, tactical, or evented)
- Explicit artifacts produced
- Open risks and next step recommendation

## Examples

```text
Use @domain-driven-design to assess if this billing platform should adopt full DDD.
Then route to the right next skill and list artifacts we must produce this week.
```

## Limitations

- This skill does not replace direct workshops with domain experts.
- It does not provide framework-specific code generation.
- It should not be used as a justification to over-engineer simple systems.
