# Erdős #461 progress update — 461A / 461B route audit

Date: 2026-07-01

This note extracts the currently useful proof-route changes from the uploaded Aristotle archives. It is not a claim that Erdős #461 is solved. It is a proof-state / merge-state document intended to make the repository auditable.

## Which uploaded archive to use for which line

Do **not** merge either archive wholesale without checking file age. Use them by role:

- `3ed908db-d5a7-43b9-8d67-fc5a3bb45298-aristotle (54).tar.gz` contains the current **461A relevant-side shadow route** in `RequestProject/RelevantExcessCharging.lean`.
- `85e8133a-e7ca-4bcc-babe-abcfda08451b-aristotle (1).tar.gz` contains the current **461B non-relevant fourth budget / four-prime wheel route** in `RequestProject/NonRelevantFourthBudget.lean`, plus diagnostics.

Merge per file, not by overwriting the whole repo.

---

# 1. Current global theorem map

The proof now has two main remaining mathematical locks.

## Lock B — 461B / non-relevant fourth budget

Current theorem:

```lean
theorem aggregatePrimeRoughFourthExcess_bound
    (n t : ℕ) (ht : 3 ≤ t) :
    2 * UnitFourthExcess n t
      + 2 * roughPrimeQuotientExcessSum n t ≤ t
```

If this is proved axiom-clean, the already-routed downstream chain should close:

```lean
aggregatePrimeRoughFourthExcess_bound
⇒ primeRoughFourthExcess_half_window
⇒ two_mul_NonRelevantFourthExcess_le_window
⇒ nonRelevantFourthExcess_half_window
```

Current proposed closure route: four-prime square-wheel sieve modulo

```text
coreMod = 2^2 * 3^2 * 5^2 * 7^2 = 44100.
```

## Lock A — 461A / relevant-side charging

Current theorem:

```lean
theorem relevantSmoothExcess_le_two_mul_relevantSmoothSingleton
    (n t : ℕ) :
    RelevantSmoothExcess n t ≤ 2 * RelevantSmoothSingleton n t
```

This has been reduced to:

```lean
theorem strictGrowthGapClosure_freeSlot_boundary_capacity
    (n t : ℕ)
    (C : Finset (ℕ × ℕ))
    (hclosed : GapSideClosed n t C) :
    C.card ≤
      (GapClosureLabelSet n t C).card
        + 2 * (GapSideSingletonBoundary n t C).card
```

or equivalently:

```lean
ClosedGapHallDefect n t C ≤ 0
```

for every `GapSideClosed C`.

Current sharpened remaining local wall:

```lean
theorem maxLayerShadow_lostFrontier_payment
    (n t : ℕ) (C : Finset (ℕ × ℕ))
    (hclosed : GapSideClosed n t C) :
    (MaxLayerShadow n t C).card
      ≤ (ShadowLostLabels n t C).card
          + 2 * (ShadowLostBoundary n t C).card
```

If this theorem is proved axiom-clean, the current file derives:

```lean
maxLayerShadow_lostFrontier_payment
⇒ maxLayerShadow_deltaCharge
⇒ positive_defect_has_smaller_positive_defect
⇒ closedGapHallDefect_nonpositive
⇒ strictGrowthGapClosure_freeSlot_boundary_capacity
⇒ relevantSmoothExcess_le_two_mul_relevantSmoothSingleton
```

---

# 2. New 461A route: maximal-layer shadow / lost-frontier payment

Source file: `RequestProject/RelevantExcessCharging.lean` from archive `(54)`.

## 2.1 Already formalized structure

The current route no longer deletes only the maximal layer. It deletes the whole dependency shadow, because lower tokens may point upward into the maximal layer.

Important definitions:

