# Next tasks

This is the current short list of useful tasks.

The public repo is still a proof-search roadmap, not a completed proof.

## Task 1: finish the signed-surplus bookkeeping

Priority theorem:

```lean
theorem signed_surplus_split (n t : ℕ) :
    SignedRelevantSurplus n t + SignedNonRelevantSurplus n t
      = (t : ℤ) - 2 * (smoothDistinct n t : ℤ)
```

This should follow from:

```lean
smoothSigma_eq_relevant_add_nonRelevant
smoothSingleton_eq_relevant_add_nonRelevant
smooth_two_f_sub_t
```

Also close:

```lean
signedRelevantSurplus_nonpos_iff
signed_surplus_bound_implies_linear_f
erdos461_from_signedSurplusLinearBound
```

These are not meant to be the hard combinatorial theorem. They are the formal bridge from signed surplus control to a linear lower bound for `smoothDistinct`.

## Task 2: prove or refute relevant singleton domination

Main structural target:

```lean
theorem relevantSmoothExcess_le_relevantSmoothSingleton
    (n t : ℕ) :
    RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t
```

Equivalent signed form:

```lean
SignedRelevantSurplus n t ≤ 0
```

Current empirical status: large native searches have not found a positive relevant surplus, but this is not a proof.  The Python proxy for relevance must be checked against the exact Lean definition of `RelevantHighRankValue`.

If false, produce explicit native-checked data and repair to a signed error term small enough to imply a fixed surplus slack.

## Task 3: find the first nontrivial signed bound

Main breakthrough target:

```lean
def SignedSurplusLinearBound (θ : ℚ) : Prop :=
  ∀ n t : ℕ,
    (SignedRelevantSurplus n t : ℚ)
      + (SignedNonRelevantSurplus n t : ℚ) ≤ θ * t
```

The goal is to prove this for some explicit `θ < 1`.

A componentwise route is acceptable only if it yields:

```lean
SignedRelevantSurplus n t ≤ θrel * t
SignedNonRelevantSurplus n t ≤ θnon * t
θrel + θnon < 1
```

This is the first theorem that would strongly indicate the hard wall has been crossed.

## Task 4: prove fallback non-relevant half-window bound

Fallback theorem:

```lean
theorem nonRelevant_deficit_half_window
    (n t : ℕ) :
    2 * NonRelevantSmoothDeficit n t ≤ t
```

Together with relevant singleton domination, this gives:

```lean
theorem erdos461_lower_bound_quarter :
    ∀ n t, t ≤ 4 * smoothDistinct n t
```

This would already prove Erdős #461.

## Task 5: stronger non-relevant sublinear bound

Ambitious theorem:

```lean
theorem nonRelevantSmoothDeficit_sublinear_from_roughInput :
    NonRelevantSmoothDeficit n t = o(t)
```

Likely route:

```lean
roughNumbersInShortInterval_bound_corrected_logLength
```

with

\[
R_t(X,L)\ll L/\log L.
\]

Then prove:

\[
\operatorname{NonRelevantSmoothExcess}(n,t)
\ll t\frac{\log\log t}{\log t}.
\]

## Task 6: remove elementary SmoothDeficit sorries

The following should be pure algebra / finite-sum bookkeeping:

```lean
smoothDeficit_bound_implies_linear_f
smoothDeficit_eps_asymptotic
smoothSingleton_eq_relevant_add_nonRelevant
smoothDeficit_le_relevantDeficit_plus_nonRelevantDeficit
erdos461_from_deficit_bounds
erdos461_from_relevantSingletonDomination_and_roughInput
```

These should not remain as deep blockers.

## Task 7: public proof path cleanup

Do not upload the whole experimental workspace to this public repo.

Upload only final-route Lean files once their import closure is clean. Until then, keep code in private/archive workspace and use this repo for status/proof-roadmap documentation.

Every public proof update should report:

```text
lake build result
actual sorry count
actual axiom count
actual implemented_by count
exact final theorem name, if any
```
