# Roadmap: Combinatorial maps, embedded graphs, and the classification of surfaces

A compact surface is a finite object. Cut it along a triangulation and it becomes a finite
set of triangles with a gluing rule; encode the gluing rule as three involutions on a finite
set of flags and the entire topology of the surface is a computation in a permutation group.
This roadmap builds that dictionary in both directions and then uses it.

The summits:

1. **The realization theorem**: a finite combinatorial map with the surface condition
   realizes as a compact surface, and every compact surface arises this way.
2. **The Heffter–Edmonds–Ringel theorem**: cellular embeddings of a graph in surfaces
   correspond to rotation systems. This is the external check that the finite model is not
   merely adequate but exactly right.
3. **Normal forms**: every polygonal schema reduces to the standard word by a finite
   sequence of moves.
4. **The classification of compact surfaces**: every compact connected surface, with or
   without boundary, is homeomorphic to exactly one of the standard models, determined by
   Euler characteristic, orientability, and number of boundary components.
5. **Curves, cutting, and the mapping class group**: Dehn twists, the change-of-coordinates
   principle, and the Dehn–Lickorish generation theorem.
6. **Planarity**: Kuratowski, Whitney, Mac Lane, Fáry, and the five-colour theorem, as a
   validation layer for everything above.

Layer 10 needs a word about why it is here. It is not a claim to cover topological graph
theory. It is the end-to-end acceptance test for the map library: Kuratowski exercises the
embedding theory, Mac Lane exercises the homology and embedding layers against each other,
and the five-colour theorem exercises the plane-map API from the definitions to a
non-trivial conclusion. It is scoped against a named chapter so that its completeness can be
checked by someone who is not a graph theorist; see layer 10.

> **Naming.** The directory name `CombinatorialMaps` describes the method; `SurfaceTopology`
> would describe the subject. Either is defensible and the choice should be made by the
> maintainers.

---

## Relationship to other roadmaps

### This roadmap supplies

| Consumer | What it needs | Status there today |
|---|---|---|
| `GeometricTopology` layer 9 | the classification of closed orientable surfaces, for the splitting surface | listed as an input with no owner anywhere in Tau Ceti |
| `GeometricTopology` layer 9 | surface mapping classes | the layer says "consume layer 3's surface mapping classes", but layer 3 is diffeomorphism groups and does not build them |
| a future Four Colour roadmap | plane maps, duality, Euler's formula, the five-colour theorem | not built |

### This roadmap consumes

From [PlanarTopology](../PlanarTopology/README.md), at exactly five points:

| Item | Which layer here |
|---|---|
| `IsCombinatorialSurface` and its realization theorem | 3 |
| Radó: every compact surface is triangulable | 8 |
| the two-dimensional Hauptvermutung | 3, 7, 8 |
| `Surface.eulerChar`, well defined and invariant | 1, 7, 8 |
| the Schoenflies theorem and tameness of arcs and simple closed curves in a surface | 9 |
| Top = PL structure comparison and isotopy to a PL homeomorphism | 9 |

From Tau Ceti and Mathlib:

| Item | Location |
|---|---|
| `Equiv.Perm`, orbits, `MulAction`, cycle types | Mathlib |
| `AbstractSimplicialComplex`, `Realization`, `link`, subdivision, `SimplicialMap` | `TauCeti/AlgebraicTopology/SimplicialComplex/` |
| `IsTriangulable` | `TauCeti/Topology/Triangulable.lean` |
| structure theorem for finitely generated modules over a PID | `Mathlib.Algebra.Module.PID` |
| free groups, group presentations, `Subgroup.closure`, abelianization | Mathlib |
| `Isotopic`, `AmbientIsotopic` | `TauCeti/Topology/Homotopy/` |
| the multigraph `Graph α β` | Mathlib, **verify the current API before layer 5** |
| `Combinatorics/SimpleGraph/{Acyclic, BranchComponents, PathGraph}` | Tau Ceti |

⚠ `TauCeti/LinearAlgebra/Matrix/SmithNormalForm.lean` covers only **square** integer matrices
of positive determinant. Boundary matrices are rectangular. Use the PID structure theorem.

### Cross-roadmap edits proposed alongside this PR

- `GeometricTopology` layer 9: record that the surface classification and the surface
  mapping class group listed as inputs are owned here.
- The same cross-links to `GeometricTopology` layers 1, 2, and 11 and to `ConformalMapping`
  that [PlanarTopology](../PlanarTopology/README.md) proposes; land them in whichever pull
  request merges first and reference both new roadmaps.

---

## What is already there

Verified against Lean `v4.34.0-rc1` and Mathlib `master` at
`05ae0103f49b1ad1248f6039bbbad43d8aeb52a9`.

**Absent from Tau Ceti, confirmed by grep.** No `GMap`, no `Hypermap`, no `Cellulation`, no
`rotationSystem`, no `eulerChar`, no `IsCombinatorialManifold`. `TauCeti/Combinatorics/`
contains Brauer diagrams, enumerative combinatorics, quivers, Young tableaux, and three thin
`SimpleGraph` files. **The entire finite-map side of this roadmap is greenfield.**

**Present and relevant.** `TauCeti/AlgebraicTopology/UniversalCover/` is substantial and
includes fundamental groups of the circle, the torus, and the real projective plane, which
are useful cross-checks for layer 7. `TauCeti/AlgebraicTopology/SimplicialComplex/` is the
target of layer 3's comparison theorem.

## Check what is already in motion

Search the Lean Zulip for combinatorial maps, hypermaps, rotation systems, planarity, and
Kuratowski before starting layers 1 and 10. Check the status of Mathlib's multigraph
`Graph α β` before layer 5: layer 5 and layer 10 both depend on it, and if it is not yet
mature, the roadmap should derive its own graph type from maps and note the reversal.

---

## Encoding conventions

### One foundational object, several presentations

**The 2-dimensional generalized map is foundational.** A dart is a complete flag, so it
natively carries the side-incidence data that a face poset loses; boundary is recorded by
fixed points of the last involution; non-orientability needs no signed variant; and the
definition is dimension-polymorphic even though the topology here is not.

Oriented maps, hypermaps, cellulations, and polygonal schemas are **presentations**: each has
a constructor into generalized maps, accessors out, and its own natural operations, but not a
parallel realization theory. Realization, Euler characteristic, orientability, and the
operations are defined once, on generalized maps.

This is deliberate, and it follows the discipline `GeometricTopology` layer 4 states for knot
presentations: pick a hub, do not aim for the complete graph on presentations. Four co-equal
first-class types would mean four realization theories, four isomorphism notions, four
orientability definitions, and naturality of every conversion with respect to all of them.

The **six round trips** in layer 2 are therefore not a symmetry commitment. They are
acceptance tests against vacuity, and they are named as such.

### Bundled index types and isomorphism closure

Every finite object bundles its own index type: darts for maps, vertices for complexes,
labels for schemas. Every move relation is **closed under isomorphism of that type** before
`Relation.ReflTransGen` is applied.

