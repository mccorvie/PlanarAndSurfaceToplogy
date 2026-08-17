# Roadmap: Planar topology and the piecewise-linear structure of surfaces

Two-dimensional topology is the last place where the general machinery of geometric
topology is unconditionally true. Triangulations exist, the Hauptvermutung holds, every
embedded arc is tame, and the piecewise-linear and smooth categories agree with the
topological one. In dimension five triangulations can fail; in dimension four the
Hauptvermutung fails; in dimension three the horned sphere shows that local flatness is a
real hypothesis. This roadmap proves that none of that happens in dimension two.

That framing matters for how the results are used. The theorems here are not
specializations of a general theory that will one day subsume them. They are the theorems
saying that the general theory's hypotheses are free and its counterexamples do not occur,
so they can only ever be proved in dimension two. The *definitions* are a different
matter, and are shared with `GeometricTopology`; see the interface table below.

The summits, in order of construction:

1. **The Jordan curve theorem and the crosscut theorem**, with the frontier and
   accessibility API that `ConformalMapping` is currently blocked on.
2. **Radó's theorem**: every compact surface is triangulable.
3. **The two-dimensional Hauptvermutung**: any two triangulations of a compact surface have
   a common subdivision, so every combinatorial invariant is a topological one.
4. **The Euler characteristic** of a compact surface, well defined and a homeomorphism
   invariant.
5. **The Schoenflies theorem** in its polygonal, planar, spherical, and relative forms, and
   tameness of arcs and simple closed curves in a surface.
6. **Uniqueness of the piecewise-linear and smooth structures** on a compact surface.

The companion roadmap
[CombinatorialMaps](../CombinatorialMaps/README.md) consumes items 2, 3, and 4 and proves
the classification of compact surfaces. This roadmap is simplicial only: generalized maps,
hypermaps, cellulations, and polygon words appear nowhere in it.

---

## Relationship to other roadmaps

### This roadmap supplies

| Consumer | What it needs | Status there today |
|---|---|---|
| `ConformalMapping` layer 5 | plane separation for Jordan curves; `J ⊆ closure (filledHull J \ J)` | named as an open frontier item in `ConformalMapping/STATUS.md` and in the roadmap role of `TauCeti/Topology/FilledHull.lean` |
| `GeometricTopology` layer 11 | `IsTriangulable` is unconditional in dimension two; the two-dimensional Hauptvermutung | the layer states that triangulability fails in dimension five and that the Hauptvermutung is false in general, but does not currently claim dimension two |
| `GeometricTopology` layer 2 | `IsLocallyFlat` is automatic for arcs and simple closed curves in a surface | the predicate exists; the two-dimensional discharge does not |
| `GeometricTopology` layer 1 | invariance of domain in dimension two, hence chart-independence of the manifold boundary in the topological (`k = 0`) case | `isBoundaryPoint_iff_mem_frontier_range` carries the hypothesis `hk : k ≠ 0` |
| `CombinatorialMaps` | Radó, the Hauptvermutung, Euler characteristic, Schoenflies, tameness | not built |

### This roadmap consumes

Nothing here is rebuilt. Every item below is cited and used.

| Item | Location | Owner |
|---|---|---|
| `Isotopy`, `Isotopic` | `TauCeti/Topology/Homotopy/Isotopy/{Basic,Comp,Prod}.lean` | `GeometricTopology` layer 1 |
| `AmbientIsotopy`, `AmbientIsotopic` | `TauCeti/Topology/Homotopy/AmbientIsotopic/{Basic,Complement,Naturality}.lean` | `GeometricTopology` layer 1 |
| `Realization`, `Face`, `faceInclusion`, `StandardSimplex` | `TauCeti/AlgebraicTopology/SimplicialComplex/Realization.lean` | `GeometricTopology` layer 11 |
| `link`, `closedStar`, `deletion` | `TauCeti/AlgebraicTopology/SimplicialComplex/LinkStar.lean` | `GeometricTopology` layer 11 |
| barycentric subdivision, `barycentricSubdivisionMap` | `TauCeti/AlgebraicTopology/SimplicialComplex/Subdivision/{Basic,Realization}.lean` | `GeometricTopology` layer 11 |
| `SimplicialMap`, `domainRestrict` | `TauCeti/AlgebraicTopology/SimplicialComplex/Maps.lean` | `GeometricTopology` layer 11 |
| `dimension` | `TauCeti/AlgebraicTopology/SimplicialComplex/Dimension.lean` | `GeometricTopology` layer 11 |
| the collapse API | `TauCeti/AlgebraicTopology/SimplicialComplex/Collapse/`, `ElementaryCollapse.lean` | `GeometricTopology` layer 11 |
| `IsTriangulable` | `TauCeti/Topology/Triangulable.lean` | `GeometricTopology` layer 11 |
| `IsLocallyFlat`, `IsSliceChart`, `IsSliceEmbedding` | `TauCeti/Geometry/Manifold/LocallyFlat/{Basic,Smooth}.lean` | `GeometricTopology` layer 2 |
| half-space boundary model, boundary charts, collars | `TauCeti/Geometry/Manifold/Boundary/{Basic,Charts,Model}.lean`, `Boundary/Collar/{Basic,Chart}.lean` | `GeometricTopology` layer 1 |
| `IsJordanCurve` and the arc theory | `TauCeti/Topology/JordanCurve/{Basic,Path,Separation,SmallArc,Subcontinuum}.lean` | `ConformalMapping` |
| `filledHull` | `TauCeti/Topology/FilledHull.lean` | `ConformalMapping` |
| `IsJordanDomain` | `TauCeti/Analysis/Complex/Conformal/JordanDomain.lean` | `ConformalMapping` |
| winding numbers for contours | `TauCeti/Analysis/Contour/Winding/` | `ContourIntegration` |
| `Circle` arc and metric API | `TauCeti/Topology/Circle/{Basic,Arc,Metric}.lean` | `ConformalMapping` |
| continua, local connectedness | `TauCeti/Topology/{Continuum,LocallyConnected,UniformlyLocallyConnected}.lean` | `ConformalMapping` |

### This roadmap extends someone else's API

Four small additions to existing files. Each should land as its own pull request, before
the layer that needs it, and each should be discussed with the owning roadmap first.

- **Relative and supported isotopy** (`IsotopicRel`, ambient isotopy with prescribed
  support) into `TauCeti/Topology/Homotopy/`.
- **General subdivision** (a complex refining another with the same realization),
  stellar subdivision, and the common-subdivision theorem, into
  `TauCeti/AlgebraicTopology/SimplicialComplex/Subdivision/`.
- **Purity, facets, and skeleta** into `TauCeti/AlgebraicTopology/SimplicialComplex/`.
- **`IsPLMap`** in the same directory, defined by quantification over subdivisions.

### Cross-roadmap edits proposed alongside this PR

