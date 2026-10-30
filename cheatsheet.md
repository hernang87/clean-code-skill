# Clean Code — Cheatsheet

## The One Rule
**Leave it cleaner than you found it** (Boy Scout Rule). The only way to go fast is to keep the code clean.

## Naming (N1–N7)
- Intention-revealing · no disinformation · meaningful distinctions · pronounceable · searchable.
- Classes = nouns; methods = verbs. One word per concept.
- Name length ∝ scope. No encodings (Hungarian, `m_`, `I`-prefix).

## Functions (F1–F4)
- **Small**, then smaller. **Do one thing.** One level of abstraction.
- Args: 0 > 1 > 2 > 3. No flag args. No output args.
- Command **or** query, not both. Exceptions, not error codes. DRY.

## Comments (C1–C5)
- Prefer expressing intent in code; comments drift. Keep the ones that earn it: legal, *why*, warnings, TODOs, public-API docs.
- **Delete commented-out code.** No journal/noise/redundant comments — VCS remembers.

## Formatting
- Read top-down (newspaper). Blank lines separate concepts; related code stays close.
- Declare variables near use. **Adopt one team style; automate it.**

## Objects vs Data
- Objects: hide data, expose behavior → easy new *types*. Data structures: expose data → easy new *functions*.
- Don't make hybrids. Obey Law of Demeter (no train wrecks). Tell, don't ask.

## Error Handling
- Unchecked exceptions with context. Wrap third-party APIs.
- **Never return null. Never pass null.** Use Special Case objects.

## Boundaries
- Wrap foreign types. Write learning tests. Code to the interface you wish you had + Adapter.

## Tests (T1–T9)
- **Three Laws of TDD**: failing test first → minimal test → minimal code.
- **F.I.R.S.T.**: Fast, Independent, Repeatable, Self-Validating, Timely.
- One concept per test. Keep tests as clean as production code. Use a coverage tool.

## Classes
- Small = few responsibilities. **SRP**: one reason to change. High cohesion.
- **OCP** (extend, don't modify) · **DIP** (depend on abstractions).

## Systems
- Separate construction from use. Inject dependencies. Grow architecture incrementally. Defer decisions to the last responsible moment.

## Emergence — Simple Design, in priority order
1. Runs all the tests  2. No duplication  3. Expresses intent  4. Fewest classes/methods

## Concurrency
- Decouples *what* from *when*. Isolate concurrency code (SRP). Limit & protect shared data; prefer copies.
- Deadlock needs all four: Mutual Exclusion, Lock & Wait, No Preemption, Circular Wait — break one.
- Treat every intermittent failure as a threading bug; jiggle to expose.

## Smell Codes
**C** Comments · **E** Environment · **F** Functions · **G** General (G1–G36) · **J** Java · **N** Names · **T** Tests.
**G5 Duplication** and **G6 Wrong Level of Abstraction** are among the most common and costly.

**Comments** — C1 metadata that belongs in VCS/tracker · C2 obsolete · C3 redundant · C4 poorly written · C5 commented-out code

**Environment** — E1 build takes more than one step · E2 tests take more than one step

**Functions** — F1 too many arguments · F2 output arguments · F3 flag arguments · F4 dead function

**General**
- G1 several languages in one file · G2 obvious behavior missing · G3 wrong at boundaries · G4 safeties switched off
- G5 duplication · G6 wrong level of abstraction · G7 base class knows its derivatives · G8 interface exposes too much
- G9 dead code · G10 declaration far from use · G11 inconsistency · G12 clutter
- G13 artificial coupling · G14 feature envy · G15 selector arguments · G16 obscured intent
- G17 responsibility in the wrong place · G18 static that should be polymorphic · G19 missing explanatory variables · G20 name doesn't say what the function does
- G21 not understanding the algorithm · G22 logical dependency not made physical · G23 if/switch where polymorphism fits · G24 ignoring standard conventions
- G25 magic numbers · G26 imprecision · G27 convention where structure would enforce it · G28 unencapsulated conditionals
- G29 negative conditionals · G30 function does more than one thing · G31 hidden temporal coupling · G32 arbitrary structure
- G33 unencapsulated boundary conditions · G34 function mixes abstraction levels · G35 config buried at low levels · G36 transitive navigation (Demeter)

**Java** — J1 wildcard imports to shorten lists · J2 inheriting constants · J3 constants instead of enums (TS readings in SKILL.md → TypeScript Notes)

**Names** — N1 not descriptive · N2 wrong abstraction level · N3 nonstandard nomenclature · N4 ambiguous · N5 length doesn't match scope · N6 encodings · N7 hides side effects

**Tests** — T1 insufficient tests · T2 no coverage tool · T3 skipping trivial tests · T4 ignored test hides an open question · T5 untested boundaries · T6 not testing hard near bugs · T7 ignoring failure patterns · T8 ignoring coverage patterns · T9 slow tests

## Definition of Done
It **works** (tested) **and** it's **right** (clean). Working alone is not done.
