# Non-relevant smooth excess checkpoint

This note summarizes the useful part of the latest `NonRelevantSmoothExcess.lean` development.

## Purpose

The affine/relevant route controls labels with sufficient small-prime structure. The remaining label-coverage concern was that small labels such as \(u=1\) and prime labels might carry excess outside that route.

The non-relevant smooth-excess file addresses this label-coverage issue.

## Main bookkeeping facts

The exact fibres partition the index window:

\[
\sum_u |F_u(n,t)|=t.
\]

Consequently the total excess is bounded by the whole window:

\[
\sigma(n,t)\le t.
\]

This is useful for coverage, but by itself it does not prove Erdős #461.

## Relevant/non-relevant split

The development splits total excess into relevant and non-relevant parts:

\[
\sigma=\sigma_{\mathrm{rel}}+\sigma_{\mathrm{nonrel}}.
\]

Prime powers \(p^a\), \(a\ge2\), are relevant under the current convention if prime factors are counted with multiplicity. The truly non-relevant class is mainly unit and prime labels.

## Important caveat

The bound

\[
\sigma\le t
\]

is not enough for Erdős #461. The final identity is

\[
2f-t=S-\sigma.
\]

A proof needs control of the deficit

\[
D=(\sigma-S)_+,
\]

not merely an upper bound for \(\sigma\).

## Current use in the proof route

The non-relevant side should now be used in one of two ways:

### Strong route

Prove:

\[
\sigma_{\mathrm{nonrel}}=o(t).
\]

A likely route uses a corrected rough-number bound:

\[
R_t(X,L)\ll L/\log L.
\]

Then the global deficit becomes sublinear if relevant singleton domination is also proved.

### Fallback route

Prove:

\[
2D_{\mathrm{nonrel}}\le t.
\]

Together with relevant singleton domination, this implies

\[
D\le t/2,
\]

hence

\[
f(n,t)\ge t/4.
\]

This already proves Erdős #461 if unconditional.