- `GeometricTopology` layer 11: a paragraph recording that the dimension-two cases are
  proved here; that they are theorems rather than specializations, since the Hauptvermutung
  is false in general; that if dimension-two specializations of `IsCombinatorialManifold`,
  subdivision, or regular neighbourhoods have landed from here, it is fine to replace them
  with general versions and rederive the two-dimensional cases; and that this roadmap
  consumes the layer's simplicial API rather than rebuilding it.
- `GeometricTopology` layer 9: a line recording that the classification of surfaces and the
  surface mapping class group, which the layer lists as inputs, are owned by
  [CombinatorialMaps](../CombinatorialMaps/README.md).
- `GeometricTopology` layer 2: a line recording that in dimension two `IsLocallyFlat` is
  automatic, proved here, giving an unconditional two-dimensional collaring theorem.
- `GeometricTopology` layer 1: a line recording that the topological case of the boundary
  characterization rests on invariance of domain, proved here in dimension two.
- `ConformalMapping` layer 5 and `STATUS.md`: replace the open frontier item with a pointer
  here, naming `J ⊆ closure (filledHull J \ J)` as the milestone.

---

## What is already there

Verified against the pinned toolchain: Lean `v4.34.0-rc1`, Mathlib `master` at
`05ae0103f49b1ad1248f6039bbbad43d8aeb52a9`.

**Present in Tau Ceti.** Everything in the consumption table above. Notably the simplicial
complex API is well past Mathlib's: realization into `ι →₀ ℝ` with the face topology,
star and link with the lattice lemmas, barycentric subdivision identified with the order
complex, simplicial maps with `FunLike`, dimension in `WithBot ℕ∞`, cones, joins, products,
and elementary collapse. The Jordan curve development is roughly 1100 lines and covers
cutting at one and two points, small-arc diameter bounds, and the classification of proper
subcontinua as arcs.

**Absent from Mathlib, verified by module-list grep at the pinned commit.**

- No **Brouwer fixed point theorem**. The only `FixedPoint` modules are order-theoretic,
  dynamical, and group-theoretic.
- No **invariance of domain** and no topological **degree theory**.
- No **space-filling curve**.
- Singular homology has only `SingularHomology.{Basic, HomologyZero, HomotopyInvariance,
  HomotopyInvarianceTopCat}`. **No excision, no Mayer–Vietoris, no homology of spheres.**
  This closes the homological route to the Jordan curve theorem; see "Routes considered and
  rejected".

**Absent from Tau Ceti.** No Jordan curve theorem, no Schoenflies theorem, no Radó, no
Hauptvermutung, no combinatorial manifolds, no general subdivision, no `IsPLMap`, no
regular neighbourhoods, no `PLGroupoid`.

## Check what is already in motion

Before claiming anything in layers 3 through 6, check the open pull requests labelled
`roadmap/GeometricTopology`, which are actively moving
`TauCeti/AlgebraicTopology/SimplicialComplex/`. The subdivision directory in particular is
a likely collision point. Search the Lean Zulip `Autoformalization` and `Is there code for
X?` channels for Jordan curve, Schoenflies, and simplicial complex threads, and check open
Mathlib pull requests for Brouwer and for invariance of domain before starting layer 2.

---

## Encoding conventions

These are decisions, not suggestions. Divergence from them will produce work that has to be
redone.

### The plane

**Planar topology is stated over `ℂ`.** `ConformalMapping` works over `ℂ`, `Circle` lives in
`ℂ`, the normal to a directed segment is `I * u`, and the exterior is handled by `z ↦ z⁻¹`.

**Chart-level results are stated over `EuclideanSpace ℝ (Fin 2)` and `EuclideanHalfSpace 2`**,
because that is Mathlib's `ModelWithCorners` vocabulary and there is no alternative.

Layer 0 fixes one named linear isometry equivalence `ℂ ≃ₗᵢ[ℝ] EuclideanSpace ℝ (Fin 2)`,
obtained from `Complex.orthonormalBasisOneI.repr`. It is an isometry, not merely a
homeomorphism, so every metric, diameter, and convexity statement transports for free.
**No statement is proved twice.** Headline theorems get both forms, one proved and one
transported.

Layer 0 also fixes the sup-norm comparison `‖x‖_∞ ≤ ‖x‖ ≤ √2 · ‖x‖_∞`, which is what
square-mesh arguments run on.

### The circle

`Circle`, the unit circle of `ℂ`, is the statement-level circle, because
`TauCeti.IsJordanCurve C` is defined as `Nonempty (C ≃ₜ Circle)`. Its group structure is
used when gluing two Schoenflies discs along a curve.

`AddCircle (1 : ℝ)` is the parametrization used whenever lifting, degree, or a linear order
on a lift is needed. `AddCircle.homeomorphCircle`, `Circle.exp`, and `sphereCircleHomeomorph`
are the bridges; layer 0 states the round trips.

⚠ `CircularOrder` on the topological circle has nothing to do with cyclic words. Keep them
in separate namespaces with no coercion between them.

### Surfaces

A **topological surface** is a `ChartedSpace (EuclideanSpace ℝ (Fin 2)) M` with `T2Space M`,
plus second countability where needed; a **surface with boundary** charts to
`EuclideanHalfSpace 2`. Compactness plus Hausdorff plus locally Euclidean gives second
countability, and layer 0 proves that rather than assuming it.

Manifold boundary `∂M` is the set of points that are boundary points in some chart.
⚠ Chart-independence in the topological case is **not** available from
`isBoundaryPoint_iff_mem_frontier_range`, which requires `k ≠ 0`. It follows from
invariance of domain, proved in layer 2. Nothing in this roadmap may use `∂M` before
layer 2 closes.

### Simplicial and PL

Abstract simplicial complexes are Tau Ceti's
(`PreAbstractSimplicialComplex` / `AbstractSimplicialComplex`, `SetLike`,
`Face K := {σ : Finset ι // σ ∈ K}`, realization into `ι →₀ ℝ`). No private complex type.

A **PL map** is defined by existential quantification over subdivisions of the domain
complex, not by privileging a triangulation. Composability then depends on the
common-subdivision theorem, which is a layer 3 target.

`IsCombinatorialSurface` is defined concretely by the link condition: every vertex link is a
combinatorial circle or a combinatorial arc, and every edge lies in one or two triangles.
It is *not* defined by instantiating a dimension-general recursion. If
`GeometricTopology` later builds `IsCombinatorialManifold`, the equality at `n = 2` is a
named compatibility theorem, listed in layer 3's targets.

### Bundled index types

Every finite combinatorial object bundles its own index type, and every move relation is
closed under isomorphism of that type before `Relation.ReflTransGen` is applied. This is
stated here because it is shared with `CombinatorialMaps`; the reason is in layer 6.

---

## Structure

After layer 1 the roadmap splits into two tracks that can be driven in parallel by
different contributors and rejoin at layer 8.

```
L0 conventions
 |
L1 polygonal engine
 |\
 | \____ Track I  (approximation and separation)
 |        L2 separation, crosscuts, invariance of domain   -> unblocks ConformalMapping
 |        L7 topological Schoenflies and tameness
 |
 \______ Track II (piecewise-linear)
          L3 simplicial and PL toolkit in dimension two
          L4 the PL approximation theorem
          L5 Rado and the two-dimensional Hauptvermutung
          L6 Euler characteristic
                     |
                     L8 uniqueness of PL and smooth structure  (joins I and II)
```

