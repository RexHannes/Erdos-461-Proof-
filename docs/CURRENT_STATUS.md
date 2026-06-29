# Current status

This repository is **not yet a formal proof** of Erdős #461.

The current proof-search has narrowed the problem to an exact-fibre deficit / signed-surplus route.

See also: [`PROGRESS_2026_06_30.md`](PROGRESS_2026_06_30.md).

## Exact identity

Let:

- \(f(n,t)=|\{s_t(n+1),\dots,s_t(n+t)\}|\);
- \(S(n,t)\) be the number of singleton exact smooth fibres;
- \(\sigma(n,t)=\sum_u(|F_u(n,t)|-2)_+\) be total fibre excess.

The key identity is:

\[
2f(n,t)-t=S(n,t)-\sigma(n,t).
\]

Define the deficit:

\[
D(n,t)=(\sigma(n,t)-S(n,t))_+.
\]

Then:

\[
f(n,t)\ge \frac{t-D(n,t)}2.
\]

So any fixed linear deficit bound

\[
D(n,t)\le \theta t,\qquad \theta<1,
\]

proves Erdős #461.

## What is known in the current workspace

The latest Aristotle workspace contains useful exact-fibre and deficit scaffolding, but still has open/sorried targets. The public repo therefore records the route, not a completed theorem.

The strong target

\[
\sigma\le S
\]

is false under the current convention. Known computational witnesses include:

```text
t = 6000, n = 2857336:
smoothDistinct = 2984
smoothSingleton = 2584
smoothSigma = 2616

 t = 10000, n = 6652777:
smoothDistinct = 4913
smoothSingleton = 4340
smoothSigma = 4514
```

Thus the proof target is not `smoothSigma_le_smoothSingleton`; it is deficit or signed-surplus control.

## Latest progress: signed-surplus rescue

The current repair is to avoid losing cancellation by truncating relevant and non-relevant deficits separately.

Define signed surpluses:

```lean
SignedRelevantSurplus n t
SignedNonRelevantSurplus n t
```

morally representing:

\[
\sigma_{\rm rel}-S_{\rm rel},\qquad
\sigma_{\rm nonrel}-S_{\rm nonrel}.
\]

The target identity is:

```lean
theorem signed_surplus_split (n t : ℕ) :
    SignedRelevantSurplus n t + SignedNonRelevantSurplus n t
      = (t : ℤ) - 2 * (smoothDistinct n t : ℤ)
```

This identity is bookkeeping, but it is important because it changes the final target to a signed linear bound:

\[
(\sigma_{\rm rel}-S_{\rm rel})
+(\sigma_{\rm nonrel}-S_{\rm nonrel})
\le \theta t,
\qquad \theta<1.
\]

Lean-style target:

```lean
def SignedSurplusLinearBound (θ : ℚ) : Prop :=
  ∀ n t : ℕ,
    (SignedRelevantSurplus n t : ℚ)
      + (SignedNonRelevantSurplus n t : ℚ) ≤ θ * t
```

If this holds for any fixed `θ < 1`, then Erdős #461 follows with constant \((1-\theta)/2\).

## Computational evidence for Input A

The old relevant-domination target remains empirically encouraging:

```lean
theorem relevantSmoothExcess_le_relevantSmoothSingleton
    (n t : ℕ) :
    RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t
```

The latest native search reported no positive relevant surplus in random large-scale tests up to `t ≤ 3000` and `n ≤ 50,000,000`; the reported maximum value of relevant surplus was still negative, around `-224`.

This is evidence, not a proof. It must still be checked against the exact Lean definition of `RelevantHighRankValue`.

## Main remaining inputs

The current conditional route needs at least one of the following proof packages.

### Package A: original relevant/non-relevant route

#### Input A: relevant singleton domination

\[
\sigma_{\mathrm{rel}}(n,t)\le S_{\mathrm{rel}}(n,t).
\]

Lean-style target:

```lean
theorem relevantSmoothExcess_le_relevantSmoothSingleton
    (n t : ℕ) :
    RelevantSmoothExcess n t ≤ RelevantSmoothSingleton n t
```

Computational evidence so far supports this target, but it is not yet proved.

#### Input B: non-relevant sublinear deficit/excess

The likely route is via a corrected rough-number estimate:

\[
R_t(X,L)\ll \frac{L}{\log L},\qquad 3\le L\le t.
\]

The denominator should be \(\log L\), not the false stronger \(\log t\) denominator.

This would give:

\[
\operatorname{NonRelevantSmoothExcess}(n,t)
  \ll t\frac{\log\log t}{\log t}=o(t).
\]

Combined with Input A, this yields:

\[
D(n,t)=o(t),
\]

and hence:

\[
f(n,t)\ge (1/2-o(1))t.
\]

### Package B: signed linear route

It is enough to prove the signed total surplus bound:

\[
(\sigma_{\rm rel}-S_{\rm rel})
+(\sigma_{\rm nonrel}-S_{\rm nonrel})
\le \theta t
\]

for some fixed \(\theta<1\).

This package may be weaker and more realistic than forcing all componentwise truncated deficits separately, because it preserves cancellation.

## Fallback target

For merely proving Erdős #461, it is enough to prove:

\[
D(n,t)\le t/2.
\]

Then:

\[
f(n,t)\ge t/4.
\]

This is less strong than the asymptotic half-bound, but it is already a full solution if unconditional.

## Current proof status

- `signed_surplus_split`: priority bookkeeping theorem; should be proved no-sorry.
- `signed_surplus_bound_implies_linear_f`: bridge from signed bound to linear lower bound.
- `RelevantSingletonDomination`: still open.
- `roughNumbersInShortInterval_bound_corrected_logLength`: still open analytic input.
- `SignedSurplusLinearBound θ` for explicit `θ < 1`: main breakthrough target.

No final theorem should be advertised until the relevant Lean files build with no new `sorry`, no new `axiom`, and no `implemented_by` on the final path.
