# Patterns & Techniques — Clean Code

## Express Intent in Code, Not Comments
**When**: about to write an explanatory comment.
**How**: extract the condition/expression into a well-named function or variable (`if (employee.isEligibleForFullBenefits())`).
**Trade-offs**: more named units; pays off because names don't rot the way comments do.

## Extract Till You Drop (Small Functions)
**When**: a function does more than one thing or mixes abstraction levels.
**How**: extract sub-steps into functions named for *what* they do until each does one thing — stop when every step sits one level below the name and any further extraction would only restate the code; apply the Stepdown Rule so the file reads top-down.
**Trade-offs**: many small functions; navigation relies on good names + IDE.

## Replace Switch with Polymorphism (Abstract Factory)
**When**: a switch/if-else dispatches on a type code and tends to recur.
**How**: bury one switch in a factory that returns polymorphic subclasses; new types touch only the factory switch; every dispatching function stays closed (OCP). Use where new *types* are likely; where new *functions* are likely, data structures + functions fit better.
**Trade-offs**: more classes; eliminates repeated, error-prone branching (G23).

## Wrap Third-Party APIs
**When**: a foreign type (e.g. `Map`) or library would otherwise spread through the codebase.
**How**: keep it inside one class (or close family) exposing only the operations you need; don't pass it through public APIs; convert its exceptions to yours (this also collapses repeated catch blocks). Not every use needs a wrapper.
**Trade-offs**: a thin wrapper layer; gains testability and one-place migration.

## Learning Tests
**When**: adopting or upgrading a third-party library.
**How**: write tests exercising the API as you intend to use it; keep them as regression guards against library changes.
**Trade-offs**: upfront test code you'd informally write anyway; catches breaking upgrades for free.

## Code to the Interface You Wish You Had
**When**: depending on code that doesn't exist yet (another team's API).
**How**: define your ideal interface, implement against it, adapt to reality later with an Adapter; test against a fake at the seam.
**Trade-offs**: an adapter to write later; keeps your design clean and testable now.

## Exceptions over Return Codes; Special Case Object
**When**: error handling clutters the happy path, or you'd return/check null.
**How**: throw unchecked exceptions whose message names the operation and the failure type. Define exception classes by how callers catch them — often one type carrying data is enough. For normal "nothing" cases return a Special Case object or empty collection; don't return or pass null (except to APIs that expect it). If error handling obscures the logic, restructure it.
**Trade-offs**: must define exception/special-case types; logic stays clean and NPE-free.

## Separate Construction from Use (DI / Separation of Main)
**When**: objects build their own dependencies (hard-wired, global dependencies; lazy-init that couples construction to use).
**How**: move construction to `main`/factories/DI container; pass fully-built collaborators in; depend on abstractions (DIP). The concern is hard-wired, global dependencies, not every `new`.
**Trade-offs**: a wiring layer; yields testable, flexible, swappable components.

## Grow the System Incrementally
**When**: choosing architecture, frameworks, or standards.
**How**: keep domain code free of framework entanglement; adopt standards and frameworks only for demonstrated value; do the simplest thing that can possibly work; no big design up front; defer decisions to the last possible moment.
**Trade-offs**: some later restructuring; avoids paying for guesses that turn out wrong.

## Four Rules of Simple Design (drive emergent design)
**When**: continuously, as you add features.
**How**: ensure (1) all tests pass, (2) no duplication, (3) intent is expressed, (4) fewest classes/methods — in that priority.
**Trade-offs**: requires test discipline; design emerges instead of being over-planned.

## Successive Refinement under Tests
**When**: always — a rough first draft is normal; also whenever code starts sliding toward a mess.
**How**: get trustworthy tests first (measure coverage, add what's missing); fix behavior before polishing structure; refactor in tiny steps, restoring green before the next change; check every caller before deleting, moving, or inlining; never a big-bang rewrite. Decisions may reverse — judge by the end state. Without trustworthy tests, leave the smell and report it.
**Trade-offs**: short-term feature pause; avoids the unrecoverable mess.

## Concurrency Defenses
**When**: introducing threads.
**How**: get the non-concurrent code working first; isolate concurrency code (SRP); limit & protect shared mutable data; keep synchronized sections small; prefer copies/thread-confinement; never dismiss an intermittent failure as a one-off; use `java.util.concurrent`; break one deadlock condition by lock-ordering.
**Trade-offs**: added structure/overhead; intermittent bugs become tractable.

## Build a Domain-Specific Testing Language
**When**: tests are verbose and hard to read.
**How**: extract helpers (`assertResponseIs(...)`, builders) so tests read at the domain level; one concept per test.
**Trade-offs**: a small test API to maintain; far more readable, durable tests.