---

## Layer 0: Conventions, transport, and plane pathologies

Small, entirely mechanical, and worth landing first so that nothing downstream invents its
own version. It also contains two constructions whose purpose is to certify that later
hypotheses are sharp.

**From Mathlib and Tau Ceti.** `Complex.orthonormalBasisOneI`, `OrthonormalBasis.repr`,
`Circle`, `AddCircle`, `AddCircle.homeomorphCircle`, `Circle.exp`, `sphereCircleHomeomorph`,
`Isotopy`, `Isotopic`, `AmbientIsotopic`, `Continuum`, `LocallyConnected`.

**Illustrative targets.**

```lean
/-- The plane transport. An isometry, so metric statements move for free. -/
noncomputable def planeEquiv : ℂ ≃ₗᵢ[ℝ] EuclideanSpace ℝ (Fin 2) :=
  Complex.orthonormalBasisOneI.repr

def perp (u : ℂ) : ℂ := Complex.I * u
def det (u v : ℂ) : ℝ := (starRingEnd ℂ u * v).im

theorem det_eq_inner_perp (u v : ℂ) : det u v = inner ℝ (perp u) v
theorem norm_inf_le_norm (z : ℂ) : ‖z‖_∞ ≤ ‖z‖
theorem norm_le_sqrt_two_norm_inf (z : ℂ) : ‖z‖ ≤ Real.sqrt 2 * ‖z‖_∞

/-- Compact plus Hausdorff plus locally Euclidean gives second countable. -/
instance (priority := low) {M : Type*} [TopologicalSpace M] [CompactSpace M] [T2Space M]
    [ChartedSpace (EuclideanSpace ℝ (Fin 2)) M] : SecondCountableTopology M

/-- Isotopy relative to a subset. Extends the existing isotopy API; land upstream first. -/
def IsotopicRel (A : Set X) (f g : C(X, Y)) : Prop

/-- Plane pathologies: these certify that later hypotheses cannot be weakened. -/
theorem exists_spaceFillingCurve : ∃ f : C(unitInterval, ℂ), Surjective f ∧
    Set.range f = Metric.closedBall 0 1
theorem exists_jordanCurve_volume_pos : ∃ J : Set ℂ, IsJordanCurve J ∧ 0 < volume J
```

**Design notes.**

- The two pathologies are not decoration. The space-filling curve is why the Jordan curve
  theorem needs injectivity to do all the work, and the positive-measure (Osgood) curve is
  why no route may assume the curve is rectifiable, small, or null. Layer 1's design note on
  approximating the crossing paths rather than the curve refers back to the second one.
- ⚠ The Osgood construction is a self-similar slab removal and its cost has not been
  measured. If it proves expensive, it may be deferred to layer 7 without blocking anything.

**Acceptance criteria.** `planeEquiv` round-trips are proved both ways and at least one
metric statement is shown transporting through it. The `Circle` and `AddCircle` round trips
are proved. The space-filling curve is exhibited as a concrete function, not merely
asserted to exist.

**Claimable units.** (i) plane transport and the `perp`/`det` API; (ii) circle transport;
(iii) second countability and surface conventions; (iv) relative and supported isotopy,
landed upstream; (v) the space-filling curve; (vi) the Osgood curve.

---

## Layer 1: The polygonal engine

Everything in dimension two that is true for elementary reasons. This layer is entirely
finite and elementary, needs nothing but layer 0, and produces the identity that both
tracks run on.

The organizing object is the **crossing parity** of a point against a closed polygon,
and the identity that makes the whole theory work is the additive one: cutting a polygonal
Jordan curve by a polygonal crosscut splits it into two polygonal Jordan curves whose
parities add.

**From layer 0.** The plane, `perp`, `det`, the sup-norm comparison.
**From Tau Ceti.** `TauCeti/Analysis/Contour/Winding/` for the comparison with the analytic
winding number; `Combinatorics/SimpleGraph/Acyclic` for the graph-theoretic pieces.

**Illustrative targets.**

```lean
structure PolygonalPath where
  vertices : List ℂ
  ne_nil : vertices ≠ []

def PolygonalPath.IsSimpleClosed (P : PolygonalPath) : Prop
def crossingParity (P : PolygonalPath) (q : ℂ) : ZMod 2
def polygonalWinding (P : PolygonalPath) (q : ℂ) : ℤ

theorem crossingParity_locallyConstant (P : PolygonalPath) (hP : P.IsSimpleClosed) :
    IsLocallyConstant (fun q : (P.carrier)ᶜ => crossingParity P q)

/-- Polygonal Jordan separation. -/
theorem polygonal_jordan (P : PolygonalPath) (hP : P.IsSimpleClosed) :
    Nat.card (ConnectedComponents ((P.carrier)ᶜ : Set ℂ)) = 2

/-- The identity everything runs on. -/
theorem polygonal_crosscut_parity (P : PolygonalPath) (hP : P.IsSimpleClosed)
    (Q : PolygonalCrosscut P) :
    crossingParity P = crossingParity (P.splitLeft Q) + crossingParity (P.splitRight Q)

/-- Two ears. -/
theorem exists_ear (P : PolygonalPath) (hP : P.IsSimpleClosed) (h : 3 < P.vertices.length) :
    ∃ i, IsEar P i

/-- Polygonal Schoenflies. The closed inside of a simple polygon is PL-homeomorphic
    to a triangle, and the homeomorphism extends to the plane. -/
theorem polygonal_schoenflies (P : PolygonalPath) (hP : P.IsSimpleClosed) :
    ∃ h : ℂ ≃ₜ ℂ, IsPLHomeomorph h ∧ h '' P.carrier = standardTriangle.boundary

/-- The lever for layer 2. -/
theorem not_planar_K33 : ¬ ∃ e : PlanarEmbedding (completeBipartiteGraph 3 3), True
```

**Design notes.**

- Crossing parity, not the winding number, is the primitive. It is `ZMod 2`-valued, it is
  decidable, and the crosscut identity is additive in it. The integer winding number and the
  comparison with `TauCeti/Analysis/Contour/Winding/` are corollaries.
- ⚠ Do not restrict to lattice polygons. `rkirov/jordan_pick` proves crossing parity and
  ear-clipping for `LatticePolygon`, and the arguments transfer, but you cannot inscribe a
  lattice polygon in an arbitrary Jordan curve, so the lattice version cannot serve layer 2.
  Re-derive at general-vertex generality.
- Nonplanarity of `K₃,₃` is a **finite** statement provable from the polygonal crosscut
  theorem alone. It is the lever that takes layer 2 from polygons to arbitrary curves, and
  it is also consumed by `CombinatorialMaps` layer 10. Do not skip it as a curiosity.
- Ear-clipping is the engine of polygonal Schoenflies: triangulate a simple polygon by
  diagonals, then induct.

