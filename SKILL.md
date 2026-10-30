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

**The prime directive.** Clean code does one thing well and *reads like well-written prose*. The only way to go fast is to keep the code clean — always. Apply the **Boy Scout Rule**: leave the code cleaner than you found it on every check-in. Beware **LeBlanc's Law**: *Later equals never.*

**Functions.** Small, then smaller. **Do one thing**, at **one level of abstraction**, and read top-down (**Stepdown Rule**). Arguments: 0 > 1 > 2 > 3; **no flag arguments** (they announce the function does >1 thing), **no output arguments**. **No side effects** — a hidden state change makes the name lie and creates temporal coupling. Multiple `return`s are fine in small functions. **Command Query Separation**: do something or answer something, never both. Prefer **exceptions over return codes**; eliminate duplication (**DRY** — "the root of all evil").

**Naming.** Use intention-revealing, pronounceable, searchable names; avoid disinformation and encodings (no Hungarian/`m_`/`I`-prefix). Classes are nouns, methods are verbs. **One word per concept.** Name length scales with scope. A name that needs a comment is a failed name.

**Comments.** Comments are a necessary evil: prefer expressing intent in code, because **comments lie** as code drifts. Some comments earn their place — legal notices, informative context, the *why* behind a decision, clarification of an unchangeable API, warnings of consequences, TODOs, amplification of something easy to miss, and docs for public APIs. Remove noise, redundant, journal, and misleading comments. **Delete commented-out code** — version control remembers.

**Objects vs. Data (anti-symmetry).** Objects hide data and expose behavior → easy to add new *types*. Data structures expose data → easy to add new *functions*. Don't build **hybrids**. Obey the **Law of Demeter** — talk only to immediate friends; no train wrecks. **Tell, don't ask.**

**Error handling.** Use **unchecked exceptions** with meaningful context; **wrap third-party APIs**. **Never return null; never pass null** — use a **Special Case object** or empty collection.

**Boundaries.** Wrap foreign types so a vendor change costs little. Write **learning tests** to pin third-party behavior. For not-yet-built code, **code to the interface you wish you had** + Adapter.

**Tests are first-class code.** **Three Laws of TDD**: (1) no production code without a failing test, (2) write only enough test to fail, (3) write only enough code to pass. Clean tests are **F.I.R.S.T.** — Fast, Independent, Repeatable, Self-Validating, Timely. One concept per test. Dirty tests are worse than none, because they rot and take your freedom-to-change with them.

**Classes.** Keep them small — count **responsibilities**, not lines. **SRP**: one reason to change. Maximize **cohesion** (falling cohesion = a class wants to split). Organize for change: **OCP** (extend, don't modify) and **DIP** (depend on abstractions).

**Systems.** **Separate construction from use** — move wiring to `main`/factories/**Dependency Injection**; don't let objects build their own dependencies. Architectures grow incrementally when concerns stay separated; defer decisions to the last responsible moment.

**Emergent design — the Four Rules of Simple Design** (priority order): (1) Runs all the tests, (2) Contains no duplication, (3) Expresses programmer intent, (4) Minimizes classes/methods. Tests first; then refactor (rules 2–4) under green.

**Concurrency.** Decouples *what* from *when* — powerful but hard. Isolate concurrency code (SRP); limit and protect shared mutable data; prefer copies and thread-confinement; learn `java.util.concurrent`. Treat every intermittent failure as a likely threading bug.

**Definition of done.** It **works (tested)** *and* it's **right (clean)**. Working alone is not done — refine successively (stop and clean the moment code starts to rot).

---

## Apply When Writing or Reviewing

1. Names reveal intent; one word per concept; length matches scope (N1–N7).
2. Each function does one thing at one level of abstraction; ≤3 args, no flag args, no hidden side effects (F1–F4, G30, G34).
3. No duplication (G5); nothing at the wrong level of abstraction (G6); no magic numbers (G25).
4. Boundaries and errors handled explicitly; no ignored failures (G3, G4).
5. Comments explain *why*, not *what* (C1–C5).

Report each violation as: **smell code · location · one-line fix** (e.g. `G25 · utils.ts:42 · extract SECONDS_PER_DAY`).

## Precedence

**The book is the authority.** Everything in this skill that is not a restatement of the
book (the checklist, the TypeScript Notes, any custom guidance) is an aid for applying the
book. If a custom note and the book disagree, follow the book and treat the note as a bug.

Project or harness rules (for example, limits on what a task can change) still bound
*what you are allowed to change*. They do not redefine what clean code is. When such a rule
stops you from applying a principle, say which smell you left in place and why.

## TypeScript Notes

These are translations of the book's principles into TypeScript idiom. They do not replace them.

- **Never return or pass null** — this also applies to `undefined` as a "nothing" value. Return an empty collection or a Special Case object. Use `strictNullChecks` so the compiler catches what is left.
- **Exceptions over return codes** — all TS exceptions are unchecked, which is what the book recommends. Throw errors that carry context, and don't return error codes or sentinel values.
- **Polymorphism over switch (G23)** — allow one `switch` on a type discriminant, kept inside a factory. Every other dispatch uses polymorphism.
- **Java rules (J1–J3), translated** — J1: import only what you use. J2: don't share constants through inheritance. J3: prefer enums or literal unions over loose constants.
- **Encodings (N6)** — don't prefix interfaces with `I` and don't put types in names. The type system already carries that information.