```lean
noncomputable def MaxGapLabelLayer (n t : ℕ) (C : Finset (ℕ × ℕ)) : Finset (ℕ × ℕ)

def CTokenEdge (n t : ℕ) (C : Finset (ℕ × ℕ)) : (ℕ × ℕ) → (ℕ × ℕ) → Prop

noncomputable def MaxLayerShadow (n t : ℕ) (C : Finset (ℕ × ℕ)) : Finset (ℕ × ℕ)

noncomputable def LowerCoreAfterMax (n t : ℕ) (C : Finset (ℕ × ℕ)) : Finset (ℕ × ℕ)

def ClosedGapDeltaCanPay (n t : ℕ) (C C' : Finset (ℕ × ℕ)) : Prop
```

Key proved scaffold in archive `(54)`:

```lean
theorem closedGapHallDefect_le_of_deltaCanPay

theorem lowerCoreAfterMax_gapSideClosed

theorem gapTokenSideLabel_repeated_or_singleton

theorem shadowNewLabels_empty

theorem shadowNewBoundary_empty

theorem maxLayerShadow_deltaCharge
    -- conditional on maxLayerShadow_lostFrontier_payment

theorem maximalLayer_discharge_or_smaller_defect_from_shadow_deltaCharge

theorem closedGapHallDefect_nonpositive
    -- conditional through the lost-frontier payment sorry

theorem strictGrowthGapClosure_freeSlot_boundary_capacity
    -- conditional through the lost-frontier payment sorry

theorem relevantSmoothExcess_le_two_mul_relevantSmoothSingleton
    -- conditional through the lost-frontier payment sorry
```

## 2.2 Remaining 461A sorries in archive `(54)`

`grep -n "sorry" RequestProject/RelevantExcessCharging.lean` shows three actual theorem sorries in the new shadow route:

```lean
theorem maxGapLabelLayer_sideLabel_singleton

theorem maxGapLabelLayer_sideLabel_mem_boundary

theorem maxLayerShadow_lostFrontier_payment
```

The first two are expected to be Lean-engineering / side-label routing. The genuine wall is:

```lean
maxLayerShadow_lostFrontier_payment
```

## 2.3 Correct next target for 461A

Target the side-label lemmas first, then the payment theorem:

```lean
gapTokenSideLabel_repeated_or_singleton  -- already present/proved in archive (54)
→ maxGapLabelLayer_sideLabel_singleton
→ maxGapLabelLayer_sideLabel_mem_boundary
→ maxLayerShadow_lostFrontier_payment
→ relevantSmoothExcess_le_two_mul_relevantSmoothSingleton
```

If `maxLayerShadow_lostFrontier_payment` fails, demand diagnostics:

```text
n,t,C
MaxGapLabelLayer
MaxLayerShadow
LowerCoreAfterMax
ShadowLostLabels
ShadowLostBoundary
shadow size - lost label count - 2*lost boundary count
side label of every shadow token
strict-growth path to maximal layer
```

---

# 3. New 461B route: four-prime square-wheel closure

Source file: `RequestProject/NonRelevantFourthBudget.lean` from archive `(85)`.

## 3.1 Already established non-wheel 461B infrastructure

The file already contains the rough quotient bridge and structural split:

```lean
theorem mem_smoothFiber_prime_iff_roughQuotient

theorem smoothFiber_prime_card_le_roughQuotientFiber_card

def supportedPrimeSet

def roughPrimeQuotientFiber

def roughPrimeQuotientExcessSum

def roughUnitFiber

def roughUnitExcess

theorem smoothFiber_prime_excess_le_roughPrimeQuotientExcess

theorem roughPrimeQuotientFiber_card_le_three_of_not_supported

theorem PrimeFourthExcess_le_roughPrimeQuotientExcessSum

theorem UnitFourthExcess_le_roughUnitExcess
```

The theorem `primeRoughFourthExcess_half_window` is already routed through `aggregatePrimeRoughFourthExcess_bound`.

## 3.2 New four-prime wheel definitions

The current wheel route defines:

