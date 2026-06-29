# Archive 39 audit

This note records the status observed in `3ed908db-d5a7-43b9-8d67-fc5a3bb45298-aristotle (39).tar.gz`.

## Summary

The direction is correct: the project is now using the exact smooth-fibre deficit route.

However, the proof is still conditional. The archive should not be read as a complete proof of Erdős #461.

## SmoothDeficit.lean status

The elementary algebraic bridge has largely improved.

Proved-looking items include:

```lean
smooth_window_lower
smoothDeficit_bound_implies_linear_f
smoothDeficit_eps_asymptotic
smoothSingleton_eq_card
smoothSingleton_eq_relevant_add_nonRelevant
smoothDeficit_le_relevantDeficit_plus_nonRelevantDeficit
erdos461_from_deficit_bounds
smoothDeficit_le_half_t_implies_quarter_lower
erdos461_from_relevantSingletonDomination_and_roughInput
erdos461_from_core_linear_input
```

Remaining actual proof `sorry`s in `SmoothDeficit.lean`:

```lean
relevantSmoothExcess_le_relevantSmoothSingleton
roughNumbersInShortInterval_bound_corrected_logLength
nonRelevantSmoothDeficit_sublinear_from_roughInput
```

The theorem

```lean
relevantSmoothDeficit_eq_zero
```

is proved only by invoking

```lean
relevantSmoothExcess_le_relevantSmoothSingleton
```

so it should not be counted as an independent proof unless the latter is closed.

## Conditional final theorem

The important conditional theorem is:

```lean
erdos461_from_relevantSingletonDomination_and_roughInput
```

It states that if:

1. relevant singleton domination holds; and
2. the non-relevant half-window slack holds,

then a linear lower bound follows.

This is useful and honest.

## Presentation issue still present

The theorem

```lean
erdos461_lower_bound_quarter
```

currently takes only:

```lean
hB : RoughNumbersShortIntervalBound
```

but internally consumes the sorried theorem:

```lean
relevantSmoothExcess_le_relevantSmoothSingleton
```

For public review, this should be renamed/repaired so that both assumptions are explicit, for example:

```lean
theorem erdos461_lower_bound_quarter_from_inputs
    (hA : RelevantSingletonDomination)
    (hB : RoughInputNonRelevantSlack) :
    ∃ c : ℚ, 0 < c ∧ ∀ n t,
      (c * t : ℚ) ≤ smoothDistinct n t
```

or use the packaged input:

```lean
smoothDeficit_core_linear_input
```

## Import-closure warning

Although `SmoothDeficit.lean` has only three actual local sorries, its import closure still contains old affine-shadow sorries:

```lean
AffineRank2ShadowCollapse.lean
AffineRank2ShadowDyadic.lean
```

Therefore, before any proof claim, run:

```lean
#print axioms erdos461_from_core_linear_input
#print axioms erdos461_lower_bound_quarter
```

and check whether any `sorryAx` enters the final theorem.

## Verdict

Archive 39 is good conditional progress, not a proof.

Current remaining blockers:

```lean
relevantSmoothExcess_le_relevantSmoothSingleton
roughNumbersInShortInterval_bound_corrected_logLength
nonRelevantSmoothDeficit_sublinear_from_roughInput
```

For a fallback Erdős #461 proof, the required package is:

```lean
smoothDeficit_core_linear_input
```

For the stronger asymptotic route, the required package is:

```lean
smoothDeficit_core_sublinear_input
```
