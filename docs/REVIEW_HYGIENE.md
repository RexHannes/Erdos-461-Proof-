# Review hygiene notes

This repository should be optimized for mathematical review, not for preserving every exploratory branch.

## Public-facing rule

The public `main` branch should contain only:

1. exact definitions needed by the final route;
2. the exact-fibre / smooth-deficit proof path;
3. a clear statement of remaining conditional inputs;
4. counterexamples to false routes;
5. build/audit instructions.

It should not contain every generated Aristotle file.

## Keep out of the main proof path

The following should be excluded from the public final path unless explicitly needed:

- unrelated `PeelSemantics` / carrier-genealogy work;
- old CRT/residue graph attempts treated as exact fibre data;
- old raw affine-mass branches;
- dead Core-B/dyadic packing attempts;
- diagnostic search logs;
- files with `sorry` that are not explicitly part of a conditional theorem statement.

Such material can be preserved in an `archive/` branch or private workspace, but it should not be imported by the final theorem.

## Lean audit checklist

Before claiming any formal proof, run:

```bash
lake build
grep -R "\bsorry\b" RequestProject
grep -R "\baxiom\b" RequestProject
grep -R "\badmit\b" RequestProject
grep -R "implemented_by" RequestProject
```

Also check the final theorem's axiom closure:

```lean
#print axioms erdos461_final
```

or whatever the final theorem is named.

## Conditional theorem hygiene

If the proof is still conditional, do not encode open inputs as `sorry` proofs of theorem statements.

Prefer an honest theorem of the form:

```lean
theorem erdos461_from_inputs
    (hRel : RelevantSingletonDomination)
    (hRough : CorrectedRoughNumberBound) :
    ∃ c > 0, ∀ n t, c * t ≤ smoothDistinct n t := ...
```

This is reviewable and honest: it proves the implication without hiding the open inputs.

## Suggested final file layout

```text
RequestProject/Erdos461/
  Main.lean
  ExactFibre.lean
  NonRelevantSmoothExcess.lean
  SmoothDeficit.lean
  RelevantSingleton.lean
  RoughInput.lean
  Final.lean

docs/
  CURRENT_STATUS.md
  SMOOTH_DEFICIT_ROUTE.md
  COUNTEREXAMPLES.md
  REVIEW_HYGIENE.md
```

## Rule for reviewers

A reviewer should be able to start from `Final.lean`, inspect its imports, and see exactly which assumptions are proved and which are external/conditional.

If unrelated files are present, they should not be imported by `Final.lean`.