⚠ This is not cosmetic. The Pachner 1↔3 move adds a vertex and the schema cancellation move
removes a letter, so both change the index type. A relation on a fixed index type cannot
express them, and without the isomorphism closure `ReflTransGen` demands that intermediate
index types line up on the nose.

### Realization

`GMap2.realization G` is defined **directly**, as the quotient of `D × Δ²` by the
side-gluings induced by the three involutions. It is always defined, with no hypotheses, and
it is the object that carries the topology.

⚠ **The flag complex of a generalized map is not in general an abstract simplicial complex.**
Take the projective plane as the word `aa`, with one vertex, one edge, and one face. As a
generalized map on darts `{1,2,3,4}` with

```
α₀ = (1 2)(3 4)      α₁ = (1 4)(2 3)      α₂ = (1 3)(2 4)
```

the involutions `α₀` and `α₂` commute, so this is a valid 2-generalized map; all three orbit
groups act transitively, so there is one cell of each dimension and `χ = 1 - 1 + 1 = 1`. The
flag vertex set has three elements and admits exactly one triangle. **Four darts collapse to
one simplex.** The one-vertex torus fails the same way at larger scale: eight darts, four
flag vertices, two distinct triples.

The bridge to `AbstractSimplicialComplex` is therefore a **theorem about a subdivision**, not
a definition: after the canonical barycentric subdivision of the map, the flag map is
injective and the flag complex is honest. The `aa` map above is the certified counter-witness
recorded in layer 3's acceptance criteria.

### Euler characteristic

```lean
def GMap2.eulerChar (G : GMap 2 D) : ℤ :=
  (Nat.card (G.Cell 0) : ℤ) - Nat.card (G.Cell 1) + Nat.card (G.Cell 2)
```

with `Cell 0 = Orb⟨α₁,α₂⟩`, `Cell 1 = Orb⟨α₀,α₂⟩`, `Cell 2 = Orb⟨α₀,α₁⟩`.

**Boundary is recorded entirely by fixed points of `α₂` and contributes no cell of any
dimension. There is no outer face.** A boundary edge has a two-element `⟨α₀,α₂⟩`-orbit and an
interior edge a four-element one; that is the decidable boundary test.

### Invariance

**Invariance comes from the Hauptvermutung, not from comparison with singular theory.** Every
invariant here is defined on a finite presentation and proved invariant under isomorphism,
subdivision, and the elementary moves. `PlanarTopology`'s Radó and Hauptvermutung then make
it a homeomorphism invariant.

⚠ No comparison theorem with Mathlib's `SingularHomology` or `FundamentalGroup` is a target.
None is needed, and none is currently provable: Mathlib's singular homology has no excision
and no Mayer–Vietoris at the pinned commit.

Consequently, ⚠ **the homology and fundamental group here are named for what they are.** Use
`Cellulation2.homology` and `Cellulation2.edgePathGroup`, never bare `H₁` or `π₁`. A theorem
reading `H₁_torus : H₁ T² ≅ ℤ × ℤ` would overclaim. When the comparison becomes available it
will be a bridge theorem with two well-named sides.

---

## Structure

```
L0 finite conventions
 |
L1 generalized maps ------------------------------.
 |                                                 |
L2 presentations and the six round trips           |
 |                                                 |
L3 realization  <---- PlanarTopology L3, L5        |
 |                                                 |
L4 conservative operations ------.                 |
 |                                \                |
L5 embedded graphs and HER        L6 schemas and normal forms
 |                                 |
 |                                L7 combinatorial homology and edge-path group
 |                                 |
 |                                L8 classification  <---- PlanarTopology L5, L6
 |                                 |
 |                                L9 curves, cutting, mapping class groups
 |                                 |    <---- PlanarTopology L7, L8
 '------------------------------- L10 planarity (validation)
```

---

## Layer 0: Finite conventions

Mechanical, and worth landing first.

**Illustrative targets.**

```lean
/-- Cyclic words. Kept deliberately separate from `CircularOrder` and from the
    topological circle: a cyclic word is a finite sequence with repeated labels,
    taken modulo rotation. -/
def CyclicWord (α : Type*) : Type* := Quotient (rotationSetoid α)

structure BundledFinite where
  ι : Type
  fintype : Fintype ι
  decEq : DecidableEq ι

/-- The isomorphism closure of a relation on bundled objects. -/
def isoClosure {X : Type*} [HasIso X] (r : X → X → Prop) : X → X → Prop :=
  fun a b => ∃ a' b', Nonempty (a ≅ a') ∧ Nonempty (b ≅ b') ∧ r a' b'

theorem isoClosure_reflTransGen_congr : ...
```

**Design notes.** `Function.Involutive` and `Commute` in `Equiv.Perm D` are the Mathlib
idioms; do not hand-roll `∀ d, f (f d) = d` or `f ∘ g = g ∘ f`.

**Acceptance criteria.** `CyclicWord` equality is `Decidable` and distinguishes `abab` from
`aabb` while identifying `abab` with `baba`.

**Claimable units.** (i) cyclic words; (ii) bundling and isomorphism closure;
(iii) `ReflTransGen` lemmas for isomorphism-closed relations.

---

## Layer 1: Generalized maps

The foundational object.

**Illustrative targets.**

```lean
structure GMap (n : ℕ) (D : Type*) [Fintype D] [DecidableEq D] where
  adj : Fin (n + 1) → Equiv.Perm D
  involutive : ∀ i, Function.Involutive (adj i)
  commute_of_far : ∀ i j, i.1 + 2 ≤ j.1 → Commute (adj i) (adj j)

abbrev GMap2 (D : Type*) [Fintype D] [DecidableEq D] := GMap 2 D

def GMap.Cell (G : GMap n D) (i : Fin (n + 1)) : Type* :=
  MulAction.orbitRel.Quotient (Subgroup.closure {G.adj j | j ≠ i}) D

def GMap2.eulerChar (G : GMap2 D) : ℤ := ...

/-- Boundary. -/
def GMap2.IsBoundaryDart (G : GMap2 D) (d : D) : Prop := G.adj 2 d = d
instance : DecidablePred G.IsBoundaryDart

/-- The surface condition. -/
def IsSurfaceGMap (G : GMap2 D) : Prop :=
  Function.Injective (G.adj 0) ∧ (∀ d, G.adj 0 d ≠ d) ∧ (∀ d, G.adj 1 d ≠ d) ∧
    (∀ d, G.adj 2 d ≠ G.adj 0 d)      -- ⚠ see design notes
instance : DecidablePred (IsSurfaceGMap (D := D))

def IsClosedSurfaceGMap (G : GMap2 D) : Prop :=
  IsSurfaceGMap G ∧ ∀ d, G.adj 2 d ≠ d

/-- Orientability as a `Prop`; an orientation is data. -/
def GMap2.IsOrientable (G : GMap2 D) : Prop :=
  ∃ S : Set D, ∀ i d, (d ∈ S ↔ G.adj i d ∉ S)
structure GMap2.Orientation (G : GMap2 D) where ...
instance : DecidablePred (GMap2.IsOrientable (D := D))

def GMap2.genus (G : GMap2 D) : ℕ
def GMap2.boundaryComponentCount (G : GMap2 D) : ℕ
```

**Design notes.**

