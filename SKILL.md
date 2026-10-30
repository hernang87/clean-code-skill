---
name: martin-clean-code
description: "Robert C. Martin's Clean Code principles and smell catalog (C/E/F/G/J/N/T, G1–G36). Use when writing, reviewing, or refactoring code for naming, functions, comments, error handling, SRP/OCP/DIP, TDD/F.I.R.S.T., or a smell code."
allowed-tools:
  - Read
  - Grep
argument-hint: [topic, framework name, or smell code (e.g. G14)]
---

# Clean Code Principles — Quick Reference

> **Disclaimer** — Independent restatement of the principles in Robert C. Martin's *Clean Code* (Prentice Hall, 2008); it does not reproduce the book's text, examples, or structure, and is not a substitute for it. Frameworks and terminology are the IP of the author and publisher.

Smell codes and formatting → [cheatsheet.md](cheatsheet.md) · terms → [glossary.md](glossary.md) · techniques → [patterns.md](patterns.md). Read them only when the question needs them.

## Core Frameworks & Mental Models

**The prime directive.** Clean code does one thing well and *reads like well-written prose*; code is read far more often than it is written. The only way to go fast is to keep the code clean. Apply the **Boy Scout Rule**: leave the code you touch a little cleaner than you found it — small improvements, under test protection. Beware **LeBlanc's Law**: *Later equals never.*

**Functions.** Small, then smaller; blocks inside `if`/`while` are ideally one call; nesting one or two levels deep. **Do one thing**, at **one level of abstraction**, and read top-down (**Stepdown Rule**). *Test for "one thing":* every step sits one level below the function's name, and you cannot extract another function whose name is more than a restatement of the code. Arguments: fewer is better — two already costs, three needs a reason; group related arguments into an object. **No flag arguments** (split into two functions), **no output arguments**. **No side effects** — a hidden state change makes the name lie and creates temporal coupling. Multiple `return`s are fine in small functions. **Command Query Separation**: do something or answer something, never both. Error handling is one thing: `try` comes first, nothing follows the `catch`. Prefer **exceptions over return codes**. Eliminate duplication (**DRY** — duplication *may be* the root of all evil in software).

**Naming.** Use intention-revealing, pronounceable, searchable names; avoid disinformation, encodings (no Hungarian/`m_`/`I`-prefix), noise words (`Data`, `Info`, `Manager`, `Processor`), number series, and puns (one word reused for two concepts). Classes are nouns, methods are verbs. **One word per concept.** Name length scales with scope. Add context through enclosing types or modules, not prefixes. Rename freely — finding a better name often improves the design.

**Comments.** Comments are a necessary evil: prefer expressing intent in code, because **comments lie** as code drifts. Some earn their place — legal notices, informative context, the *why* behind a decision, clarification of code you can't change, warnings of consequences, TODOs (never an excuse to leave bad code; clean them up regularly), amplification, and docs for public APIs. Remove noise, redundant, journal, mandated (doc block on every function), non-public API docs, misleading comments, and comments about code far away. **Delete commented-out code** — version control remembers.

**Objects vs. Data (anti-symmetry).** Objects hide data and expose behavior → easy to add new *types*. Data structures expose data → easy to add new *functions*. Choose per part of the system; "everything is an object" is a myth. Don't build **hybrids**. The **Law of Demeter** applies to *objects*: don't navigate through their internals (train wrecks). Reaching through plain data structures is not a violation. **Tell, don't ask.**

**Error handling.** Use **unchecked exceptions** with context (the operation and the failure type). Define exception classes by how callers catch them — often one type carrying data is enough. **Wrap third-party APIs** to collapse repeated catch blocks. Don't return null; don't pass null — use a **Special Case object**, an empty collection, or an exception (APIs that expect null are the exception). Error handling that obscures the logic is wrong.

**Boundaries.** Keep foreign types (and generic containers like maps) inside one class or close family; don't pass them through public APIs. Write **learning tests** to pin third-party behavior. For not-yet-built code, **code to the interface you wish you had** + Adapter, with a fake at the seam.