**Acceptance criteria.** `crossingParity` is `Decidable` and evaluates correctly on a
worked example with a non-convex polygon and a point in a re-entrant pocket.
`polygonal_jordan` is checked against a polygon whose complement is *not* two components
after removing the simplicity hypothesis (a figure eight), confirming the hypothesis does
work. `exists_ear` fails for a triangle (the hypothesis `3 < length` is not decorative) and
is checked on a polygon with exactly two ears and no more.

**Claimable units.** (i) polygonal paths, carriers, and simplicity; (ii) strips and the
two-sidedness lemma; (iii) crossing parity, local constancy, and the integer winding number;
(iv) polygonal Jordan separation; (v) the polygonal crosscut theorem; (vi) two ears and
polygon triangulation; (vii) polygonal Schoenflies; (viii) nonplanarity of `K₃,₃`.

**Unlocks.** Both tracks. Unit (viii) also unlocks `CombinatorialMaps` layer 10.

---

## Layer 2: Separation, crosscuts, and invariance of domain

The layer that discharges `ConformalMapping`'s open frontier item.

⚠ **The milestone of this layer is the crosscut theorem, not the component count.** The
component count is the least useful consequence of separation and is proved last, as a
corollary. A development that proves only `Nat.card (ConnectedComponents Jᶜ) = 2` does not
discharge this layer: it supplies nothing that `ConformalMapping`'s length-area argument or
this roadmap's layer 7 actually spend.

**From layers 0 and 1.** The polygonal crosscut theorem and nonplanarity of `K₃,₃`.
**From Tau Ceti.** `IsJordanCurve` and the arc theory in `TauCeti/Topology/JordanCurve/`;
`filledHull`; `IsJordanDomain`; `TauCeti/Topology/{Continuum,LocallyConnected}.lean`.

**Illustrative targets.**

```lean
namespace IsJordanCurve
variable {J : Set ℂ} (hJ : IsJordanCurve J)

def inside  : Set ℂ := filledHull J \ J
def outside : Set ℂ := (filledHull J)ᶜ

theorem isConnected_inside      : IsConnected hJ.inside
theorem isConnected_outside     : IsConnected hJ.outside
theorem isBounded_inside        : Bornology.IsBounded hJ.inside
theorem not_isBounded_outside   : ¬ Bornology.IsBounded hJ.outside
theorem frontier_inside         : frontier hJ.inside = J
theorem frontier_outside        : frontier hJ.outside = J

/-- The statement named in the roadmap role of `TauCeti/Topology/FilledHull.lean`
    and in `ConformalMapping/STATUS.md`. -/
theorem subset_closure_inside   : J ⊆ closure (filledHull J \ J)

theorem dense_accessible : Dense {p ∈ J | hJ.Accessible p}

/-- The workhorse. -/
theorem crosscut (P : Set ℂ) (hP : hJ.IsCrosscut P) :
    ∃ J₁ J₂, IsJordanCurve J₁ ∧ IsJordanCurve J₂ ∧
      J₁ ∩ J₂ = P ∧ J₁ ∪ J₂ = J ∪ P ∧
      hJ.inside \ P = insideOf J₁ ∪ insideOf J₂ ∧ Disjoint (insideOf J₁) (insideOf J₂)

/-- The corollary, including the shape used by lean-eval. -/
theorem card_connectedComponents : Nat.card (ConnectedComponents ((J : Set ℂ)ᶜ)) = 2
end IsJordanCurve

/-- An arc does not separate. -/
theorem arc_not_separates (A : Set ℂ) (hA : IsArc A) : IsConnected (Aᶜ)

/-- Not in Mathlib at the pinned commit. -/
theorem invarianceOfDomain₂ {U : Set ℂ} (hU : IsOpen U) (f : C(U, ℂ)) (hf : Injective f) :
    IsOpen (Set.range f) ∧ IsOpenMap f
```

**Design notes.**

- The route is Thomassen's. Approximate the paths that would have to cross the curve, not
  the curve itself. A curve can have positive measure (layer 0), so polygonal approximation
  of the curve with control on the complementary regions is where the classical routes
  bleed; the crossing paths are compact arcs in an open set and are cheap to control.
  Nonplanarity of `K₃,₃` converts a hypothetical failure of separation, or a third
  complementary component, into a plane drawing of `K₃,₃`.
- Arc non-separation is proved by the chain-of-small-squares argument and needs the small-arc
  diameter bounds already in `TauCeti/Topology/JordanCurve/SmallArc.lean`.
- ⚠ `invarianceOfDomain₂` is on the critical path, not a side branch. Without it `∂M` is not
  known to be chart-independent for a topological surface, and the classification with
  boundary quantifies over boundary components. Mathlib has neither invariance of domain nor
  degree theory at the pinned commit.
- The Maehara route (Jordan from Brouwer, as in `rkirov/jordan_pick`) is retained as an
  **independent second proof** of `card_connectedComponents`, in the same file, with a
  comment recording that the two proofs share no lemmas. It is a non-vacuity cross-check.
  It is not the primary route, because it produces no frontier statement, no accessibility,
  and no crosscut theorem. Brouwer itself is absent from Mathlib and is worth upstreaming
  independently.

**Acceptance criteria.** `subset_closure_inside` is stated in exactly the form named in
`TauCeti/Topology/FilledHull.lean`. A `IsJordanDomain` is constructed from an arbitrary
`IsJordanCurve`, which is currently impossible: at the pinned commit the only constructors
are from a ball, from a convex set, and by transport along a map. The two proofs of
`card_connectedComponents` are checked to have disjoint dependency cones. `crosscut` is
checked on a worked example where the two pieces are visibly different (an off-centre
chord of a disc).

**Claimable units.** (i) `inside`, `outside`, boundedness, and connectedness; (ii) the
common frontier; (iii) arc non-separation; (iv) accessibility and its density; (v) the
crosscut theorem; (vi) the component count and the Maehara cross-check;
(vii) `invarianceOfDomain₂` and chart-independence of `∂M`.

**Unlocks.** `ConformalMapping` layer 5. Layer 7. `∂M` for the rest of this roadmap.

---

## Layer 3: The simplicial and piecewise-linear toolkit in dimension two

Everything the approximation theorem needs, built on Tau Ceti's existing complex API rather
than beside it.

**From Tau Ceti.** `AbstractSimplicialComplex`, `Realization`, `link`, `closedStar`,
`deletion`, barycentric subdivision, `SimplicialMap`, `dimension`, the collapse API,
`IsTriangulable`.

**Illustrative targets.**

