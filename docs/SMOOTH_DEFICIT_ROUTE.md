# Smooth deficit route

This note records the current proof architecture.

## Exact smooth fibres

For each smooth value \(u\), define the exact fibre

\[
F_u(n,t)=\{1\le i\le t:s_t(n+i)=u\}.
\]

The exact smooth-fibre route avoids using CRT/residue objects as if they were exact value fibres.

## Fibre accounting

Let:

\[
f(n,t)=|\{u:F_u(n,t)\ne\varnothing\}|.
\]

Let:

\[
S(n,t)=|\{u:|F_u(n,t)|=1\}|.
\]

Let:

\[
\sigma(n,t)=\sum_u(|F_u(n,t)|-2)_+.
\]

For a fibre of size \(m\):

- if \(m=1\), its contribution to \(2f-t\) is \(+1\);
- if \(m=2\), its contribution is \(0\);
- if \(m\ge3\), its contribution is \(-(m-2)\).

Therefore:

\[
2f(n,t)-t=S(n,t)-\sigma(n,t).
\]

## Deficit

Define:

\[
D(n,t)=(\sigma(n,t)-S(n,t))_+.
\]

Then:

\[
f(n,t)\ge \frac{t-D(n,t)}2.
\]

Thus:

- if \(D(n,t)\le \theta t\) for fixed \(\theta<1\), then \(f(n,t)\ge \frac{1-\theta}{2}t\);
- if \(D(n,t)=o(t)\), then \(f(n,t)\ge (1/2-o(1))t\).

## Ambitious route

Prove:

\[
D(n,t)=o(t).
\]

This gives:

\[
f(n,t)\ge (1/2-o(1))t.
\]

A sufficient path is:

1. prove relevant singleton domination:
   \[
   \sigma_{\mathrm{rel}}\le S_{\mathrm{rel}};
   \]
2. prove non-relevant excess is sublinear:
   \[
   \operatorname{NonRelevantSmoothExcess}=o(t).
   \]

## Fallback route

Prove:

\[
D(n,t)\le t/2.
\]

Then:

\[
f(n,t)\ge t/4.
\]

This is already enough for Erdős #461.

A sufficient input package is:

```lean
def smoothDeficit_core_linear_input : Prop :=
  (∀ n t, RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t)
  ∧
  (∀ n t, 2 * NonRelevantSmoothDeficit n t ≤ t)
```

Then one should prove:

```lean
theorem erdos461_lower_bound_quarter
    (h : smoothDeficit_core_linear_input) :
    ∀ n t, t ≤ 4 * smoothDistinct n t
```

or a rational/real equivalent.

## Strong route input package

```lean
def smoothDeficit_core_sublinear_input : Prop :=
  (∀ n t, RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t)
  ∧
  (∀ ε > 0, ∃ T, ∀ n t, T ≤ t →
      (NonRelevantSmoothDeficit n t : ℝ) ≤ ε * t)
```

Then one should prove:

```lean
theorem smoothDistinct_half_asymptotic
    (h : smoothDeficit_core_sublinear_input) :
    ∀ ε > 0, ∃ T, ∀ n t, T ≤ t →
      ((1 / 2 : ℝ) - ε) * t ≤ smoothDistinct n t
```

This stronger statement implies Erdős #461.