**Tests are first-class code.** **Three Laws of TDD**: (1) no production code without a failing test, (2) write only enough test to fail, (3) write only enough code to pass. Clean tests are **F.I.R.S.T.** — Fast, Independent, Repeatable, Self-Validating, Timely. One concept per test. Test code may be less efficient than production code, never less clean. Dirty tests are as bad as, if not worse than, no tests: they rot and take your freedom to change with them.

**Classes.** Keep them small — count **responsibilities**, not lines; if you can't describe the class in about 25 words without "and/or/but", or its name is vague, it has too many. **SRP**: one reason to change. Keep **cohesion** high (maximal cohesion is neither advisable nor possible; falling cohesion = a class wants to split). Organize for change: **OCP** and **DIP** — but restructure when change actually arrives; a logically complete class can stay as it is.

**Systems.** **Separate construction from use** — move wiring to `main`/factories/**Dependency Injection**; the concern is hard-wired, global dependencies, not every `new`. Keep domain code free of framework entanglement; adopt standards and frameworks only for demonstrated value. Grow architecture incrementally; do the simplest thing that can possibly work; no big design up front; defer decisions to the last possible moment.

**Emergent design — the Four Rules of Simple Design** (priority order): (1) Runs all the tests, (2) Contains no duplication, (3) Expresses programmer intent, (4) Minimizes classes/methods. Tests first; then refactor (rules 2–4) under green. Avoid dogma (an interface per class, splitting for its own sake).

**Concurrency.** Decouples *what* from *when* — powerful but hard. Isolate concurrency code (SRP); limit and protect shared mutable data; keep synchronized sections small; prefer copies and thread-confinement; get non-concurrent code working first. Never dismiss an intermittent failure as a one-off — treat it as a candidate threading bug.

**Refactoring.** A rough first draft cleaned up afterwards is the normal workflow. Refactor only under tests you trust — measure coverage, add missing tests first; fix behavior before polishing structure. Take many tiny steps; never a big-bang rewrite. Restore green before the next change. Check every caller before deleting, moving, or inlining. Decisions may reverse — judge by the end state.

**Definition of done.** It **works (tested)** *and* it is **clean** — cleaner than you found it, and honest about what is left. Working alone is not done.

---

## Apply When Writing or Reviewing

1. Names reveal intent; one word per concept; length matches scope (N1–N7).
2. Each function does one thing at one level of abstraction; few args, no flag args, no hidden side effects (F1–F3, G30, G34, N7).
3. No duplication (G5); nothing at the wrong level of abstraction (G6); no magic numbers or unexplained literals (G25).
4. Boundaries handled explicitly (G3); safeties not switched off (G4); errors carry context, never swallowed.
5. Comments add what the code can't say (C1–C5).

For each violation give **smell code · location · fix**, plus the trade-off when there is one (e.g. `G25 · utils.ts:42 · extract SECONDS_PER_DAY`).

## Precedence

**Follow the book over this skill's custom notes.** Everything here that is not a restatement of the book (the checklist, the TypeScript Notes) is an aid. If a note disagrees with the book, follow the book and treat the note as a bug. Apply the book as it does: with judgment, not mechanically.

Project or harness rules (for example, limits on what a task can change) bound *what you are allowed to change*. Team-agreed conventions are part of clean code — follow them. When a rule, or missing tests, stops you from applying a principle, leave the smell and say which one and why.

## TypeScript Notes

Translations of the book's principles into TypeScript idiom. They do not replace them.

- **Null** — don't return or pass `null`. A typed optional (`T | undefined`) under `strictNullChecks` is not the problem the book describes, but prefer an empty collection or Special Case object where callers would otherwise repeat the same check.
- **Exceptions over return codes** — all TS exceptions are unchecked, which is what the book recommends. Throw errors that carry context; don't return error codes. For a *normal* "nothing" case, use a Special Case rather than throwing.
- **Switch vs polymorphism (G23)** — a discriminated union with functions over it is the book's data-structure side: fine where new *functions* are likely. Use polymorphism where new *types* are likely; keep any type-dispatch `switch` in one place.
- **Java rules (J2, J3)** — J2: don't share constants through inheritance. J3: prefer enums or literal unions over loose constants. J1 (wildcard imports) does not carry over to TS.
- **Encodings** — don't prefix interfaces with `I` and don't put types in names; the type system already carries that information.