```lean
/-- Extends the existing subdivision directory. Land upstream. -/
def Subdivides (K' K : AbstractSimplicialComplex ι) : Prop
theorem Subdivides.realization_homeomorph (h : Subdivides K' K) :
    Realization K' ≃ₜ Realization K
def stellarSubdivision (K : AbstractSimplicialComplex ι) (σ : K.Face) :
    AbstractSimplicialComplex (Option ι)

/-- Common subdivision for a fixed polyhedron. Genuinely nontrivial; distinct from the
    Hauptvermutung, which is about an arbitrary homeomorphism. -/
theorem exists_common_subdivision (K L : AbstractSimplicialComplex ι)
    (h : Realization K ≃ₜ Realization L) (hid : /- the homeomorphism is the identity
      on the underlying polyhedron -/) : ∃ M, Subdivides M K ∧ Subdivides M L

def IsPLMap (f : Realization K → Realization L) : Prop :=
  ∃ K' L' (_ : Subdivides K' K) (_ : Subdivides L' L) (g : SimplicialMap K' L'), /- ... -/

theorem IsPLMap.comp : IsPLMap f → IsPLMap g → IsPLMap (g ∘ f)

/-- Concrete, not an instance of a dimension-general recursion. -/
def IsCombinatorialSurface (K : AbstractSimplicialComplex ι) : Prop :=
  K.dimension = 2 ∧ K.IsPure ∧
    (∀ e ∈ K.faces, e.card = 2 → 1 ≤ (K.trianglesContaining e).card ∧
       (K.trianglesContaining e).card ≤ 2) ∧
    (∀ v ∈ K.vertices, IsCombinatorialCircle (K.link v) ∨ IsCombinatorialArc (K.link v))

def IsClosedCombinatorialSurface (K : AbstractSimplicialComplex ι) : Prop :=
  IsCombinatorialSurface K ∧ ∀ v ∈ K.vertices, IsCombinatorialCircle (K.link v)

theorem IsCombinatorialSurface.realization_isSurface (h : IsCombinatorialSurface K) :
    IsTopologicalSurfaceWithBoundary (Realization K)

/-- Compatibility, for when `GeometricTopology` builds the general recursion. -/
theorem isCombinatorialSurface_iff_isCombinatorialManifold_two :
    IsCombinatorialSurface K ↔ IsCombinatorialManifold 2 K
```

**Design notes.**

- ⚠ The common-subdivision theorem for a *fixed* polyhedron (layer 3) and the Hauptvermutung
  for an *arbitrary* homeomorphism (layer 5) are different theorems with very different
  costs. Most treatments blur them. Keep them apart.
- The compatibility theorem is stated now and proved later, when and if the general recursion
  exists. Until then it is a `sorry`-free absence, not a `sorry`.
- Combinatorial circles and arcs in dimension one are concrete: a combinatorial circle is a
  connected 1-complex in which every vertex has exactly two neighbours; a combinatorial arc
  is one in which exactly two vertices have one neighbour and the rest have two. Do not
  route these through a general sphere-recognition recursion.

**Acceptance criteria.** `IsCombinatorialSurface` is **false** for each of: two triangles
sharing exactly one vertex; three triangles sharing an edge; the dunce hat; the cone on a
theta graph. It is **true** for: the boundary of the tetrahedron; a triangulated disc; a
triangulated Möbius band. Every one of these is a concrete finite complex checked by
`decide`. ⚠ This is the layer where vacuity is a real risk, because pinch points genuinely
are possible for abstract complexes.

**Claimable units.** (i) general subdivision and realization invariance, landed upstream;
(ii) stellar subdivision; (iii) purity, facets, skeleta, landed upstream; (iv) the common
subdivision theorem; (v) `IsPLMap` and its closure properties; (vi) combinatorial circles
and arcs; (vii) `IsCombinatorialSurface` and the counter-witness battery;
(viii) the realization theorem.

**Unlocks.** Layers 4, 5, 6. `CombinatorialMaps` layer 3.

---

## Layer 4: The piecewise-linear approximation theorem

One hard theorem, which powers both of the next two layers. Moise's Theorem 6.3/6.4: a
homeomorphism between triangulated 2-manifolds can be approximated, within any prescribed
epsilon, by a piecewise-linear homeomorphism.

This is the expensive, unglamorous engine of the roadmap. It is also the piece with no
prior art in any proof assistant.

**From layers 1 and 3.** Polygonal Schoenflies, general position for polygonal arcs,
subdivision, `IsPLMap`.

**Illustrative targets.**

```lean
theorem exists_pl_approximation {K L : AbstractSimplicialComplex ι}
    (hK : IsCombinatorialSurface K) (hL : IsCombinatorialSurface L)
    (f : Realization K ≃ₜ Realization L) (ε : ℝ) (hε : 0 < ε) :
    ∃ g : Realization K ≃ₜ Realization L, IsPLMap g ∧ ∀ x, dist (f x) (g x) < ε

/-- The relative form, needed for the boundary case. -/
theorem exists_pl_approximation_rel {A : Set (Realization K)}
    (hA : IsSubcomplexCarrier A) (hf : IsPLMap (f.restrict A)) (ε : ℝ) (hε : 0 < ε) :
    ∃ g, IsPLMap g ∧ g.restrict A = f.restrict A ∧ ∀ x, dist (f x) (g x) < ε
```

**Design notes.**

- The proof is a mesh-refinement argument: subdivide finely enough that images of simplices
  lie in small discs, then correct on the 0-skeleton, the 1-skeleton, and the 2-skeleton in
  turn, using polygonal Schoenflies to fill in each triangle. Layer 1's ear-clipping is what
  fills the triangles.
- ⚠ The relative form is not an afterthought. Layer 5's boundary case and layer 8's isotopy
  statement both need it, and retrofitting relativity to a completed absolute proof is
  expensive. Build it relative from the start.
- Local flatness is not a hypothesis here, because layer 7 shows it is automatic. If layer 7
  has not landed, this layer may temporarily assume tameness of the relevant arcs; record
  that as an explicit assumption rather than a silent one.

**Acceptance criteria.** The approximation is exhibited on a worked example where the
original homeomorphism is not piecewise linear (a radial map with a non-linear radial
profile). The epsilon is checked to be achievable for arbitrarily small values, that is,
the statement is not accidentally vacuous by allowing `g = f`.

**Claimable units.** (i) general position for polygonal arcs in a triangulated surface;
(ii) the mesh-refinement construction; (iii) correction on the 1-skeleton;
(iv) filling triangles by polygonal Schoenflies; (v) the relative form.

**Unlocks.** Layers 5 and 8.

---

## Layer 5: Radó's theorem and the two-dimensional Hauptvermutung

The two theorems that make combinatorial invariants topological.

⚠ **Neither of these depends on the Schoenflies theorem.** Moise says so in print,
immediately after his triangulation theorem: the usual derivation from Schoenflies "is in a
way misleading", since in dimension three Schoenflies fails and triangulation still holds.
Cannon takes the other route and assumes Schoenflies plus the arc-crossing lemma. This
roadmap follows Moise. Layer 7 comes after layer 5 and is not an input to it.

**From layers 3 and 4.**

**Illustrative targets.**

