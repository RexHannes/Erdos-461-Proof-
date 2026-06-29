# Erdős #461 proof-search workspace

This repository is a **clean public-facing workspace** for the current Erdős #461 proof-search route.

It is **not yet a complete proof** of Erdős #461. The current state is a conditional exact-fibre / deficit route, with remaining inputs listed in [`docs/CURRENT_STATUS.md`](docs/CURRENT_STATUS.md) and [`docs/NEXT_TASKS.md`](docs/NEXT_TASKS.md).

## Main principle

Let

\[
f(n,t)=\left|\{s_t(n+1),\dots,s_t(n+t)\}\right|.
\]

The proof-search now works with exact smooth fibres

\[
F_u(n,t)=\{1\le i\le t:s_t(n+i)=u\}.
\]

Define:

- `smoothDistinct` = number of nonempty exact smooth fibres;
- `smoothSingleton` = number of singleton fibres;
- `smoothSigma` = total excess \(\sum_u(|F_u|-2)_+\);
- `smoothDeficit = (smoothSigma - smoothSingleton)_+`.

The central identity is:

\[
2f(n,t)-t=S(n,t)-\sigma(n,t).
\]

Therefore, if one proves

\[
(\sigma-S)_+ \le \theta t\qquad(\theta<1),
\]

then

\[
f(n,t)\ge \frac{1-\theta}{2}t,
\]

which proves Erdős #461.

## Current route

The ambitious route is to prove

\[
f(n,t)\ge (1/2-o(1))t.
\]

The fallback route is to prove a fixed linear lower bound such as

\[
f(n,t)\ge t/4.
\]

Either unconditional bound proves Erdős #461; the former is stronger and more elegant, the latter is already enough.

## What is deliberately excluded

This repository should not contain the full experimental Aristotle workspace. Dead branches, old CRT/residue graph attempts, raw affine-mass attempts, and unrelated `PeelSemantics` work should remain in a private/archive branch, not in the main public proof path.

See [`docs/REVIEW_HYGIENE.md`](docs/REVIEW_HYGIENE.md).