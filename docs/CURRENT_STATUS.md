# Current status

This repository is **not yet a formal proof** of Erdős #461.

The current proof-search has narrowed the problem to an exact-fibre deficit route.

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

Thus the proof target is not `smoothSigma_le_smoothSingleton`; it is deficit control.

## Main remaining inputs

The current conditional route needs the following proof inputs.

### Input A: relevant singleton domination

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

### Input B: non-relevant sublinear deficit/excess

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