```lean
/-- Rado. -/
theorem compact_surface_isTriangulable (M : Type*) [TopologicalSpace M] [CompactSpace M]
    [T2Space M] [ChartedSpace (EuclideanSpace ℝ (Fin 2)) M] : IsTriangulable M

theorem compact_surfaceWithBoundary_isTriangulable (M : Type*) [TopologicalSpace M]
    [CompactSpace M] [T2Space M] [ChartedSpace (EuclideanHalfSpace 2) M] :
    ∃ K, IsCombinatorialSurface K ∧ Nonempty (Realization K ≃ₜ M)

/-- The two-dimensional Hauptvermutung. -/
theorem hauptvermutung₂ {K L : AbstractSimplicialComplex ι}
    (hK : IsCombinatorialSurface K) (hL : IsCombinatorialSurface L)
    (h : Realization K ≃ₜ Realization L) :
    ∃ M, Subdivides M K ∧ Subdivides M L

/-- The consequence that makes it useful. -/
theorem hauptvermutung₂_isPL {K L} (hK : IsCombinatorialSurface K)
    (hL : IsCombinatorialSurface L) (h : Realization K ≃ₜ Realization L) :
    ∃ g : Realization K ≃ₜ Realization L, IsPLMap g ∧ IsPLMap g.symm
```

**Design notes.**

- The route to Radó covers the surface by finitely many closed discs, replaces boundary
  curves by polygonal approximations inside neighbouring charts, and assembles a complex.
  Layer 4's approximation theorem is what makes the replacement legal.
- The collar theorem is the lever for the boundary case: push `∂M` inward along a collar,
  triangulate the interior, and extend combinatorially. Check whether
  `TauCeti/Geometry/Manifold/Boundary/Collar/` is usable at the topological level; if not,
  the topological two-dimensional collar is a target here and a contribution back to
  `GeometricTopology` layer 1.
- Locally finite triangulations of second-countable noncompact surfaces are built only
  insofar as the compact proof needs them, and the general noncompact Radó is out of scope.

**Acceptance criteria.** A triangulation is produced for a surface presented only by charts,
with no combinatorial data supplied. `hauptvermutung₂` is checked on two visibly different
triangulations of the sphere (the tetrahedron boundary and the octahedron boundary), with a
common subdivision exhibited.

**Claimable units.** (i) finite chart covers and shrinking; (ii) polygonal replacement of
chart boundaries; (iii) assembly and Radó, closed case; (iv) the topological collar in
dimension two; (v) Radó with boundary; (vi) the Hauptvermutung.

**Unlocks.** Layer 6, layer 8, `CombinatorialMaps` throughout, `GeometricTopology` layer 11.

---

## Layer 6: The Euler characteristic

The summit of the piecewise-linear track, and the invariant that
[CombinatorialMaps](../CombinatorialMaps/README.md) builds on.

**Illustrative targets.**

```lean
def AbstractSimplicialComplex.eulerChar (K : AbstractSimplicialComplex ι) [Finite K.faces] : ℤ :=
  ∑ i, (-1 : ℤ) ^ i * (K.facesOfDim i).card

theorem eulerChar_subdivision (h : Subdivides K' K) : K'.eulerChar = K.eulerChar
theorem eulerChar_simplicialIso (h : K ≃ₛ L) : K.eulerChar = L.eulerChar

/-- Well defined by Rado, invariant by the Hauptvermutung. -/
noncomputable def Surface.eulerChar (M : Type*) [CompactSurface M] : ℤ

theorem Surface.eulerChar_congr {M N} [CompactSurface M] [CompactSurface N] (h : M ≃ₜ N) :
    Surface.eulerChar M = Surface.eulerChar N

theorem eulerChar_sphere : Surface.eulerChar (Metric.sphere (0 : EuclideanSpace ℝ (Fin 3)) 1) = 2
theorem eulerChar_disc  : Surface.eulerChar (Metric.closedBall (0 : ℂ) 1) = 1
```

**Design notes.**

- ⚠ Do not prove topological invariance of the cell count by appealing to the
  classification of surfaces. That is circular: the classification consumes the Euler
  characteristic. Invariance comes from subdivision invariance plus the Hauptvermutung, and
  from nothing else.
- Define `eulerChar` on complexes first, then transport. The surface-level definition is
  `Classical.choice` over triangulations plus the invariance theorem.
- **This is the general principle of both roadmaps.** Every invariant is defined on a finite
  presentation, proved invariant under the elementary moves, and upgraded to a homeomorphism
  invariant by the Hauptvermutung. No comparison theorem with Mathlib's singular homology or
  fundamental group is a target here or in `CombinatorialMaps`, and none is needed. It is
  also not currently provable: Mathlib's singular homology has no excision and no
  Mayer–Vietoris at the pinned commit.

**Acceptance criteria.** `eulerChar` is `Decidable` on concrete complexes and computes 2 for
the tetrahedron boundary, 2 for the octahedron boundary, 1 for a triangulated disc, 0 for a
triangulated annulus, 0 for a triangulated Möbius band. `eulerChar_subdivision` is checked
against a barycentric subdivision of the tetrahedron boundary, where the face counts change
and the alternating sum does not.

**Claimable units.** (i) `eulerChar` on complexes and its decidability; (ii) subdivision
invariance; (iii) isomorphism invariance; (iv) the surface-level definition and transport;
(v) the worked example table.

**Unlocks.** `CombinatorialMaps` layers 1, 5, 7, 8, 10.

---

## Layer 7: The Schoenflies theorem and tameness

The celebrated theorem, and the reason `GeometricTopology` layer 2's local flatness
hypothesis is free in dimension two.

This layer depends only on layers 0 through 2 and may be driven in parallel with layers 3
through 6.

**Illustrative targets.**

```lean
theorem schoenflies {J : Set ℂ} (hJ : IsJordanCurve J) :
    ∃ h : ℂ ≃ₜ ℂ, h '' J = Metric.sphere 0 1

theorem schoenflies_closure {J : Set ℂ} (hJ : IsJordanCurve J) :
    Nonempty (closure hJ.inside ≃ₜ Metric.closedBall (0 : ℂ) 1)

theorem schoenflies_sphere {S : Type*} [Sphere2 S] {J : Set S} (hJ : IsJordanCurve J) :
    ∃ h : S ≃ₜ S, h '' J = equator

/-- Tameness. The discharge of `GeometricTopology` layer 2's hypothesis. -/
theorem isLocallyFlat_of_isArc_surface {M} [Surface M] {A : Set M} (hA : IsArc A) :
    IsLocallyFlat A
theorem isLocallyFlat_of_isJordanCurve_surface {M} [Surface M] {J : Set M}
    (hJ : IsJordanCurve J) : IsLocallyFlat J

/-- Unconditional two-dimensional collaring, as a corollary. -/
theorem exists_collar_of_isJordanCurve_surface {M} [Surface M] {J : Set M}
    (hJ : IsJordanCurve J) : ∃ U, IsOpen U ∧ J ⊆ U ∧ Nonempty (U ≃ₜ J × Ioo (-1 : ℝ) 1)
```

**Design notes.**