```lean
def coreMod : ℕ := 44100

inductive CoreCat
  | none | two | three | five | seven

def catIdx (m : ℕ) : ℕ

def coreCat? (m : ℕ) : Option CoreCat

def categoryOfPrime (p : ℕ) : CoreCat

def coreCatCount (n t : ℕ) (c : CoreCat) : ℕ

def coreWheelCategoryExcess (n t : ℕ) : ℕ

def coreAllowedCount (n t : ℕ) : ℕ
```

Interpretation:

- `CoreCat.none`: no divisibility by `2,3,5,7`.
- `CoreCat.two`: divisible by `2` exactly once and not by `3,5,7`.
- `CoreCat.three`: divisible by `3` exactly once and not by `2,5,7`.
- `CoreCat.five`: divisible by `5` exactly once and not by `2,3,7`.
- `CoreCat.seven`: divisible by `7` exactly once and not by `2,3,5`.

The wheel controls the actual window integers `m = p*q`, not just the quotient `q`.

## 3.3 Finite certificates already sketched / native-decide-backed

Archive `(85)` contains the finite-check backbone:

```lean
theorem Fcount_period : Fcount coreMod = 21936 := by native_decide

theorem mmFold_wtList : mmFold wtList = (0, -14827, 12999) := by native_decide

theorem two_mul_Fcount_period_lt : 2 * Fcount coreMod < coreMod

theorem smallSweepCheck : ... := by native_decide

theorem bigSweepCheck : ... := by native_decide
```

The scratch checks reported:

```text
allowed residues per period = 21936
2 * 21936 < 44100
prefix-weight min/max = (-14827, 12999)
small-window sweep for 8 ≤ t < 168 passes
large-window category lower bounds at t = 168 pass
```

These are strong evidence for the wheel theorem, but not full closure until all bridge lemmas are proved and the axiom audit is clean.

## 3.4 Remaining 461B sorries in archive `(85)`

The current wheel skeleton still has many sorries. They fall into categories:

### Basic wheel/category lemmas

```lean
coreCat?_isSome_iff
coreCat?_eq_some_iff
catIdx_mod
idxCountUpto_eq_filter_card
idxCountUpto_mono
idxCountUpto_mod
coreCatCount_eq_idxCountUpto
coreWheelCategoryExcess_eq_wheelExcessAt
wheelExcessAt_mod
```

### Sweep / finite-certificate soundness

```lean
sweepResidue_count1 ... sweepResidue_count5
sweepResidue_ok
```

### Prefix-weight / discrepancy lemmas

```lean
mmFold_take_sum_mem
Gval_eq_map_sum
Gval_bounds_le
Gval_eq
Fcount_mono
Fcount_block
Gval_periodic
Gval_mod
Gval_bounds
coreAllowedCount_eq
coreAllowed_discrepancy
```

### Partition and half-window lemmas

```lean
coreCat_partition
coreCatCount_ge_four_of_large
coreWheelCategoryExcess_half_window_large
coreWheelCategoryExcess_half_window_small
```

### Rough image coverage / disjointness / excess aggregation

```lean
roughUnitFiber_subset_core_none
supportedPrime_roughQuotient_coreCat
supportedPrime_roughQuotient_images_disjoint
roughUnit_disjoint_supportedPrime_images
sum_excess_le_card_excess
aggregate_rough_excess_le_coreWheelCategoryExcess
supportedPrimeSet_eq_empty_of_lt_thirteen
aggregatePrimeRoughFourthExcess_bound_t_lt_8
```

The final theorem is already reduced to:

```lean
aggregate_rough_excess_le_coreWheelCategoryExcess
+
coreWheelCategoryExcess_half_window
⇒ aggregatePrimeRoughFourthExcess_bound
```

## 3.5 Correct next target for 461B

Do not switch back to Mertens/Selberg unless the wheel route fails. The right sequence is:

```lean
coreCat?_isSome_iff / coreCat?_eq_some_iff / catIdx_mod
→ idxCountUpto and sweep soundness
→ coreWheelCategoryExcess_half_window
→ roughUnit / supportedPrime image coverage
→ image disjointness
→ aggregate_rough_excess_le_coreWheelCategoryExcess
→ aggregatePrimeRoughFourthExcess_bound
```

