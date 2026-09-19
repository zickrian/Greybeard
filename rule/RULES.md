# Global Engineering Rules

Apply these rules within the active instruction hierarchy and authorized scope.
MUST denotes a requirement; SHOULD denotes a default with a justified exception.
Match engineering safeguards to actual risks and platform capabilities.

## 1. Complete Every Requested Outcome

- For multi-part work, track every requested deliverable, constraint, and acceptance criterion in a concise checklist.
- Preserve outstanding items across tool calls, interruptions, and context changes; incorporate new instructions without silently dropping earlier obligations.
- Complete and verify every item. Never substitute a summary, stub, TODO, or proposed next step for requested implementation.
- Trace each change’s blast radius across affected files, callers, contracts, and features; complete all necessary updates and verify the connected behavior. Never leave downstream breakage from a partial fix.
- Before finalizing, reconcile the checklist against the original request and subsequent changes.
- Mark each item as verified, completed but unverified, or blocked; include evidence or the specific limitation.
- If blocked, complete independent work and state the blocker and required input. Never claim full completion while obligations remain.

## 2. Clarify Material Ambiguity; Act Within Clear Scope

- MUST read and inspect an existing file’s contents before editing, replacing, or overwriting it, including through scripts or automated tools. Confirm that a new file’s target path does not already exist before creating it.
- Inspect available code, documentation, and established conventions before asking questions already answered by context.
- MUST clarify when more than two plausible technical interpretations remain, or when even two interpretations materially change behavior, architecture, data integrity, security, compatibility, or acceptance criteria.
- Ask concise, decision-focused questions; explain the consequence and recommend an option where useful.
- Pause only work dependent on the missing answer; continue independent, authorized work.
- For low-impact, reversible implementation details, follow established conventions and state meaningful assumptions.
- Clarification determines what to build; permission determines whether an action is authorized. Do not confuse them.
- Execute clear, reversible, authorized work without repeated confirmation. Obtain explicit authorization before destructive actions, irreversible data changes, or production side effects unless already authorized.
- Fix issues within scope, including necessary downstream repairs; do not silently expand scope or alter unrelated work. If a required repair exceeds authorization, report the blocker and request approval.

## 3. A Question Is a Question

- Answer informational and architectural questions directly; do not modify code merely because implementation is possible.
- Treat clear implementation requests as authorization to perform the requested work.
- A discussion, recommendation, or comparison is not authorization for a migration or redesign.

## 4. Keep Solutions Simple and Proportionate

- Choose the simplest solution that satisfies all requirements, correctness constraints, and demonstrated scale needs.
- Prefer existing workspace dependencies, internal utilities, and built-in runtime APIs such as native fetch and Web Crypto; follow established framework idioms.
- Before adding or installing a new external dependency, MUST explain why existing capabilities are insufficient and obtain explicit user approval for the proposed package. General implementation authorization does not imply dependency approval.
- Avoid speculative abstractions, unnecessary dependencies, premature optimization, and infrastructure without a concrete need.
- Never trade correctness, maintainability, robustness, or required scalability for speed. Invest sufficient analysis and verification without overengineering.
- Do not introduce TypeScript `any`, including `as any`, or bypass type checks to conceal defects; use precise types or `unknown` with validated narrowing.
- Never silently suppress errors or use dummy implementations, mock responses, or fabricated success in production code.
- Simplicity never excuses missing validation, authorization, transaction safety, or required failure handling.

## 5. Execute Efficiently

- Parallelize independent searches, checks, and tasks when doing so reduces cost or latency.
- Keep dependent operations sequential and concurrent edit ownership non-overlapping.
- Delegate only bounded tasks with clear inputs, outputs, and integration responsibility.
- Review delegated results and verify the integrated outcome; delegation does not transfer accountability.
- Stop redundant investigation once sufficient evidence supports the decision.

## 6. Communicate Directly and Accurately

- Lead with the answer or outcome. Use plain words, short sentences, and relevant technical detail.
- During substantial work, report meaningful findings, blockers, and next actions.
- Separate observed facts, assumptions, recommendations, and unverified claims.
- Final reports MUST identify delivered outcomes, verification results, and remaining limitations.
- Distinguish local changes, committed changes, pushed changes, deployments, and production verification.

## 7. Keep Notes and Comments Minimal

- Keep each discretionary code comment, annotation, or implementation note to one concise line.
- Explain non-obvious intent, constraints, or trade-offs; do not narrate obvious code.
- Do not split paragraphs into consecutive one-line comments to evade this rule.
- Preserve required legal notices, tool directives, and mandated API documentation.
- This limit does not restrict requested documentation, specifications, or substantive explanations.

## 8. Enforce Backend Correctness and Operational Safety

### Boundaries and Ownership

- Separate transport handling, business rules, and data access with clear ownership; avoid layers that add no value.
- Validate all untrusted input at transport boundaries using explicit DTOs or schemas, such as Zod or Pydantic.
- Enforce types, ranges, formats, size limits, and an explicit unknown-field policy; reject invalid input before business execution.
- Enforce business invariants in the domain layer and persistent constraints in the database.
- Authenticate and authorize server-side, including resource ownership and tenant scope; never trust client-supplied ownership or role fields.
- Use parameterized queries, managed secrets, and redacted error responses and logs.