- ⚠ **`IsSurfaceGMap` is the highest-risk definition in either roadmap and the condition
  above is a candidate, not a decision.** Observe that an orbit of a set under two
  involutions is always a cycle or a path, and a path exactly when there are fixed points.
  So every vertex link, edge, and face boundary in a 2-generalized map is automatically a
  cycle or an arc: **the link condition is free**, and a pinched vertex, whose link would be
  two disjoint circles, cannot arise. If that is right, the surface condition reduces to
  fixed-point-freeness plus a non-degeneracy condition ruling out folded edges. Compare
  Damiand–Lienhardt and settle the exact non-degeneracy condition with a subject expert
  before building on it.
- The consequence of the observation, if it holds, is that **the vacuity risk moves out of
  this layer**. It sits in `PlanarTopology`'s `IsCombinatorialSurface`, where pinch points
  genuinely are possible, and in layer 2's cellulation constructor. Point the counter-witness
  battery there.
- Orientability as a dart bipartition in which every non-trivial involution changes side is
  the right definition and the common trap is to conflate it with a choice of orientation.
  Keep the `Prop` and the data apart.
- The `commute_of_far` condition is stated only for `i + 2 ≤ j`; supply the symmetric lemma
  so that contributors do not prove it awkwardly at every use.

**Acceptance criteria.** The following table is checked by `decide`, with the maps written
out explicitly.

| Map | darts | V | E | F | χ | orientable |
|---|---|---|---|---|---|---|
| sphere, tetrahedral | 24 | 4 | 6 | 4 | 2 | yes |
| projective plane, `aa` | 4 | 1 | 1 | 1 | 1 | no |
| torus, `aba⁻¹b⁻¹` | 8 | 1 | 2 | 1 | 0 | yes |
| Klein bottle, `abab⁻¹` | 8 | 1 | 2 | 1 | 0 | no |
| disc, `n`-gon | 2n | n | n | 1 | 1 | yes |
| annulus | 8 | 2 | 3 | 1 | 0 | yes |
| Möbius band | 8 | 2 | 3 | 1 | 0 | no |

Counter-witnesses: `IsSurfaceGMap` is **false** for a map with a folded edge
(`α₂ d = α₀ d`), and **false** for a map with a fixed point of `α₀` or of `α₁`.
`IsOrientable` is **false** for the Klein bottle map and **true** for the torus map, decided
by the same procedure.

**Claimable units.** (i) the structure and basic permutation lemmas; (ii) cells, orbits, and
decidability; (iii) Euler characteristic and the example table; (iv) boundary darts and the
boundary test; (v) `IsSurfaceGMap` and its counter-witnesses; (vi) orientability;
(vii) genus and boundary component count.

---

## Layer 2: Presentations and the six round trips

Each presentation gets a constructor into generalized maps, accessors out, and its own
natural operations. None gets a realization theory.

**Illustrative targets.**

```lean
/-- Oriented map: a rotation system. -/
structure CombinatorialMap (D : Type*) [Fintype D] [DecidableEq D] where
  σ : Equiv.Perm D          -- rotation at vertices
  α : Equiv.Perm D          -- edge involution
  α_involutive : Function.Involutive α
  α_free : ∀ d, α d ≠ d
def CombinatorialMap.toGMap2 : GMap2 (D × Bool)

/-- Hypermap. -/
structure Hypermap (D : Type*) [Fintype D] [DecidableEq D] where
  σ φ α : Equiv.Perm D
  comp : σ * φ * α = 1
def Hypermap.toGMap2 : GMap2 (D × Bool)

/-- Cellulation: a presentation, not an independent structure with its own validity theory. -/
structure Cellulation2 where ...
def Cellulation2.toGMap2 : GMap2 _

/-- Polygonal schema. -/
structure PolygonalSchema where
  label : Type
  fintypeLabel : Fintype label
  faces : List (CyclicWord (label × Bool))
  paired : ∀ l, (occurrences l).length = 2
def PolygonalSchema.toGMap2 : GMap2 _
```

**The six round trips.** These are acceptance tests against vacuity, not a claim that the
presentations are co-equal.

1. `K.toGMap2.flagComplex ≅ K.barycentricSubdivision` for a combinatorial surface `K`.
   ⚠ This is the master test. If it holds, the generalized map is not lying about the
   surface. It is stated here and proved in layer 3, where realization exists.
2. `|flagComplex G| ≃ₜ G.realization`. The two realizations agree. Proved in layer 3.
3. `G.toCellulation.toGMap2 ≅ G`, up to the canonical subdivision. Tests that the cellulation
   constructor loses no flag data.
4. `M.toGMap2.toCombinatorialMap ≅ M` after choosing the induced orientation. Tests the
   orientation encoding.
5. `G.dual.dual ≅ G`, with canonically homeomorphic realizations.
6. `S.toGMap2.boundaryWord = S.faces` up to cyclic equivalence and relabelling.

**Acceptance criteria.** ⚠ **Exhibit two non-isomorphic surface cellulations with the same
incidence poset.** One such example retires the "why is this not just a poset" objection
permanently and is the justification for `Cellulation2` carrying side-incidence data at all.
Draw it from the Damiand–Lienhardt figures.

Each conversion is checked on the layer 1 example table, and each round trip is checked to
fail for a degenerate input, so that it is visibly not a tautology.

**Claimable units.** (i) oriented maps and the conversion; (ii) hypermaps and the
conversion; (iii) cellulations and the conversion; (iv) polygonal schemas and the
conversion; (v) round trips 3, 4, and 6; (vi) the poset counter-example.

---

## Layer 3: Realization

Where the finite side meets [PlanarTopology](../PlanarTopology/README.md).

**From PlanarTopology.** `IsCombinatorialSurface`, its realization theorem, the
Hauptvermutung.

**Illustrative targets.**

```lean
/-- Direct. No hypotheses, no abstract simplicial complex. -/
def GMap2.realization (G : GMap2 D) : Type* :=
  Quotient (gluingSetoid G)    -- of `D × StandardSimplex (Fin 3)`

instance : TopologicalSpace (GMap2.realization G)
instance : CompactSpace (GMap2.realization G)

theorem IsSurfaceGMap.realization_isSurface (h : IsSurfaceGMap G) :
    IsCompactSurfaceWithBoundary G.realization

theorem IsSurfaceGMap.eulerChar_eq (h : IsSurfaceGMap G) :
    Surface.eulerChar G.realization = G.eulerChar

/-- The simplicial bridge: a theorem about a subdivision, not a definition. -/
theorem GMap2.flagComplex_isSimplicial_of_subdivided (G : GMap2 D) :
    Function.Injective (G.barycentricSubdivision.flagTriple)

theorem GMap2.realization_homeomorph_flagComplex (h : IsSurfaceGMap G) :
    G.realization ≃ₜ Realization (G.barycentricSubdivision.flagComplex)

/-- Every compact surface arises. Uses Rado. -/
theorem exists_gmap_of_compactSurface (M : Type*) [CompactSurface M] :
    ∃ (D : Type) (_ : Fintype D) (G : GMap2 D), IsSurfaceGMap G ∧
      Nonempty (G.realization ≃ₜ M)

/-- Round trips 1 and 2 from layer 2. -/
theorem flagComplex_toGMap2 (hK : IsCombinatorialSurface K) :
    K.toGMap2.flagComplex ≅ K.barycentricSubdivision
```

