# Next tasks

This is the current short list of useful tasks.

## Task 1: remove elementary SmoothDeficit sorries

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

## Task 2: prove or refute relevant singleton domination

Main structural target:

```lean
theorem relevantSmoothExcess_le_relevantSmoothSingleton
    (n t : ℕ) :
    RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t
```

Equivalent:

```lean
theorem relevantSmoothDeficit_eq_zero
    (n t : ℕ) :
    RelevantSmoothDeficit n t = 0
```

If false, produce explicit native-checked data and repair to an error term small enough to imply a fixed deficit slack.

## Task 3: prove fallback non-relevant half-window bound

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

## Task 4: stronger non-relevant sublinear bound

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

## Task 5: public proof path cleanup

Do not upload the whole experimental workspace to this public repo.

Upload only final-route Lean files once their import closure is clean. Until then, keep code in private/archive workspace and use this repo for status/proof-roadmap documentation.