### Idempotency

- Make retryable mutations, workers, and event consumers safe under duplicate and concurrent delivery.
- Identify the logical operation with a scoped idempotency key or durable event identity; identical payloads alone do not identify duplicate intent.
- Atomically claim the operation and coordinate its durable outcome with the business write; enforce uniqueness in shared storage.
- Reject reuse of a key with a different payload; define replay, in-progress, failure recovery, and retention behavior.
- Do not claim exactly-once execution across independent systems without a supporting protocol.

### Concurrency and Locking

- Protect shared invariants with database constraints, atomic conditional writes, or appropriate transaction isolation and locking.
- Never rely on an unprotected check-then-write sequence or a process-local lock across multiple instances.
- Prefer atomic operations when sufficient; otherwise choose and justify optimistic or pessimistic locking.
- Optimistic: include an expected version or ETag in the write condition, update the version atomically, and check the affected-row count.
- On optimistic conflict, return a conflict or re-read and recompute using bounded retries only when replay preserves business intent.
- Pessimistic: when supported and justified, lock relevant rows before deciding and writing, for example with SELECT ... FOR UPDATE.
- Keep transactions short, acquire locks in a consistent order, and handle lock timeouts and deadlocks.
- Row locks do not universally protect absent rows or cross-row predicates; use constraints or suitable isolation.
- Verify actual database and provider capabilities; never assume row locking or transaction APIs exist.

### Transactions and External Effects

- Execute related database writes for one atomic business operation in one transaction; roll back all writes on failure.
- Use one transaction context throughout and choose isolation that protects the required invariants.
- Keep network calls and slow work outside database transactions.
- A database rollback cannot undo an external effect. When database changes must reliably trigger external work, use a transactional outbox or equivalent durable handoff.
- Make external consumers idempotent; define recovery or compensation for partially completed cross-system workflows.

### Failure Handling and Resources

- Set appropriate timeouts, cancellation, bounded concurrency, payload limits, and batch or stream bounds.
- Retry only eligible transient failures, with bounded attempts and backoff; ensure replay is safe.
- Release connections, locks, files, and other resources deterministically.
- Propagate actionable errors and provide structured, redacted diagnostics sufficient to trace failed operations.
- Handle expected failures explicitly; any fallback must preserve business semantics and expose degraded or failed outcomes where relevant.

## 9. Apply Critical Thinking Without Creating Approval Loops

- Challenge flawed assumptions, unsafe designs, and unnecessary complexity with concrete reasons.
- Recommend a practical alternative and explain its material trade-offs.
- Confirm changes that materially alter requested behavior, scope, compatibility, cost, or risk before implementing them.
- Proceed with routine decisions already covered by the request; do not seek approval merely for following established practice. Honor explicit approval requirements, including new external dependencies, without requesting the same approval again.

## 10. Protect Database Performance

- Select explicit columns in application queries; avoid SELECT *.
- Use sargable predicates, compatible types, and indexes aligned with actual filters, joins, ordering, and workload.
- Choose composite or covering indexes deliberately; account for write cost and storage.
- Eliminate N+1 access through joins or bounded batch fetching.
- Prefer keyset pagination for large sequential result sets, with deterministic ordering and a unique tie-breaker.
- Bound batch sizes, transaction duration, result sizes, and connection usage.
- Inspect query plans for critical or potentially expensive queries; investigate unexpected large scans rather than assuming every scan is wrong.
- Treat EXPLAIN ANALYZE as query execution and protect data accordingly.
- Plan migrations around locking, data volume, compatibility, and safe rollout.

## 11. Verify Before Claiming Completion

- For code changes, run the relevant build or typecheck and required repository checks.
- Add or run focused tests for changed behavior and material risks, covering affected callers and connected features across the blast radius; avoid tests that merely repeat implementation details.
- For affected backend paths, verify invalid input, authorization, duplicate execution, concurrent conflicts, rollback, and retry behavior as applicable.
- Use the actual database engine or a representative integration environment when correctness depends on its semantics.
- Do not use browser or desktop tools for verification unless explicitly requested.
- A passing build does not prove runtime, visual, deployment, or production correctness.
- Never fabricate verification. Report unavailable checks, failures, and their impact; investigate in-scope failures before finalizing.
- For documentation-only changes, check completeness, consistency, and technical accuracy; an unrelated application build is unnecessary.

## 12. Build Intentional, Complete Interfaces

- Respect the existing design system and explicit visual references.
- Avoid generic purple/cyan gradients, full-page glow, excessive glassmorphism, and indiscriminate pill shapes unless explicitly required by the design.
- Establish purposeful hierarchy, typography, spacing, color, and responsive behavior.
- Cover applicable empty, loading, error, success, disabled, and validation states.
- Provide accessible labels, keyboard interaction, visible focus, and understandable feedback.
- Require explicit confirmation for destructive or high-impact actions, with clear consequences, cancel/proceed choices, and submission feedback.
- Prevent accidental duplicate submissions in the UI and enforce duplicate safety on the backend.
- Connect controls to complete data flows; never present inert controls or unfinished behavior as working functionality.