**Design notes.**

- ⚠ Do not define realization through `AbstractSimplicialComplex`. See the encoding
  conventions and the `aa` counter-witness.
- The subdivision needed for the bridge may be one barycentric step or two, depending on the
  map. State the theorem for the canonical subdivision that makes the flag triple injective
  and prove that it does; do not assert a fixed number of steps.

**Acceptance criteria.** The `aa` generalized map from the encoding conventions is a
certified counter-witness: it is a valid surface map, its flag triple map is **not**
injective, and its unsubdivided flag complex has one triangle where the map has four darts.
Realization is checked to produce the correct Euler characteristic on the full layer 1
table, through `PlanarTopology`'s `Surface.eulerChar`, not through `GMap2.eulerChar`.

**Claimable units.** (i) the gluing setoid and the quotient topology; (ii) compactness and
the surface property; (iii) the flag triple, injectivity after subdivision, and the
counter-witness; (iv) the homeomorphism with the flag complex realization;
(v) `exists_gmap_of_compactSurface`; (vi) Euler characteristic agreement.

**Unlocks.** Everything below, and `GeometricTopology` layer 11's dimension-two case.

---

## Layer 4: Conservative operations

Operations that change the presentation and not the realization. Each carries a realization
theorem, not merely cell-count bookkeeping.

**Illustrative targets.**

```lean
def GMap2.subdivideEdge   (G : GMap2 D) (e : G.Cell 1) : GMap2 _
def GMap2.subdivideFace   (G : GMap2 D) (f : G.Cell 2) : GMap2 _
def GMap2.contractEdge    (G : GMap2 D) (e : G.Cell 1) (h : ¬ e.IsLoop) : GMap2 _
def GMap2.dual            (G : GMap2 D) : GMap2 D
def GMap2.connectedSum    (G H : GMap2 D) : GMap2 _
def GMap2.cutAlong        (G : GMap2 D) (c : G.CombinatorialCurve) : GMap2 _
def GMap2.orientationDoubleCover (G : GMap2 D) : GMap2 (D × Bool)
def GMap2.cap             (G : GMap2 D) : GMap2 _   -- glue a disc on each boundary circle
def GMap2.double          (G : GMap2 D) : GMap2 (D × Bool)

theorem GMap2.subdivideEdge_realization : (G.subdivideEdge e).realization ≃ₜ G.realization
theorem GMap2.dual_realization          : G.dual.realization ≃ₜ G.realization
theorem GMap2.dual_eulerChar            : G.dual.eulerChar = G.eulerChar

theorem GMap2.orientationDoubleCover_isOrientable :
    G.orientationDoubleCover.IsOrientable
theorem GMap2.orientationDoubleCover_eulerChar :
    G.orientationDoubleCover.eulerChar = 2 * G.eulerChar
theorem GMap2.orientationDoubleCover_isConnected (h : ¬ G.IsOrientable) :
    IsConnected G.orientationDoubleCover
theorem GMap2.orientationDoubleCover_isCovering :
    IsCoveringMap (G.orientationDoubleCover.projection)

theorem GMap2.cap_eulerChar : G.cap.eulerChar = G.eulerChar + G.boundaryComponentCount
theorem GMap2.double_eulerChar : G.double.eulerChar = 2 * G.eulerChar
```

**Design notes.**

- The **orientation double cover** is a genuine covering map and works with boundary: the
  boundary of the cover double-covers the boundary of the base, so each boundary circle
  either lifts to two circles or to one circle wrapping twice according to whether it is
  orientation-preserving. State it in that generality; the boundary behaviour is what people
  get wrong.
- ⚠ The **double** `DM = M ∪_∂M M` is *not* a covering. It is a quotient by an involution
  with fixed points, `Fix σ = ∂M` and `DM/σ = M`. No covering space can remove boundary: a
  covering map is a local homeomorphism, boundary is a local property, so `∂M = ∅` if and
  only if the cover has empty boundary. Record the double for its Euler characteristic
  identity and for later use; do not attempt to derive triangulation with boundary from it,
  which would require an equivariant triangulation theorem.
- **Capping** is the operation that makes the classification with boundary cheap: every
  compact surface with boundary is a closed surface minus `b` open discs. That reduction is
  layer 8's main structural simplification.
- Do not build the Petrie dual or higher-dimensional operations. They were "where
  algebraically natural" in an earlier draft, which is not a target.

**Acceptance criteria.** `dual (dual G) ≅ G` on the layer 1 table (round trip 5).
`orientationDoubleCover` of the projective plane map is the sphere map, of the Klein bottle
map is the torus map, and of an already orientable map is disconnected. `cap` of the disc
map is the sphere map. `double` of the Möbius band map is the Klein bottle map. All by
`decide` on the explicit maps.

**Claimable units.** (i) subdivision operations; (ii) contraction and deletion;
(iii) duality; (iv) connected sum; (v) cutting along a combinatorial curve;
(vi) orientation double cover; (vii) capping and doubling.

---

## Layer 5: Embedded graphs and the Heffter–Edmonds–Ringel theorem

The external validation of the whole finite model. It says the maps are not merely an
adequate encoding of embedded graphs but exactly the right one.

**From layers 1–4.** **From Mathlib.** the multigraph `Graph α β`; ⚠ verify its API first.

**Illustrative targets.**

```lean
def GMap2.underlyingGraph (G : GMap2 D) : Graph (G.Cell 0) (G.Cell 1)

structure GraphEmbedding (Γ : Graph α β) (M : Type*) [Surface M] where ...
def GraphEmbedding.IsCellular : Prop   -- every face is an open disc

def RotationSystem (Γ : Graph α β) : Type* := ...

/-- Heffter-Edmonds-Ringel. -/
theorem cellularEmbeddings_equiv_rotationSystems (Γ : Graph α β) (hΓ : Γ.Connected) :
    {e : Σ M, GraphEmbedding Γ M // e.2.IsCellular} / EquivalenceOfEmbeddings
      ≃ RotationSystem Γ / EquivalenceOfRotations

/-- Euler's formula for a cellular embedding. -/
theorem eulerFormula_cellular {Γ : Graph α β} (e : GraphEmbedding Γ M) (h : e.IsCellular) :
    (Γ.vertexCount : ℤ) - Γ.edgeCount + e.faceCount = Surface.eulerChar M

def Graph.genus (Γ : Graph α β) : ℕ            -- needs layer 8
def Graph.nonorientableGenus (Γ : Graph α β) : ℕ
```

**Design notes.**

- ⚠ The correspondence is only for **cellular** embeddings. A graph embedded in a torus with
  a non-disc face is the counter-witness and must appear in the acceptance criteria.
- The orientable case runs on `CombinatorialMap` (rotation systems); the general case needs
  the signed or generalized-map version. State both.
- `Graph.genus` minimises over surfaces and therefore waits on layer 8. Split the layer at
  that point so that everything before it can be claimed immediately.