The highest-risk bridge is:

```lean
aggregate_rough_excess_le_coreWheelCategoryExcess
```

because it must preserve the `-3` fourth-excess truncation while grouping many unit/prime fibres into five categories.

---

# 4. Repository hygiene recommendations

## 4.1 Merge strategy

Recommended files to take from uploaded archives:

```text
From archive (54):
  RequestProject/RelevantExcessCharging.lean

From archive (85):
  RequestProject/NonRelevantFourthBudget.lean
  RequestProject/NonRelevantFourthBudgetDiagnostics.lean
  NON_RELEVANT_FOURTH_BUDGET_461B.md  -- update or replace with wheel-route note
```

Do not overwrite the newer `RelevantExcessCharging.lean` from `(54)` with the older-looking version in `(85)`.

## 4.2 Add/update repository documents

Suggested docs:

```text
ERDOS461_CURRENT_STATUS.md
461A_RELEVANT_SHADOW_ROUTE.md
461B_FOUR_PRIME_WHEEL_ROUTE.md
AUDIT_CHECKLIST.md
```

## 4.3 Must-run audit commands

After each serious merge:

```bash
lake build

grep -R "sorry" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "implemented_by" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "globalSmoothDeficit" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean

grep -R "dilatedSpan\|from_dilatedSpan\|collapse\|top-band" RequestProject/RelevantExcessCharging.lean RequestProject/NonRelevantFourthBudget.lean
```

Then audit core theorems:

```lean
#print axioms aggregatePrimeRoughFourthExcess_bound
#print axioms primeRoughFourthExcess_half_window
#print axioms two_mul_NonRelevantFourthExcess_le_window
#print axioms nonRelevantFourthExcess_half_window

#print axioms maxLayerShadow_lostFrontier_payment
#print axioms closedGapHallDefect_nonpositive
#print axioms strictGrowthGapClosure_freeSlot_boundary_capacity
#print axioms relevantSmoothExcess_le_two_mul_relevantSmoothSingleton

#print axioms erdos461_from_relevantTwoCongestion_and_nonRelevantFourthHalf
#print axioms erdos461_lower_bound_of_relevantTwoCongestion
```

Interpretation:

- If `aggregatePrimeRoughFourthExcess_bound` has no `sorryAx`, 461B is essentially closed.
- If `relevantSmoothExcess_le_two_mul_relevantSmoothSingleton` has no `sorryAx`, 461A is essentially closed.
- If both are clean and the final algebra theorem is clean, full #461 is in serious closure territory.

---

# 5. Public / forum status wording

Safe wording before full closure:

> I have formalized substantial partial progress on Erdős #461. The non-relevant fourth-excess route is reduced to a four-prime square-wheel finite residue theorem modulo 44100; the relevant route is reduced to a maximal-layer shadow / lost-frontier payment theorem for a closed gap Hall defect. The full problem is not claimed solved.

Safe wording after 461B axiom-clean:

> The non-relevant fourth-excess component of the Erdős #461 proof route is now closed axiom-clean in Lean via a finite four-prime square-wheel sieve modulo 44100. The remaining major wall is the relevant-side closed gap Hall/free-slot theorem.

Unsafe wording until both locks are clean:

> Erdős #461 is solved.

---

# 6. Current bottom line

## 461B

Promising and potentially near closure, but archive `(85)` still has many wheel-route sorries. Once these are discharged and `#print axioms aggregatePrimeRoughFourthExcess_bound` shows no `sorryAx`, 461B can be treated as clean.

## 461A

Route is now sharp, but not closure-ready. Archive `(54)` narrows the wall to:

```lean
maxLayerShadow_lostFrontier_payment
```

plus two side-label support lemmas. If `maxLayerShadow_lostFrontier_payment` falls, the relevant theorem should close quickly through the already-built descent chain.

## Full Erdős #461

No third major conceptual wall is currently visible, but final algebra/audit remains necessary after both 461A and 461B close.
