# Erdős #461 repository audit checklist

## Merge sources

- Use `RelevantExcessCharging.lean` from `3ed908db-d5a7-43b9-8d67-fc5a3bb45298-aristotle (54).tar.gz`.
- Use `NonRelevantFourthBudget.lean` and `NonRelevantFourthBudgetDiagnostics.lean` from `85e8133a-e7ca-4bcc-babe-abcfda08451b-aristotle (1).tar.gz`.
- Do not overwrite `RelevantExcessCharging.lean` from archive `(54)` with the version in archive `(85)` unless manually diffed.

## Build

```bash
lake build
```

## Basic hygiene grep

```bash
grep -R "sorry" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "implemented_by" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "axiom" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "globalSmoothDeficit" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "dilatedSpan\|from_dilatedSpan\|collapse\|top-band" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean
```

## 461B theorem audit

```lean
#print axioms aggregatePrimeRoughFourthExcess_bound
#print axioms primeRoughFourthExcess_half_window
#print axioms two_mul_NonRelevantFourthExcess_le_window
#print axioms nonRelevantFourthExcess_half_window
```

Expected before closure: `sorryAx` only through `aggregatePrimeRoughFourthExcess_bound`.

Expected after closure: no `sorryAx`, no new axioms, no `implemented_by`.

## 461A theorem audit

```lean
#print axioms maxLayerShadow_lostFrontier_payment
#print axioms maxLayerShadow_deltaCharge
#print axioms positive_defect_has_smaller_positive_defect
#print axioms closedGapHallDefect_nonpositive
#print axioms strictGrowthGapClosure_freeSlot_boundary_capacity
#print axioms relevantSmoothExcess_le_two_mul_relevantSmoothSingleton
```

Expected before closure: `sorryAx` through `maxLayerShadow_lostFrontier_payment` and side-label support lemmas if still open.

Expected after closure: no `sorryAx`, no new axioms, no `implemented_by`.

## Final algebra audit

```lean
#print axioms erdos461_from_relevantTwoCongestion_and_nonRelevantFourthHalf
#print axioms erdos461_lower_bound_of_relevantTwoCongestion
```

Only claim full #461 once relevant side, non-relevant side, and final algebra are all axiom-clean.