**Acceptance criteria.** `K₅` and `K₃,₃` have genus 1 and embed cellularly in the torus.
`IsCellular` is **false** for `K₄` drawn inside a disc in the torus. Euler's formula is
checked on the layer 1 table.

**Claimable units.** (i) the underlying multigraph; (ii) embeddings and cellularity;
(iii) rotation systems; (iv) the orientable HER correspondence; (v) the general HER
correspondence; (vi) Euler's formula; (vii) genus, after layer 8.

---

## Layer 6: Polygonal schemas and normal forms

**Illustrative targets.**

```lean
inductive ElementarySchemaMove : PolygonalSchema → PolygonalSchema → Prop
  | cancel   : ...        -- `a a⁻¹` adjacent, removes a label
  | relabel  : ...
  | cut      : ...
  | paste    : ...

def SchemaMove := isoClosure ElementarySchemaMove

def normalFormOrientable (g : ℕ) : PolygonalSchema
def normalFormNonorientable (k : ℕ) : PolygonalSchema
def normalFormWithBoundary (S : PolygonalSchema) (b : ℕ) : PolygonalSchema

theorem schema_reduces_to_normalForm (S : PolygonalSchema) (h : S.IsSurface) :
    ∃ N, IsNormalForm N ∧ Relation.ReflTransGen SchemaMove S N

theorem SchemaMove.realization_homeomorph (h : SchemaMove S T) :
    S.toGMap2.realization ≃ₜ T.toGMap2.realization
```

**Design notes.**

- ⚠ Every move changes the label type. The isomorphism closure from layer 0 is what makes
  `ReflTransGen` usable. Do not fix an alphabet.
