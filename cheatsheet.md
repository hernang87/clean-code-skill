# Clean Code — Formatting & Smell Codes

Core principles live in SKILL.md; this file holds only what it does not.

## Formatting
- Read top-down (newspaper). Blank lines separate concepts; related code stays close.
- Declare variables near use. **Adopt one team style; automate it.**

## Concurrency
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
