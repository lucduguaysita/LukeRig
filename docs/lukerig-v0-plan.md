# LukeRig v0: Experiment Plan

LukeRig tests Luke's ideas in real life, one claim at a time, cheapest and most fundamental first. Each experiment has pass and fail criteria written before any code exists. A failed experiment is a result: it says which idea broke, and the next experiment waits until the current one passes or the claim is changed.

## Ground rules

The rules exist so that results stay numbers, not opinions.

- **Criteria are frozen before building.** The numbers below are proposals. Set the final ones before starting an experiment, then never change them for that run. A new number means a new experiment.
- **One experiment at a time, in order.** Experiment 1 is the foundation. If it fails, nothing built on it matters.
- **Pass, fail, or one rerun.** A result between the pass and fail lines allows one rerun with one written change. After that it passes or it fails.
- **v0 scope.** Single user, local machine, four verbs: COMMIT, RESOLVE, REVERT and AUDIT. An ordinary repository serves as the hidden store underneath; no user ever touches it.
- **Everything is logged.** Every run records cost, time, outcome and any human intervention.
- **Real code, not toys.** Experiments run on a real codebase you know well, so you can judge results without trusting the AI's own account.

## Experiment 1: intents land one at a time, no merges

**Claim:** an AI can re-implement a queued intent on top of the one that just landed, reliably and cheaply, so a textual merge is never needed.

**Setup:** write at least 20 pairs of intents that touch the same code, each with acceptance criteria. Draft both intents of a pair against the same starting point, land A, then let RESOLVE re-implement B on top of A. Include pairs where A restructures the code B targets, such as moving it into a new method; that is the hard case and the one that matters.

**Correct means:** B's acceptance tests pass, A's tests still pass, and B still does what its intent asked. You judge that last point, not the implementer.

| Measure | Pass | Fail |
| --- | --- | --- |
| Pairs landing correctly on the first re-implementation | 90% or more | Under 75% |
| Silent intent changes (B lands but no longer does what it asked) | None | Any |
| Cost of re-implementing B on top of A, versus implementing B alone | Under 2x (median) | Over 4x (median) |
| Time to land B | Under 10 minutes (median) | Over 30 minutes (median) |
| Pairs needing human intervention | Under 1 in 10 | Over 1 in 4 |

## Experiment 2: hidden tests from a second vendor

**Claim:** hold-out tests written by a different vendor's model, from the acceptance criteria alone, catch defects the implementer's own tests miss.

**Setup:** for at least 20 intents, the implementer (vendor A) writes the code and its own tests. The auditor (vendor B) writes hold-out tests from the acceptance criteria, never seeing the code. Then run mutation testing: a tool injects small deliberate bugs into the code and counts which test suite catches each one. Stryker covers .NET and JavaScript, for example.

| Measure | Pass | Fail |
| --- | --- | --- |
| Extra mutants caught by both suites together, versus the implementer's suite alone | 10 points or more | Under 5 points |
| False failures: hold-out tests failing on correct code because the auditor misread the criteria | Under 10% of hold-out tests | Over 20% |
| Cost of writing hold-out tests, versus implementing | Under 0.5x | Over 1x |

If the implementer's own tests already catch nearly every mutant, the experiment cannot show a gain. Record that as "not needed on this codebase", not as a pass, and rerun on harder intents.

## Experiment 3: no orphan intents and proportionality

**Claim:** the gatekeeper's proportionality check stops over-engineering without blocking reasonable work.

Refusing a COMMIT that names no goal is a plain rule, not an experiment; v0 simply enforces it. What needs testing is the judgment call: does a change do more than its goal requires?

**Setup:** prepare two sets of intents, each tied to a goal. The proportionate set does what its goal asks. The over-scoped set deliberately overreaches: a generic plugin framework for a one-off need, a new library for a two-line problem, an abstraction layer nobody asked for. Also review the implementer's output on ordinary intents for layers or dependencies it added on its own.

| Measure | Pass | Fail |
| --- | --- | --- |
| Over-scoped intents held by the gatekeeper | 80% or more | Under 60% |
| Proportionate intents wrongly held | Under 10% | Over 20% |
| Unrequested additions by the implementer that the gatekeeper catches | 80% or more | Under 60% |

## If an experiment fails

A failure redirects the design; it does not end the project.

| Experiment | A failure means | What changes |
| --- | --- | --- |
| 1. No merges | Re-implementing on top of landed work is too costly or too unreliable | Smaller intents or a different landing strategy, before anything else is built |
| 2. Hidden tests | A second vendor's tests add cost without catching more | Keep mutation testing as the check; drop or rethink hold-out test writing |
| 3. Proportionality | The AI cannot judge over-engineering reliably | Proportionality becomes a human review item instead of an automatic hold |

## After v0

Once all three pass, LukeRig's own next features are built as intents through LukeRig, like a compiler that compiles itself. Only then come BRANCH, RELEASE, DEPLOY and one-way code, each with its own experiment and its own frozen criteria.