- The reduction is the classical algorithm: cancel adjacent inverse pairs, make the schema
  have a single vertex class, group crosscaps, group handles, and convert a handle plus a
  crosscap into three crosscaps (Dyck's theorem). Each step needs its realization theorem
  from `SchemaMove.realization_homeomorph`, not a separate argument.
- Dyck's theorem, `handle + crosscap = 3 crosscaps`, is the step most often skipped and is
  what makes the normal forms exhaustive rather than merely available.

**Acceptance criteria.** The reduction is run by `decide` on `abab⁻¹` (Klein bottle),
`aabb` (Klein bottle again, reaching the same normal form), `abca⁻¹b⁻¹c⁻¹` (genus 1 with a
twist), and a schema with an unpaired letter, which must **fail** `IsSurface`.

**Claimable units.** (i) schemas and the surface condition; (ii) the moves and the
isomorphism closure; (iii) realization invariance of each move; (iv) reduction to a single
vertex class; (v) handle and crosscap grouping; (vi) Dyck's theorem; (vii) the normal forms
with boundary.

---

## Layer 7: Combinatorial homology and the edge-path group

Finite linear algebra and finite group presentations. ⚠ Not algebraic topology; see the
encoding conventions on invariance and naming.

**Illustrative targets.**

```lean
/-- Three finite free modules and two matrices. -/
def Cellulation2.chainComplex (C : Cellulation2) (R : Type*) [CommRing R] :
    ChainComplex (ModuleCat R) ℕ

theorem Cellulation2.d_comp_d (C : Cellulation2) : C.boundary₁ ∘ₗ C.boundary₂ = 0

def Cellulation2.homology (C : Cellulation2) (R : Type*) [CommRing R] (i : ℕ) : ModuleCat R

theorem Cellulation2.eulerPoincare (C : Cellulation2) (F : Type*) [Field F] :
    C.eulerChar = ∑ i, (-1 : ℤ) ^ i * Module.finrank F (C.homology F i)

theorem Cellulation2.homology_zero_iff_connected : ...
theorem Cellulation2.homology_two_iff_orientable (h : C.IsClosed) :
    Nontrivial (C.homology ℤ 2) ↔ C.IsOrientable

/-- The combinatorial fundamental group. -/
def Cellulation2.edgePathGroup (C : Cellulation2) : Group   -- presented

theorem Cellulation2.edgePathGroup_presentation (C : Cellulation2) (T : C.SpanningTree) :
    C.edgePathGroup ≃* PresentedGroup (generators := edges off T) (relations := face words)

theorem Cellulation2.abelianization_edgePathGroup :
    Abelianization C.edgePathGroup ≃* C.homology ℤ 1

theorem Graph.edgePathGroup_free (Γ : Graph α β) (h : Γ.Connected) :
    IsFreeGroup Γ.edgePathGroup ∧ Nat.card (FreeGroup.basis Γ.edgePathGroup) = 1 - Γ.eulerChar

/-- Invariance, from the moves and nothing else. -/
theorem Cellulation2.homology_subdivision : (C.subdivide).homology R i ≅ C.homology R i
theorem Cellulation2.edgePathGroup_subdivision : (C.subdivide).edgePathGroup ≃* C.edgePathGroup

/-- The normal forms. Uses the PID structure theorem, not Smith normal form. -/
theorem homology_normalFormOrientable (g : ℕ) :
    (normalFormOrientable g).homology ℤ 1 ≅ (ℤ : Type) ^ (2 * g)
theorem homology_normalFormNonorientable (k : ℕ) :
    (normalFormNonorientable k).homology ℤ 1 ≅ (ℤ : Type) ^ (k - 1) × ZMod 2
```

**Design notes.**

- Abelianizing a presentation turns relators into the columns of the boundary matrix, so
  `abelianization_edgePathGroup` is a mechanical algebraic theorem and not a topological one.
  This is Munkres' route with the topology removed.
- ⚠ Use `Mathlib.Algebra.Module.PID`, not `TauCeti/LinearAlgebra/Matrix/SmithNormalForm.lean`,
  which handles only square matrices of positive determinant.
- ⚠ Do not attempt van Kampen. The edge-path group is defined combinatorially and its
  invariance comes from the moves, so no comparison with `π₁` of the realization is needed.
  Record the absence of that comparison as a known gap with its reason.
- Cross-check opportunity: `TauCeti/AlgebraicTopology/UniversalCover/` already computes the
  fundamental groups of the circle, the torus, and the real projective plane. Comparing those
  against `edgePathGroup` of the corresponding normal forms is not a required target, but it
  would be strong independent evidence and is worth attempting once for each.

**Acceptance criteria.** The chain complexes of the layer 1 table are written out explicitly
and `d_comp_d` is checked by `decide`. `homology_two_iff_orientable` is checked **false** for
the Klein bottle and **true** for the torus. The `ZMod 2` torsion class in
`homology_normalFormNonorientable` is exhibited, not merely asserted, since it is what
distinguishes the nonorientable normal forms and an off-by-one here would be invisible.

**Claimable units.** (i) the chain complex and `d ∘ d = 0`; (ii) homology and Euler-Poincaré;
(iii) `H₀` and connectedness; (iv) `H₂` and orientability; (v) the edge-path group and its
presentation; (vi) abelianization; (vii) free groups for graphs; (viii) the normal-form
computations; (ix) invariance under the moves.

---

## Layer 8: The classification of compact surfaces

**From PlanarTopology.** Radó, the Hauptvermutung, Euler characteristic.

**Illustrative targets.**

```lean
inductive StandardSurface
  | orientable    (g : ℕ) (b : ℕ)
  | nonorientable (k : ℕ) (b : ℕ)

def StandardSurface.model : StandardSurface → Type
def StandardSurface.eulerChar : StandardSurface → ℤ
  -- orientable g b => 2 - 2*g - b ; nonorientable k b => 2 - k - b

/-- Existence. -/
theorem exists_standard_homeomorph (M : Type*) [CompactConnectedSurface M] :
    ∃ S : StandardSurface, Nonempty (M ≃ₜ S.model)

/-- Uniqueness. -/
theorem standard_homeomorph_unique {S T : StandardSurface}
    (h : Nonempty (S.model ≃ₜ T.model)) : S = T

/-- The library statement. -/
theorem classification_of_surfaces (M : Type*) [CompactConnectedSurface M] :
    ∃! S : StandardSurface, Nonempty (M ≃ₜ S.model)

/-- The complete invariant. -/
theorem homeomorph_iff_invariants {M N : Type*}
    [CompactConnectedSurface M] [CompactConnectedSurface N] :
    Nonempty (M ≃ₜ N) ↔
      Surface.eulerChar M = Surface.eulerChar N ∧
      (IsOrientable M ↔ IsOrientable N) ∧
      boundaryComponentCount M = boundaryComponentCount N

/-- The reduction that makes the boundary case cheap. -/
theorem exists_closed_minus_discs (M : Type*) [CompactConnectedSurface M] :
    ∃ (N : Type) (_ : ClosedConnectedSurface N) (b : ℕ), Nonempty (M ≃ₜ N.minusDiscs b)
```

**Design notes.**

- Existence: Radó gives a triangulation, layer 3 gives a generalized map, layer 6 reduces the
  schema to normal form, and layer 4's realization theorems transport the homeomorphism back.
- Uniqueness: layer 7's invariants separate the normal forms. Euler characteristic separates
  within each family, orientability separates the families, and boundary count is preserved
  because ⚠ `∂M` is a topological invariant only by `PlanarTopology`'s
  `invarianceOfDomain₂`. Cite it.
- ⚠ The `∃!` form is the library statement. A quotient-and-representative form, of the kind
  lean-eval statements use, is a bad library statement and should appear only in a bridge
  file if at all.
- The boundary case goes through `exists_closed_minus_discs`, via layer 4's `cap`. Do not run
  the normal-form reduction separately for the boundary case.

**Acceptance criteria.** The classification is instantiated on a surface presented only by
charts, with no combinatorial data supplied, and produces the correct `StandardSurface`.
`homeomorph_iff_invariants` is checked to **fail** if any one of the three invariants is
dropped: the Klein bottle and the torus agree on Euler characteristic and boundary count and
differ; the disc and the Möbius band agree on boundary count and differ; and so on. Dyck's
theorem is exercised by checking that `nonorientable 3 0` and `orientable 1 0 # nonorientable 1 0`
give the same standard surface.

**Claimable units.** (i) `StandardSurface` and its models; (ii) existence, closed case;
(iii) the capping reduction; (iv) existence with boundary; (v) uniqueness; (vi) the complete
invariant and its sharpness battery.

**Unlocks.** `GeometricTopology` layer 9. Layer 5's `Graph.genus`. Layer 9.

---

## Layer 9: Curves, cutting, and mapping class groups

**From PlanarTopology.** Schoenflies, tameness of arcs and simple closed curves in a surface,
isotopy to a PL homeomorphism.

**Illustrative targets.**

```lean
def SimpleClosedCurve (M : Type*) [Surface M] : Type*
def IsEssential {M} [Surface M] (c : SimpleClosedCurve M) : Prop
def IsSeparating {M} [Surface M] (c : SimpleClosedCurve M) : Prop

def cutAlong {M} [Surface M] (c : SimpleClosedCurve M) : Type*
theorem eulerChar_cutAlong : Surface.eulerChar (cutAlong c) = Surface.eulerChar M

def MappingClassGroup (M : Type*) [Surface M] : Type* :=
  Homeomorph M M ⧸ isotopicSetoid

/-- Base cases. -/
theorem mcg_disc     : MappingClassGroup (Metric.closedBall (0:ℂ) 1) ≃* Unit  -- Alexander
theorem mcg_sphere   : MappingClassGroup Sphere2 ≃* ZMod 2
theorem mcg_annulus  : MappingClassGroup Annulus ≃* Multiplicative ℤ

def dehnTwist {M} [OrientedSurface M] (c : SimpleClosedCurve M) : MappingClassGroup M

/-- The workhorse of the induction. -/
theorem change_of_coordinates {M} [ClosedOrientableSurface M]
    (c d : SimpleClosedCurve M) (hc : ¬ IsSeparating c) (hd : ¬ IsSeparating d) :
    ∃ f : M ≃ₜ M, f '' c.carrier = d.carrier

theorem mcg_torus : MappingClassGroup Torus ≃* Matrix.GeneralLinearGroup (Fin 2) ℤ

/-- Dehn-Lickorish, in Lickorish's original form. -/
theorem dehn_lickorish {M} [CompactConnectedOrientableSurface M] :
    Subgroup.closure (Set.range (dehnTwist (M := M)) ) = ⊤ ∧
    ∃ S : Finset (SimpleClosedCurve M), S.card = 3 * genus M - 1 ∧
      Subgroup.closure (dehnTwist '' S) = ⊤

/-- The category comparison. Consumes PlanarTopology layer 8. -/
theorem mcg_top_eq_pl {M} [CompactSurface M] :
    MappingClassGroupTop M ≃* MappingClassGroupPL M
theorem mcg_pl_eq_smooth {M} [CompactSurface M] :
    MappingClassGroupPL M ≃* MappingClassGroupSmooth M
```

**Design notes.**

- A **Dehn twist** about a simple closed curve takes an annular neighbourhood, cuts, rotates
  one side through a full turn, and reglues. It is the identity outside the annulus and its
  class depends only on the isotopy class of the curve. The annular neighbourhood comes from
  `PlanarTopology`'s tameness and collaring corollary.
- Target **Lickorish's** generating set of `3g - 1` twists, not Humphries' minimal `2g + 1`.
  Lickorish's induction on genus runs on the change-of-coordinates principle and cutting,
  which is machinery being built here anyway; Humphries' minimality is a separate and harder
  argument and belongs in a follow-on.
- ⚠ Do **not** write "of finite type". For a compact surface it is redundant, and in the
  literature it means "closed surface minus finitely many points and open discs", which
  suggests punctures. Punctured surfaces are a genuinely larger subject (the Birman exact
  sequence, point pushing) and are out of scope.
- ⚠ Chained multiplicative equivalences are not valid Lean. State the two comparisons
  separately, as above.

**Acceptance criteria.** `mcg_torus` is checked by exhibiting the twists corresponding to the
standard generators of `SL(2,ℤ)` and confirming they generate. `change_of_coordinates` is
checked to be **false** without the non-separating hypothesis: a separating and a
non-separating curve in the genus-2 surface are not related by any homeomorphism.
`dehnTwist` about an inessential curve is checked to be the identity class, so the map is
visibly not injective and the statement is not accidentally trivial.

**Claimable units.** (i) simple closed curves, essentiality, separation; (ii) cutting and its
Euler characteristic; (iii) the mapping class group and the three base cases; (iv) Dehn
twists; (v) the change-of-coordinates principle; (vi) `mcg_torus`; (vii) the Lickorish
induction; (viii) the category comparisons.

**Unlocks.** `GeometricTopology` layer 9.

---

## Layer 10: Planarity, as validation

⚠ **What this layer is.** It is the end-to-end acceptance test for layers 1 through 8, not a
claim to cover topological graph theory. Kuratowski exercises the embedding theory, Whitney
exercises duality against connectivity, Mac Lane exercises the homology layer against the
embedding layer, and the five-colour theorem exercises the plane-map API from the definitions
through to a non-trivial conclusion.

**Scope, stated so that it can be checked without a graph theorist in the room.** This layer
covers **Mohar and Thomassen, *Graphs on Surfaces*, chapter 2, in full**, together with
Fáry's theorem and Wagner's theorem. A reviewer who is not a graph theorist can hold the
layer against that table of contents.

**Excluded, with reasons:** planarity-testing algorithms such as Hopcroft–Tarjan and
LR-planarity (the theorems are the target, not the algorithms); Steinitz's theorem (a
convexity theorem needing polytope machinery not built here); embeddings in general surfaces,
face-width, edge-width, embedding extension, and Robertson–Seymour (a separate subject and a
separate roadmap); Grötzsch's theorem (named as a known gap rather than silently omitted);
the four colour theorem and the Heawood/Ringel–Youngs map colour theorem (see the
roadmap-for-a-roadmap below).