- The route builds no homeomorphism directly. It builds two matched finite cell
  decompositions, one of the closed inside and one of the closed square, isomorphic as
  cellulations and agreeing with a chosen boundary correspondence; refines them alternately
  so that cells become small on both sides; and reads the homeomorphism off the nested
  closed stars. Every finite stage is a plane graph governed by layer 2's crosscut theorem.
  No polygonal approximation of the curve appears anywhere.
- ⚠ The matched cellulations here are a device internal to this proof. They are **not** the
  `Cellulation2` of `CombinatorialMaps`, they carry no realization theory, and they should
  live in a private namespace. Do not attempt to unify them.
- Record the alternative derivation as a design note and a cross-check, not as the primary
  route: once `ConformalMapping` layer 5 closes (which this roadmap's layer 2 unblocks),
  Carathéodory's theorem gives `closure hJ.inside ≃ₜ closed disc` from `riemannMapping`, and
  gluing the inside and outside discs along `J` gives `schoenflies` again. That derivation
  creates a roadmap-level cycle and must not be the primary proof, but two proofs agreeing
  is a strong non-vacuity check.

**Acceptance criteria.** `schoenflies_closure` is instantiated on a curve that is not
rectifiable. `isLocallyFlat_of_isArc_surface` is checked against `GeometricTopology`'s
actual `IsLocallyFlat` predicate, not a local restatement. A note records that the
corresponding statement is **false** in dimension three, citing the Alexander horned sphere,
so that the two-dimensional statement is visibly not a triviality.

**Claimable units.** (i) `IsCrosscut` and finite cellulations of a Jordan domain;
(ii) matching and refinement transfer; (iii) alternation and the small-cell estimate;
(iv) the nested-star limit and `schoenflies_closure`; (v) the ambient and spherical forms;
(vi) tameness of arcs; (vii) tameness of simple closed curves and the collar corollary.

**Unlocks.** `GeometricTopology` layer 2. `CombinatorialMaps` layer 9 (cutting along
curves). Layer 8.

---

## Layer 8: Uniqueness of the piecewise-linear and smooth structures

Where the two tracks rejoin.

**Illustrative targets.**

```lean
/-- Every compact surface has an essentially unique PL structure. -/
theorem unique_pl_structure {M} [CompactSurface M] :
    ∃! s : PLStructure M, True   -- up to PL isomorphism; see design notes

/-- Every homeomorphism of compact surfaces is isotopic to a PL homeomorphism. -/
theorem exists_isotopic_plHomeomorph {M N} [CompactSurface M] [CompactSurface N]
    (f : M ≃ₜ N) : ∃ g : M ≃ₜ N, IsPLMap g ∧ Isotopic f g

/-- Top and PL homeomorphism classes agree. -/
theorem homeomorph_iff_plHomeomorph {K L} (hK : IsCombinatorialSurface K)
    (hL : IsCombinatorialSurface L) :
    Nonempty (Realization K ≃ₜ Realization L) ↔ Nonempty (Realization K ≃ₚₗ Realization L)

/-- Every PL surface carries a compatible smooth structure, unique up to diffeomorphism. -/
theorem exists_unique_smoothing {K} (hK : IsCombinatorialSurface K) : ...
```

**Design notes.**

- Smoothing in dimension two is Whitehead's theorem, and the route is corner-rounding on a
  triangulation. The uniqueness half needs the relative approximation theorem from layer 4.
- ⚠ The mapping class group comparison (topological, PL, and smooth mapping class groups
  agree) belongs to `CombinatorialMaps` layer 9, not here, because that is where the groups
  are defined. This layer supplies the isotopy statement it consumes.
- "Essentially unique" must be spelled out rather than left to `∃!`. State it as: any two
  combinatorial surfaces with homeomorphic realizations have a common subdivision, and any
  two smoothings are related by a diffeomorphism isotopic to the identity.

**Acceptance criteria.** The statement is checked to be non-vacuous by exhibiting two
genuinely different triangulations of the same surface and producing the PL homeomorphism.
A note records that the corresponding uniqueness fails in dimension four, so the statement
is visibly dimension-specific.

**Claimable units.** (i) uniqueness of the PL structure; (ii) isotopy to a PL homeomorphism;
(iii) the Top/PL classification agreement; (iv) corner rounding and existence of a smoothing;
(v) uniqueness of the smoothing.

**Unlocks.** `CombinatorialMaps` layer 9.

---

## Routes considered and rejected

Recording these so that a contributor does not rediscover them.

**The homological route to the Jordan curve theorem** (Cannon chapter 7: singular homology
of sphere complements, then arc non-separation and Jordan). **Closed.** At the pinned
commit Mathlib's singular homology consists of `Basic`, `HomologyZero`, and
`HomotopyInvariance` only. There is no excision, no Mayer–Vietoris for singular homology,
and no homology of spheres. Taking this route would require building a substantial piece of
algebraic topology first, which is outside this roadmap and outside `GeometricTopology`.

**The Kline sphere characterization route to Schoenflies** (Cannon chapter 8: Schoenflies as
a corollary of a topological recognition theorem for the 2-sphere). **Rejected on cost.**
It is the more illuminating route and it is substantially more expensive, requiring Peano
continua, upper semicontinuous decompositions, and Bing's proof. See the
roadmap-for-a-roadmap below.

**Maehara's route to the Jordan curve theorem** (Jordan from the Brouwer fixed point
theorem). **Retained as a second proof, not as the primary route.** It is the shortest proof
of the component count that exists, and `rkirov/jordan_pick` closes it in roughly 2,600
lines. It supplies no frontier statement, no accessibility, and no crosscut theorem, so it
discharges nothing downstream.

**Deriving Schoenflies from Carathéodory.** **Recorded as a cross-check.** It creates a
roadmap-level cycle through `ConformalMapping` and would make a headline theorem hostage to
another roadmap's schedule.

---

## Roadmap-for-a-roadmap: continuum theory and wild plane topology

This section is motivation for a separate future roadmap. It is **not** work in this
roadmap, and contributors should not claim or implement tasks from it here.

The point-set topology of compact connected metric spaces is a subject in its own right,
with named theorems and essentially no Lean prior art, and it is the natural home for the
*wild* side of plane topology that `GeometricTopology` layer 2 quantifies over. A roadmap
for it would build: continua and Peano continua; the boundary-bumping lemma;
**Hahn–Mazurkiewicz** (a space is a continuous image of the interval exactly when it is a
Peano continuum); characterizations of the arc and the simple closed curve; **Brouwer's
characterization of the Cantor set**; upper semicontinuous decompositions and decomposition
spaces; **R. L. Moore's decomposition theorem**; the **Kline sphere characterization**
(conjectured by Kline, proved by Bing); and **tameness of Cantor sets in the plane**,
against Antoine's necklace as the dimension-three counterexample.

The principal reference is Cannon, *Topology as Fluid Geometry*, volume 2, chapters 5, 6, 8,
10, and 11.

Downstream consumers sit outside this roadmap. Moore's decomposition theorem is the
two-dimensional ancestor of the cell-like approximation theorems, and it is what makes
Douady's pinched-disc model of the Mandelbrot set a disc; locally connected models of the
Mandelbrot set are standard objects in complex dynamics. Brouwer's characterization of the
Cantor set is used throughout descriptive set theory and one-dimensional dynamics.

