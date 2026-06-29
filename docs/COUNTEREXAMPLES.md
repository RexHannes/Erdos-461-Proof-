# Counterexamples and red-team checks

This note records known failed targets so that future proof-search does not chase false statements.

## 1. Global singleton domination is false

The target

\[
\sigma(n,t)\le S(n,t)
\]

would imply \(f(n,t)\ge t/2\), but it is false under the current convention.

Known computational witnesses:

```text
t = 6000, n = 2857336:
smoothDistinct = 2984
smoothSingleton = 2584
smoothSigma = 2616
smoothSingleton - smoothSigma = -32
```

```text
t = 10000, n = 6652777:
smoothDistinct = 4913
smoothSingleton = 4340
smoothSigma = 4514
smoothSingleton - smoothSigma = -174
```

These do not refute Erdős #461. They only refute the stronger route \(f\ge t/2\) via \(\sigma\le S\).

## 2. Correct target: deficit control

Define:

\[
D(n,t)=(\sigma(n,t)-S(n,t))_+.
\]

The correct target is:

\[
D(n,t)\le \theta t\qquad(\theta<1),
\]

or stronger:

\[
D(n,t)=o(t).
\]

## 3. Rough-number denominator

The uniform rough-number bound with denominator \(\log t\) is too strong.

The plausible corrected form is:

\[
R_t(X,L)\ll \frac{L}{\log L},\qquad 3\le L\le t.
\]

The interval length \(L\), not the global smoothness threshold \(t\), controls the local rough-number density.

## 4. Affine/raw mass and multiplicity warning

Earlier raw affine-shadow mass targets are not suitable final targets because one high-rank value can have several semiprime kernels in one dyadic bin. The excess route is preferable to raw mass.

## 5. Public proof-hygiene conclusion

Do not present any of the following as final proof targets:

```lean
smoothSigma_le_smoothSingleton
roughNumbersInShortInterval_bound_log_t
rawAffineShadowMass_absorbed_by_exactRank2Mass
```

The current proof target is deficit control.