**Illustrative targets.**

```lean
theorem eulerFormula_plane {Γ : Graph α β} (e : PlaneEmbedding Γ) (h : Γ.Connected) :
    (Γ.vertexCount : ℤ) - Γ.edgeCount + e.faceCount = 2
theorem edge_bound_of_planar (Γ : Graph α β) (h : Γ.IsPlanar) (hs : Γ.IsSimple)
    (h3 : 3 ≤ Γ.vertexCount) : Γ.edgeCount ≤ 3 * Γ.vertexCount - 6
theorem exists_vertex_degree_le_five (Γ : Graph α β) (h : Γ.IsPlanar) (hs : Γ.IsSimple) :
    ∃ v, Γ.degree v ≤ 5

/-- Connectivity machinery. -/
theorem tutte_wheel (Γ : Graph α β) (h : Γ.IsThreeConnected) : ...
theorem exists_contractible_edge (Γ : Graph α β) (h : Γ.IsThreeConnected)
    (h5 : 5 ≤ Γ.vertexCount) : ∃ e, (Γ.contract e).IsThreeConnected

/-- Kuratowski, via Thomassen's 3-connectivity induction. -/
theorem kuratowski (Γ : Graph α β) (hs : Γ.IsSimple) :
    Γ.IsPlanar ↔ ¬ Γ.HasTopologicalMinor (completeGraph 5) ∧
                 ¬ Γ.HasTopologicalMinor (completeBipartiteGraph 3 3)
theorem wagner (Γ : Graph α β) (hs : Γ.IsSimple) :
    Γ.IsPlanar ↔ ¬ Γ.HasMinor (completeGraph 5) ∧
                 ¬ Γ.HasMinor (completeBipartiteGraph 3 3)
theorem isPlanar_minorClosed : Γ.IsPlanar → Δ.IsMinorOf Γ → Δ.IsPlanar

/-- Peripheral cycles, and Whitney. -/
theorem tutte_peripheral (Γ : Graph α β) (h : Γ.IsThreeConnected) (e : PlaneEmbedding Γ) :
    ∀ C, C.IsPeripheral ↔ e.IsFacial C
theorem whitney_unique_embedding (Γ : Graph α β) (h : Γ.IsThreeConnected)
    (h' : Γ.IsPlanar) : Subsingleton (PlaneEmbedding Γ / EquivalenceOfEmbeddings)
theorem whitney_two_isomorphism (Γ Δ : Graph α β) : ...

/-- Cycle space, cut space, duality, Mac Lane. -/
def Graph.cycleSpace (Γ : Graph α β) : Submodule (ZMod 2) (β → ZMod 2)
def Graph.cutSpace   (Γ : Graph α β) : Submodule (ZMod 2) (β → ZMod 2)
theorem cycleSpace_finrank : Module.finrank (ZMod 2) Γ.cycleSpace
    = Γ.edgeCount - Γ.vertexCount + Γ.componentCount
theorem cycleSpace_orthogonal_cutSpace : ...
theorem planar_dual_cycleSpace_eq_cutSpace (e : PlaneEmbedding Γ) : ...
theorem maclane (Γ : Graph α β) (hs : Γ.IsSimple) :
    Γ.IsPlanar ↔ ∃ B : Basis _ (ZMod 2) Γ.cycleSpace, ∀ e, (B.support e).card ≤ 2

/-- Straight-line and convex embeddings. -/
theorem fary (Γ : Graph α β) (hs : Γ.IsSimple) (h : Γ.IsPlanar) :
    ∃ e : PlaneEmbedding Γ, e.IsStraightLine
theorem tutte_spring (Γ : Graph α β) (h : Γ.IsThreeConnected) (h' : Γ.IsPlanar) :
    ∃ e : PlaneEmbedding Γ, e.IsConvex

theorem five_colour (Γ : Graph α β) (hs : Γ.IsSimple) (h : Γ.IsPlanar) :
    Γ.Colorable 5
```

**Design notes.**

- ⚠ Use **Thomassen's** proof of Kuratowski, via contracting an edge in a 3-connected graph.
  It is shorter and considerably more formalization-friendly than the Tutte and Bondy–Murty
  route through bridges and conflict graphs. The 3-connectivity machinery it needs (the wheel
  theorem, contractible edges) is shared with Whitney.
- The dual of a simple plane graph need not be simple, which is why duality lives on
  multigraphs and why layer 1 refused `SimpleGraph` as the foundation.
- Fáry's absence from a planarity roadmap is the first thing a combinatorialist notices.
  Prove it by induction on a vertex of degree at most five in a maximal planar graph.
- Whitney's unique-embedding theorem and Whitney's 2-isomorphism theorem are different
  results. Target both, and do not cite the 2-isomorphism paper for the unique-embedding
  statement.
- Tutte's spring embedding is a linear-algebra argument and is unusually formalization-friendly
  for its strength. It is the natural bridge toward Steinitz without committing to Steinitz.

**Acceptance criteria.** `K₅` and `K₃,₃` are non-planar and `K₄` is planar, by `decide`.
`edge_bound_of_planar` is checked to **fail** for a multigraph with parallel edges, exhibiting
the necessity of `IsSimple`. `whitney_unique_embedding` is checked to **fail** for a
2-connected but not 3-connected planar graph, with two inequivalent embeddings exhibited.
`maclane` is checked against `K₅`, whose cycle space has no sparse basis. Five-colouring is
computed on a concrete triangulation with a degree-5 vertex requiring a Kempe chain
interchange.