The present roadmap deliberately does **not** take the Cannon route to the Schoenflies
theorem. Anyone writing the continuum-theory roadmap should treat the Kline characterization
as a summit in its own right rather than as a path to Schoenflies, and may wish to prove
Schoenflies a second time from it as a cross-check against layer 7.

This section is written by someone who is not a continuum theorist. It needs an author who
is.

---

## Out of scope

- Anything in dimension three or higher. `GeometricTopology` owns it.
- The general (chart-based) `PLGroupoid`. `GeometricTopology` layer 1 owns it. This roadmap's
  PL language is subdivisions and simplicial maps.
- Combinatorial maps, hypermaps, generalized maps, cellulations, polygon words, the
  classification of surfaces, mapping class groups, graph embeddings.
  [CombinatorialMaps](../CombinatorialMaps/README.md) owns all of it.
- Riemann surfaces, conformal structure, uniformization, Carathéodory's theorem.
  `ConformalMapping` owns it. This roadmap supplies its missing input and consumes nothing
  analytic in return.
- Singular homology, Mayer–Vietoris, excision, van Kampen, cohomology, duality.
- Continuum theory beyond the local-connectedness lemmas actually consumed. See the
  roadmap-for-a-roadmap above.
- Noncompact surfaces beyond what the compact proofs require. The classification of
  noncompact surfaces (Kerékjártó) is a natural follow-on roadmap.
- Measure-theoretic properties of curves beyond the single Osgood example in layer 0.

---

## Provenance and prior art

**Disclosure.** The author of this roadmap is the author of
`mccorvie/classification-of-surfaces`, cited below. That repository is prior art, not a
specification. The mathematics here is stated independently and a contributor should be able
to implement every milestone without reading it.

| Development | Licence | Coordination | What it evidences |
|---|---|---|---|
| `mccorvie/classification-of-surfaces` | Apache-2.0 | authored by this roadmap's author | that the Moise route to Radó and the Hauptvermutung closes, at roughly 90k lines of PL infrastructure plus 70k for Schoenflies |
| `alonamaloh/schoenflies-lean`, blueprint `alonamaloh/jordan-schoenflies` | Apache-2.0 code, CC BY 4.0 blueprint | *[status to fill in]* | that the Thomassen route closes in 77,520 lines with no `sorry` beyond the lean-eval comparator hole, roughly 33k of it reaching the Jordan curve theorem |
| `rkirov/jordan_pick` | Apache-2.0 | *[agreement to fill in]* | Maehara's route to the component count, sorry-free at roughly 2,600 lines, plus Brouwer built from scratch, plus lattice-polygon crossing parity and ear-clipping |

**What the prior art gets wrong, and what this roadmap does differently.** The existing
surface-classification development carried, for a period, a set of definitions on the
polygonal-schema side that typechecked and were vacuous: `SurfaceCellModel`,
`OrientableRel`, `realization`, `gluingRel`, and `Equivalent` admitted degenerate witnesses,
and continuous integration was green throughout. That experience is why every validity
predicate in this roadmap and in `CombinatorialMaps` carries both a witness and a certified
counter-witness, and why realization is a construction with a proved comparison rather than
a structure field.

`schoenflies-lean` targets a single lean-eval statement rather than a reusable library and
exports almost no API. Its **route** is adopted here (layers 1, 2, and 7); its file
structure is not, and this roadmap does not propose porting it. `jordan_pick`'s polygonal
material is written for `LatticePolygon`, which cannot serve layer 2, since one cannot
inscribe a lattice polygon in an arbitrary Jordan curve; the arguments transfer and the code
does not. `jordan_pick`'s `Uniformization/` directory is a separate unfinished project with
`sorry`s and is not cited as complete.

**Integration.** Where existing code is adapted it will be re-derived against Tau Ceti's
vocabulary: `TauCeti.IsJordanCurve` rather than a parametrized definition, `ℂ` rather than
`EuclideanSpace ℝ (Fin 2)` for planar statements, and
`TauCeti/AlgebraicTopology/SimplicialComplex/` rather than a private complex type.

---

## How to drive it

Layers 0 and 1 are the only serial prefix. After layer 1 the roadmap forks into two tracks
that share nothing until layer 8, and each track has five to eight independent claimable
units per layer.

**Best first claims, in parallel, on day one:**

- Layer 0 units (i)–(iv). Mechanical, and everything else depends on them.
- Layer 1 units (i)–(iii). Finite, elementary, decidable, and testable by `decide`.
- The four upstream API extensions listed under "This roadmap extends someone else's API".
  These are small, they land in someone else's directory, and getting them merged early
  establishes that the roadmap's contributors can work inside existing conventions.

**Highest value per unit of effort:** layer 2 units (i)–(vi). They discharge an open
frontier item in an active roadmap, and the pull request should say so.

**Highest risk:** layer 3 unit (vii). `IsCombinatorialSurface` is where vacuity can hide,
because pinch points are genuinely possible for abstract complexes and green continuous
integration proves nothing about a definition. It should get the most reviewer attention and
the counter-witness battery should land in the same pull request as the definition.

**Longest pole:** layer 4. It is one theorem, it has no prior art in any proof assistant,
and layers 5, 6, and 8 all wait on it. Start it early even though it will finish late, and
build it relative from the start.

Signatures throughout are indicative and have not been elaborated. Extract them into
`Suggested.lean` with `sorry` bodies and fix the ones that do not typecheck before claiming
the corresponding unit.

---

## References

- C. Thomassen, *The Jordan–Schönflies theorem and the classification of surfaces*,
  Amer. Math. Monthly **99** (1992), 116–130. The route for layers 1, 2, and 7.
- `alonamaloh/jordan-schoenflies`, the blueprint. The only source cited here written at
  formalization granularity, with a statement-level index and a suggested module order.
  Treat it as the primary route document for layers 1, 2, and 7.
- E. Moise, *Geometric Topology in Dimensions 2 and 3*, chapters 1–9. The route for layers 3,
  4, 5, and 8. Note in particular his remark following the triangulation theorem that
  deriving it from Schoenflies "is in a way misleading".
- C. Rourke and B. Sanderson, *Introduction to Piecewise-Linear Topology*, chapters 1–3.
  The PL vocabulary, already followed by `TauCeti/AlgebraicTopology/SimplicialComplex/`.
- M. H. A. Newman, *Elements of the Topology of Plane Sets of Points*. Classical reference
  for the plane material.
- J. R. Munkres, *Elementary Differential Topology*; J. H. C. Whitehead, *On C¹-complexes*.
  The smoothing route for layer 8.
- J. Cannon, *Topology as Fluid Geometry*, volume 2. Exposition reference for the
  point-set lemmas in layer 0, and the reference for the roadmap-for-a-roadmap above. Note
  that it is an idiosyncratic tour rather than a reference text, and its selection principle
  is explicitly aesthetic; it should not be used to scope a roadmap.