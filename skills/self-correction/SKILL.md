---
name: self-correction
description: "Use during any long, multi-step, or stuck task to catch your own failure modes BEFORE they compound: repeated guessing, looping on a dead-end approach, drifting from the original goal, losing track of state in a long session, or fabricating when uncertain. The metacognition layer — run it on yourself, not the code."
category: reasoning
risk: safe
source: frontier-tuning
tags: [metacognition, self-correction, anti-hallucination, reliability]
---

# Self-Correction (Know When You're Going Wrong)

Most failures on hard tasks are not knowledge gaps — they are **failures to notice you
are already off the rails**. Capable models still loop, drift, and bluff. This skill is
the check you run on *yourself* while working, not on the code.

Cheap to run, expensive to skip. When in doubt, run it.

---

## The Five Failure Modes (catch these in yourself)

### 1. Repeated Guessing
**Signal:** You've tried 2 things for the same problem and neither worked, and your next
idea is also a guess ("maybe it's X… let me try Y…").

**Stop rule:** After **2 unverified attempts at the same problem**, you are no longer
debugging — you are guessing. Halt. Do exactly one of:
- Gather real evidence (read the actual file, run a command, print a value, check a log).
- State your top 2 hypotheses to the user and ask which to pursue.

Never make a 3rd blind attempt. A 3rd guess is how a 10-minute fix becomes an hour of damage.

### 2. Dead-End Looping
**Signal:** You keep patching the same area, each fix creating a new error, the diff
growing, the original error mutating but never disappearing.

**Stop rule:** If a fix produces a *new* error twice in a row, the **approach** is wrong,
not the details. Do not patch further. Instead:
- **Revert to the last known-good state.** A clean revert beats five layered patches.
- Re-read the problem from scratch. State a *different* approach, not a tweak of the same one.

### 3. Goal Drift
**Signal:** You're deep in a sub-problem (a type error, a config quirk) and haven't
referenced the user's original request in several steps.

**Stop rule:** Every few steps on a long task, restate in one line: *"Original goal: X.
This step serves it by: Y."* If you can't connect the step to the goal, you've drifted —
stop the rabbit hole and return to the goal. Solving the wrong problem perfectly is still failure.

### 4. State Loss in Long Sessions
**Signal:** The conversation is long; you're unsure what you already changed, what passed,
or what's still open. You're about to re-do or contradict earlier work.

**Stop rule:** When the task spans many steps, **maintain a running ledger** (in your
reasoning or written to the user):
- DONE: what's changed and *verified*.
- OPEN: what's left.
- ASSUMING: assumptions still unconfirmed.
Refresh it before each major step. Never trust vague memory of a long session over a written record.

### 5. Bluffing Under Uncertainty
**Signal:** You're about to state an API signature, a file's contents, a library's
behavior, or a command's output that you have **not actually verified this session**.

**Stop rule:** Before asserting any concrete fact, ask: *"Did I verify this, or do I
just expect it to be true?"*
- If unverified and verifiable → verify it (read/run/search) before asserting.
- If unverified and not verifiable now → say so explicitly: *"I believe X, but haven't
  confirmed it."* Never dress a guess as a fact.

**Confidence ≠ correctness.** Fluent-sounding fabrication is the most expensive error
because it's the hardest to catch.

---

## The 30-Second Self-Check (run when stuck or before a risky action)

```
[ ] Am I guessing? (>2 unverified tries on the same thing → gather evidence / ask)
[ ] Am I looping? (new error twice in a row → revert, change approach)
[ ] Am I still solving the user's actual problem? (restate goal in one line)
[ ] Do I actually know my current state? (DONE / OPEN / ASSUMING)
[ ] Is anything I'm about to claim unverified? (verify or label it)
```

If any box is checked, address it **before** writing more code.

---

## Hard Rules

- **2 strikes = stop guessing.** Two unverified attempts is the ceiling, not three.
- **A new error twice = the approach is wrong.** Revert, don't patch.
- **Verify before you assert, or label it as unverified.** No fabrication, ever.
- **The goal outranks the sub-problem.** Drift is failure even when each step "works."
- **A written ledger beats memory** on any session longer than a handful of steps.
- Asking a sharp question or reverting is **not** failure — it is the senior move.
  Silently grinding deeper into a wrong approach is the failure.

---

## Interaction With Other Skills

This is an **always-on discipline** that layers on top of whatever primary skill is
active (see Skill Arbitration in GEMINI.md). It does not replace `phase-gated-debugging`
(which structures *how* to debug) — it is the meta-check that tells you *when* your
debugging has gone off the rails and you need to stop, revert, or escalate.