**Claimable units.** (i) plane embeddings, faces, and Euler's formula; (ii) the edge bound
and the degree-5 lemma; (iii) minors, topological minors, and minor-closure; (iv) the
3-connectivity machinery; (v) Kuratowski; (vi) Wagner and the equivalence; (vii) peripheral
cycles; (viii) Whitney unique embedding; (ix) Whitney 2-isomorphism; (x) cycle and cut
spaces; (xi) planar duality of the two spaces; (xii) Mac Lane; (xiii) Fáry;
(xiv) Tutte's spring embedding; (xv) the five-colour theorem.

---

## Roadmap-for-a-roadmap: map colouring

This section is motivation for a separate future roadmap and is not work here.

With plane maps, duality, Euler's formula, and the five-colour theorem in place, the four
colour theorem becomes a specification problem rather than a foundations problem: an
unavoidable set, discharging configurations, and a verified reducibility check. Gonthier's
Coq development is the reference for how the statement and the discharging argument should be
encoded, and its use of hypermaps is the reason this roadmap builds on permutations rather
than on `SimpleGraph`. The Heawood map colour theorem and the Ringel–Youngs solution for
higher genus are the natural companions, and they consume layer 5's `Graph.genus`.

An author for this is needed. It is a large project and the value of a roadmap for it is
mostly in pinning the encoding of unavoidability and reducibility before anyone writes code.

---

## Out of scope

- Everything in [PlanarTopology](../PlanarTopology/README.md): the Jordan curve theorem,
  Schoenflies, Radó, the Hauptvermutung, the PL toolkit, invariance of domain, tameness.
- Punctured surfaces, the Birman exact sequence, point pushing, and the mapping class groups
  of surfaces with marked points.
- Humphries' minimal generating set and the presentation of the mapping class group.
- Teichmüller theory, hyperbolic structures, and geodesic representatives.
- Non-compact surfaces and their classification.
- Singular homology, van Kampen, Mayer–Vietoris, cohomology, duality. See the encoding
  conventions on invariance.
- Higher-dimensional generalized maps beyond the dimension-polymorphic definition itself.
- Planarity-testing algorithms and the other layer 10 exclusions listed above.
- Dessins d'enfants, ribbon graphs, and the Galois action, which are natural follow-ons to
  layers 1 through 5 but are a different subject.

---

## Provenance and prior art

**Disclosure.** The author of this roadmap is the author of
`mccorvie/classification-of-surfaces`, cited below. That repository is prior art, not a
specification.

| Development | Licence | Coordination | What it evidences |
|---|---|---|---|
| `mccorvie/classification-of-surfaces` | Apache-2.0 | authored by this roadmap's author | that the classification closes end to end, and where the definitional risks are |
| Dufourd and collaborators, Coq generalized maps and hypermaps; Dehlinger–Dufourd, *Formalizing generalized maps in Coq* | — | design reference | that a permutation model materially reduces effort on plane topology |
| Gonthier, Coq four colour theorem | — | design reference | the hypermap encoding, and the discipline of deriving graphs from maps rather than the reverse |
| Damiand and Lienhardt, *Combinatorial Maps* | — | design reference | the generalized map definition, the non-degeneracy conditions, and the incidence-poset counter-example |

**What the prior art gets wrong, and what this roadmap does differently.** The existing
surface-classification development carried, for a period, a set of definitions on the
polygonal-schema side that typechecked and were vacuous: `SurfaceCellModel`,
`OrientableRel`, `realization`, `gluingRel`, and `Equivalent` admitted degenerate witnesses,
and continuous integration was green throughout. Agent throughput does not fix this: a
blitz can close goals against a vacuous statement indefinitely. That experience is why every
validity predicate here carries both a witness and a certified counter-witness, why
realization is a construction with a proved comparison rather than a structure field, why
the six round trips are framed as vacuity tests, and why the `aa` generalized map appears in
the encoding conventions rather than in a footnote.

---

## How to drive it

**Layers 0, 1, and 2 need nothing but `Equiv.Perm` and can start on day one**, in parallel
with all of `PlanarTopology`. That makes them the best first claims in either roadmap: they
are finite, decidable, testable by `decide`, and they have no upstream dependency at all.

Layer 3 is the first point of contact with `PlanarTopology` and waits on its layers 3 and 5.
Everything from layer 4 onward is downstream of layer 3.

**Highest risk:** layer 1, unit (v). ⚠ `IsSurfaceGMap` should be settled with a subject
expert before anything is built on it. It is a small, self-contained question that someone
who knows the subject can answer quickly, and getting it wrong is discovered late. The
candidate condition in layer 1 carries a warning marker for that reason.

**Highest external value:** layer 5, the Heffter–Edmonds–Ringel theorem. It is the only place
where the finite model is validated against an independent definition of the thing it
models, and it is what makes the round trips in layer 2 more than internal consistency.

**Most parallel:** layer 10, whose fifteen claimable units are largely independent of one
another once layers 5 and 8 have landed. It is also the layer most likely to attract an
outside contributor, which is a second reason for the explicit chapter-level scope statement.

Signatures throughout are indicative and have not been elaborated. Extract them into
`Suggested.lean` with `sorry` bodies and fix the ones that do not typecheck before claiming
the corresponding unit.

---

## References

- G. Damiand and P. Lienhardt, *Combinatorial Maps: Efficient Data Structures for Computer
  Graphics and Image Processing*. The definition of generalized maps, the non-degeneracy
  conditions, and the operations in layer 4.
- J.-F. Dufourd and collaborators, and Dehlinger–Dufourd, *Formalizing generalized maps in
  Coq*. The prior formalization experience.
- G. Gonthier, *Formal proof: the four-color theorem*, Notices AMS **55** (2008).
  The hypermap encoding and the discipline of deriving graphs from maps.
- S. Lando and A. Zvonkin, *Graphs on Surfaces and Their Applications*, chapter 1. The
  identification of embedded graphs with combinatorial maps, which is layer 5.
- B. Mohar and C. Thomassen, *Graphs on Surfaces*, chapter 2. The scope statement for
  layer 10, and the route for Whitney and Mac Lane.
- C. Thomassen, *Kuratowski's theorem*, J. Graph Theory **5** (1981). The proof route for
  Kuratowski in layer 10.
- W. B. R. Lickorish, *A finite set of generators for the homeotopy group of a 2-manifold*,
  Proc. Cambridge Philos. Soc. **60** (1964). The generating set targeted in layer 9.
- B. Farb and D. Margalit, *A Primer on Mapping Class Groups*, chapters 1–4. The
  change-of-coordinates principle and the Lickorish induction as presented for a modern
  reader.
- J. R. Munkres, *Topology*, chapter 12. The classification via normal forms and
  abelianization, which is the route in layers 6 through 8 with the topology replaced by the
  Hauptvermutung.
- E. Moise, *Geometric Topology in Dimensions 2 and 3*, chapter 8, and A. Gallier and
  D. Xu, *A Guide to the Classification Theorem for Compact Surfaces*. The classification
  itself.