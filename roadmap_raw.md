# Roadmap: planar and surface topology, two-dimensional PL topology, and combinatorial maps

This roadmap builds a Lean library for the topology of the plane and compact surfaces, together with the finite combinatorial models that make two-dimensional topology calculable.

Its organizing spine is the passage among four descriptions of the same objects:

\[
\boxed{
\text{topological surfaces}
\;\longleftrightarrow\;
\text{triangulated and PL surfaces}
\;\longleftrightarrow\;
\text{finite cellular surfaces}
\;\longleftrightarrow\;
\text{maps, hypermaps, and 2-Gmaps}.
}
\]

The transitions are themselves major theorems. They must be formalized with round-trip and realization-preservation results, not treated as informal changes of language.

The point-set-topological breadth and level are modeled on Chapters 5–16 of Cannon’s *Topology as Fluid Geometry: Two-Dimensional Spaces, Volume 2*: Peano continua; arcs, circles, and Cantor sets; Jordan separation and Schönflies; characterization of the 2-sphere; planar tameness; Moore’s decomposition theorem; two-dimensional invariance of domain; triangulation and classification of surfaces; curves on the torus; orientation; and Euler characteristic. Cannon deliberately organizes the subject around the intrinsic interest of its named theorems rather than treating them only as prerequisites, which is also the intended shape of this roadmap.

The finite-combinatorial side is modeled on the classical theories of maps and hypermaps and on Damiand–Lienhardt’s systematic treatment of \(n\)-maps and \(n\)-Gmaps. In those models, cells are permutation orbits, orientability and Euler characteristic are finite invariants, and cellular operations become local modifications of involutions or permutations. Damiand–Lienhardt also develops the simplicial interpretation of Gmaps, which is the key bridge this roadmap needs.

The main named summits are:

- the polygonal and full Jordan curve theorems;
- the Schönflies theorem, including its ambient form;
- the characterization of the 2-sphere;
- planar tameness of arcs and Cantor sets;
- Moore’s decomposition theorem;
- invariance of domain in dimension two;
- the Radó triangulation theorem;
- the two-dimensional Hauptvermutung;
- Pachner’s theorem in dimension two;
- equivalence of triangulated surfaces, finite cellulations, and 2-Gmaps;
- classification of compact surfaces, including boundary;
- Euler–Poincaré and the homological characterization of orientability;
- equivalence of topological, PL, and smooth surface structures in the precise low-dimensional sense stated below;
- the Heffter–Edmonds–Ringel rotation principle;
- Kuratowski’s planarity theorem;
- Whitney’s unique-embedding theorem;
- Mac Lane’s cycle-space criterion for planarity;
- the five-color theorem;
- Dehn–Lickorish generation of the mapping class group.

The Four Color Theorem and the Map Color Theorem motivate the library but are not work in this roadmap. They are recorded at the end as a roadmap-for-a-roadmap.

Suggested homes:

- `TauCeti/Topology/Plane/`
- `TauCeti/Topology/Surface/`
- `TauCeti/Topology/PL/TwoDimensional/`
- `TauCeti/Combinatorics/Map/`
- `TauCeti/Combinatorics/Graph/Surface/`
- `TauCeti/AlgebraicTopology/FiniteComplex/`

## Relationship to other Tau Ceti roadmaps

### No upstream Tau Ceti dependency

This roadmap depends only on Mathlib and on earlier layers in this roadmap. It does not require another Tau Ceti roadmap to be implemented first.

Other roadmaps consume its results, and several own general-dimensional or analytic structures adjacent to the present subject. Those ownership boundaries are stated here to prevent duplicate APIs.

### Relationship to `GeometricTopology`

The `GeometricTopology` roadmap contains two accounts of PL topology:

1. a chart-based `PLGroupoid` in general dimension;
2. a simplicial account through abstract complexes, links, combinatorial manifolds, realizations, and collapse.

It explicitly requires a reconciliation theorem between these accounts.

The division of responsibility is:

- this roadmap owns finite and PL topology special to dimensions one and two;
- this roadmap owns triangulation of topological surfaces, the two-dimensional Hauptvermutung, two-dimensional Pachner moves, regular neighborhoods in surfaces, and the finite realization theorems for surface cellulations and Gmaps;
- this roadmap owns the low-dimensional comparison of topological, PL, and smooth surface structures;
- `GeometricTopology` owns the chart-based `PLGroupoid`, arbitrary-dimensional combinatorial manifolds, high-dimensional triangulation and smoothing obstructions, and general collapse theory;
- the adapter from the subdivision-based notion of a PL surface developed here to the chart-based `PLGroupoid` belongs to `GeometricTopology`;
- the dimension-two instance of the combinatorial-manifold realization theorem should consume the implementation built here rather than creating a second surface theory.

This roadmap therefore does not import `PLGroupoid`. Its intrinsic PL language is triangulations, subdivisions, simplicial maps, and PL maps between finite realizations. It exports enough structure for the general chart-based comparison to be proved elsewhere.

The surface-curve and mapping-class layers here are also direct inputs to `GeometricTopology`’s Heegaard-splitting layer, whose gluing data includes a surface diffeomorphism.

### Relationship to `ConformalMapping`

This roadmap owns:

- Jordan curves and Jordan domains as topological objects;
- the Jordan separation theorem;
- the Schönflies theorem;
- the homeomorphism type of the closure of a Jordan domain;
- topological boundary and extension lemmas needed to state those results.

The `ConformalMapping` roadmap owns the analytic Carathéodory boundary correspondence: the Riemann map of a Jordan domain extends to a homeomorphism of closures. Its boundary theorem should use the Jordan-domain API defined here. This roadmap does not prove Schönflies through conformal mapping and does not depend on the Riemann mapping theorem.

### Relationship to `UniversalCovers`

This roadmap does not rebuild the general theory of universal covers or deck transformations. It may use explicit covers of particular surfaces, such as

\[
\mathbb R^2 \longrightarrow \mathbb R^2/\mathbb Z^2,
\]

when proving the classification of curves on the torus. General covering-space constructions and their classification remain owned by `UniversalCovers`.

### Prior Lean work: source and integration target, not dependency

Two existing Lean developments provide substantial prior art:

- `mccorvie/classification-of-surfaces` proves the Lean-Eval classification theorem and contains finite cyclic polygonal presentations, a finite surface-cell-complex layer, and a faithful chain of realization homeomorphisms. Its public proof is complete and sorry-free.
- `rkirov/jordan_pick` contains sorry-free polygonal and continuous Jordan curve theorems and related Radó material.

This roadmap specifies the mathematics independently of those implementations. Integration or adaptation must follow Tau Ceti’s coordination and licensing rules. Existing code should be treated as cited prior art rather than as the roadmap specification, in accordance with the Tau Ceti contribution guidelines.

The Coq/Rocq Four Color development, Dufourd’s generalized-map developments, and Hales’s hypermap library are likewise design references rather than code dependencies. They provide strong evidence for the utility of the representation: Hales reports that replacing topological planar-map arguments by hypermap arguments substantially reduced formalization effort.

## Principles

These principles specialize the general Tau Ceti roadmap rules to this subject. A roadmap is a specification for a reusable library, not a list of headline statements; every layer must be grounded in Mathlib or in earlier layers, conventions must be fixed before agents implement incompatible versions, and every definition must receive a usable basic API.

### Build the equivalences, not four isolated theories

The topological, PL, cellular, and permutation models are all first-class. No representation is declared canonical. Each receives the operations natural to it, and the library proves explicit conversions and round-trip theorems.

A theorem proved in one presentation should be transportable to the others through named equivalences. The principal value is not merely that all representations describe surfaces, but that they form a coherent formal ecosystem.

### Dimension two at full natural generality

Results are stated for compact surfaces with boundary unless the mathematics genuinely requires an empty boundary. Connectedness is a hypothesis, not a bundled field. Results for disconnected compact surfaces are obtained componentwise.

The algebraic definition of an \(n\)-Gmap is dimension-polymorphic because its axioms are naturally indexed by `Fin (n + 1)`. The topological realization, manifold recognition, triangulation, classification, and move theory in this roadmap are developed only through dimension two.

### Raw data and validity predicates remain separate

A finite incidence structure, Gmap, map, or polygon word may describe a manifold, a pseudomanifold, a quasi-manifold, or a degenerate cellular object. The data structure should not hide its meaningful subcategories.

Use separate predicates such as:

- `IsConnected`;
- `IsSurfaceGMap`;
- `IsClosedSurfaceGMap`;
- `IsSurfaceCellulation`;
- `IsCombinatorialSurface`;
- `IsCellularEmbedding`;
- `IsOrientable`.

This permits operations to state their exact preservation hypotheses and allows malformed or singular structures to be discussed when proving recognition criteria.

### Boundary is first-class

Boundary is not simulated by deleting a face after every theorem has been proved for closed surfaces.

The roadmap supports:

- manifolds with boundary;
- interval links at boundary vertices;
- boundary darts or fixed points in the appropriate Gmap involution;
- boundary walks and boundary components in cellulations;
- relative isotopy and PL results;
- cutting and sewing along boundary arcs and circles;
- classification with an explicit boundary-component count.

The topological boundary of a subset, the manifold boundary, and the combinatorial boundary of a cellulation must have distinct names and proved comparison theorems.

### Operations carry semantic theorems

Sewing two darts, contracting an edge, cutting along a curve, or applying a polygon-word move is not complete until the library proves what happened to the represented space.

Every operation has:

1. a finite combinatorial definition;
2. a precise validity predicate or precondition;
3. a theorem describing cells, boundary, connectedness, and orientation after the operation;
4. a realization theorem, usually a homeomorphism, PL homeomorphism, or a theorem identifying a cut-and-glued topological space.

### Finite objects should compute

For finite complexes, maps, Gmaps, and cellulations, the following should be computable:

- connected components;
- cells as orbit partitions;
- vertex, edge, face, and boundary-component counts;
- orientability;
- Euler characteristic;
- genus or crosscap number when the object is a surface;
- duality;
- the incidence matrices of the cellular chain complex;
- the induced underlying multigraph;
- whether a finite combinatorial structure satisfies the local surface condition.

Theorems may be noncomputable where Mathlib’s quotient or linear-algebra APIs require it, but executable companions and `Decidable` instances should be supplied where the data are finite.

### No `SimpleGraph` foundation for maps

Cellulations and surface maps naturally contain loops, parallel edges, bridges, repeated vertices on face boundaries, and one-vertex presentations. The foundational graph-facing type is Mathlib’s `Graph α β`, which supports loops and multiple edges. `SimpleGraph` is used only after proving the relevant map has no loops and no parallel edges, or when a theorem such as Kuratowski, Whitney, or vertex coloring is intentionally stated for simple graphs.

### Isomorphism before quotient

Do not quotient maps, Gmaps, triangulations, or cellulations by relabeling in their foundational definitions.

Define:

- map isomorphisms as equivalences of dart types commuting with the distinguished permutations;
- Gmap isomorphisms as equivalences commuting with every involution;
- cellulation isomorphisms as equivalences of cells preserving all boundary and incidence data;
- simplicial isomorphisms using the existing complex map API;
- equivalence of triangulations through common subdivisions or PL homeomorphisms.

Invariants are proved invariant under these equivalences. Quotient or moduli types are introduced only for a theorem that genuinely quantifies over equivalence classes.

### Orientation data and orientability are distinct

`IsOrientable` is a proposition. An orientation is data, with two choices on each connected orientable closed surface.

The APIs must distinguish:

- proving that an object is orientable;
- choosing an orientation;
- reversing an orientation;
- orientation-preserving maps;
- forgetting an orientation.

For Gmaps, orientability is represented by a bipartition of the dart-adjacency graph in which every nontrivial involution changes side. For triangulations and cellulations, it is represented by coherent orientations of top-dimensional cells. These definitions are proved equivalent.

### No analytic shortcuts in the foundational route

Jordan–Schönflies, triangulation, classification, and the PL comparison are proved topologically and combinatorially. The roadmap does not import the Riemann mapping theorem, uniformization, hyperbolic metrics, Gauss–Bonnet, or analytic approximation.

Metrics may be used as auxiliary devices for compactness, mesh size, Lebesgue numbers, and controlled approximation. Metric geometry is not developed.

### Named theorems are acceptance tests for the library

Jordan–Schönflies tests plane topology and approximation. Radó tests the passage from local charts to global finite structure. Hauptvermutung and Pachner test the PL API. Surface classification tests the elementary-move calculus. Euler–Poincaré tests the chain-complex interface. Heffter–Edmonds–Ringel tests the map–embedding correspondence. Kuratowski and Whitney test the graph-facing boundary of the library.

A proof that bypasses the intended infrastructure does not discharge the corresponding layer.

## Encoding conventions

### Topological surfaces

A topological surface is a type with Mathlib’s standard topological and manifold instances, not a bundled record. Use the existing `TopologicalSpace`, separation, countability, `ChartedSpace`, `continuousGroupoid`, and `IsManifold` vocabulary.

Constructions such as cutting, doubling, connected sum, and gluing produce types equipped with instances and named homeomorphisms. They are not represented by a global quotient of all surfaces.

“Compact surface” means a compact, Hausdorff, second-countable topological 2-manifold, allowing boundary unless explicitly stated otherwise.

### Embeddings and isotopies

Use Mathlib’s `Topology.IsEmbedding` and `Homeomorph`.

Build once:

- isotopy of continuous maps;
- isotopy relative to a subset;
- ambient isotopy as a continuous path of self-homeomorphisms;
- supported ambient isotopy;
- PL and smooth specializations;
- isotopy of embeddings.

An “arc” is an embedding of a standard closed interval. A “simple closed curve” is an embedding of the standard circle. Images may be used in geometric statements, but the embedding remains available whenever parameterization and orientation matter.

### Finite simplicial complexes

Use Mathlib’s `PreAbstractSimplicialComplex` and `AbstractSimplicialComplex`; do not introduce a competing sets-of-faces structure. A finite complex is represented on a finite vertex type. Mathlib’s abstract-complex definition contains every singleton of its ambient vertex type, so the ambient type must not silently contain irrelevant infinitely many vertices.

Build the missing finite-complex API around those definitions:

- dimension, purity, facets, skeletons;
- star and link;
- joins and cones;
- subdivisions and barycentric subdivision;
- simplicial maps and isomorphisms;
- finite realization;
- boundary subcomplex;
- local 1- and 2-manifold predicates;
- collapse and shelling.

A topological triangulation of `M` consists of a finite abstract complex `K`, the appropriate local-manifold proof when needed, and a homeomorphism from its realization to `M`.

### PL maps and PL equivalence

A map between finite realizations is PL when subdivisions of source and target make it simplicial. The definition quantifies over subdivisions rather than choosing a privileged triangulation.

A PL homeomorphism contains:

- a homeomorphism of realizations;
- source and target subdivisions;
- a simplicial isomorphism representing the homeomorphism on those subdivisions.

The theorem that a topological homeomorphism of triangulated surfaces is isotopic to such a PL homeomorphism belongs to the two-dimensional Hauptvermutung layer.

### Finite 2-cellulations

A `Cellulation2` is not merely a finite poset of cells. Incidence posets lose multi-incidence information: a face may meet the same vertex or edge more than once.

The structure therefore carries finite types of vertices, edges, and faces, together with:

- the two ends of each edge, allowing them to agree;
- an oriented finite cyclic boundary walk for each face;
- an indication of whether each use of an edge follows or opposes its chosen orientation;
- proof that every boundary step has matching endpoints;
- enough side-incidence data to distinguish the two appearances of an edge;
- boundary-edge information.

The condition `IsSurfaceCellulation` states the local surface axioms. It is not a structure field.

The realization of a cellulation is defined by attaching a polygonal disk for each face to the graph realization along its cyclic boundary walk. The flag triangulation gives a second realization, and the two are proved homeomorphic.

### Cyclic words

A polygonal schema is a cyclic word, not a list with a secretly preferred first letter.

Define cyclic words intrinsically or as lists modulo rotation, with:

- reversal;
- cyclic concatenation and cutting;
- occurrence counts;
- signed letters;
- renaming;
- cyclic equivalence;
- proved normalization lemmas.

For a closed surface schema, every unoriented edge label occurs exactly twice. For a surface with boundary, boundary letters and paired gluing letters remain distinguished.

### Oriented combinatorial maps

For a finite dart type `D`, an oriented combinatorial map contains:

- a permutation `vertexPerm : Equiv.Perm D`, whose cycles are vertices;
- a fixed-point-free involution `edgeFlip : Equiv.Perm D`, whose two-cycles are edges.

Pin the face convention by a pointwise definition rather than relying on multiplication conventions:

```lean
def facePerm (M : CombinatorialMap D) (d : D) : D :=
  M.vertexPerm.symm (M.edgeFlip d)
```

The orientation convention is chosen so that repeated application of `facePerm` traverses the face lying to the left of the dart. All examples and conversion theorems use this convention.

Connectedness is transitivity of the action generated by `vertexPerm` and `edgeFlip`. It is a predicate.

### Hypermaps

A hypermap on `D` is encoded by permutations

\[
\sigma_0,\sigma_1,\sigma_2
\]

with a pinned product relation and a transitivity predicate for connectedness. It is acceptable to store two permutations and derive the third, provided the derived permutation and all three cycle interfaces are public.

The cycles represent hypervertices, hyperedges, and hyperfaces. Ordinary oriented maps are the special case in which the hyperedge permutation is a fixed-point-free involution.

### Generalized maps

An \(n\)-Gmap on a finite dart type `D` contains involutions

\[
\alpha_0,\ldots,\alpha_n.
\]

The distant-involution condition is stated as commutativity:

\[
|i-j|\ge 2
\quad\Longrightarrow\quad
\alpha_i\alpha_j=\alpha_j\alpha_i.
\]

Since each \(\alpha_i\) is an involution, this is equivalent to the standard condition that \(\alpha_i\alpha_j\) is an involution.

For a 2-Gmap:

\[
\begin{aligned}
0\text{-cells} &= \operatorname{Orb}\langle\alpha_1,\alpha_2\rangle,\\
1\text{-cells} &= \operatorname{Orb}\langle\alpha_0,\alpha_2\rangle,\\
2\text{-cells} &= \operatorname{Orb}\langle\alpha_0,\alpha_1\rangle,\\
\text{components} &= \operatorname{Orb}\langle\alpha_0,\alpha_1,\alpha_2\rangle.
\end{aligned}
\]

Boundary is represented using fixed points of the relevant involution, following the standard “\(i\)-free dart” convention. Surface Gmaps have no \(0\)- or \(1\)-boundary and may have \(2\)-boundary. The exact local predicate is proved equivalent to the link condition on the flag triangulation.

The algebraic definition may be dimension-polymorphic:

```lean
structure GMap (n : ℕ) (D : Type*) [Fintype D] where
  adj : Fin (n + 1) → Equiv.Perm D
  involutive : ∀ i d, adj i (adj i d) = d
  commute_of_far :
    ∀ i j, i.1 + 2 ≤ j.1 → Function.Commute (adj i) (adj j)
```

The implementation may use an equivalent index relation that is more symmetric or easier for Lean.

### Cells as orbits

Use Mathlib’s permutation, subgroup-action, and orbit APIs rather than defining reachability afresh for every object. Mathlib already provides permutation-cycle theory and `MulAction.orbit`.

Each orbit-based cell notion needs both:

- a quotient or subtype suitable for theorem statements;
- a finite partition or representative API suitable for computation and counting.

### Underlying graphs

A map or cellulation has an underlying Mathlib multigraph

```lean
Graph Vertex Edge
```

whose edge incidence is induced by the combinatorial data. Loops and parallel edges are preserved.

Provide predicates and conversions for:

- loopless maps;
- maps without parallel edges;
- the associated `SimpleGraph`;
- graph isomorphisms induced by map isomorphisms;
- deletion and contraction compatibility.

Mathlib’s multigraph API is young: it supplies incidence, subgraphs, deletion, and vertex maps, but not yet a mature morphism, planarity, minor, or embedding theory. Build the missing graph-facing structures here in a way that follows the existing `Graph` design.

### Chain complexes

Finite simplicial and cellular chains are actual Mathlib chain complexes:

```lean
ChainComplex (ModuleCat R) ℕ
```

over at least \(R=\mathbb Z\) and \(R=\mathbf F_2\).

Use Mathlib’s `HomologicalComplex`, homology, free modules, matrices, and Euler-characteristic infrastructure. Do not define homology as an ad hoc quotient of lists.

The direct finite simplicial chain complex is related to Mathlib’s simplicial-set chain complex rather than becoming a parallel incompatible theory. Mathlib already defines `SSet.chainComplex` and `SSet.homology`.

## Inventory: what Mathlib gives us

Consume these APIs rather than rebuilding them.

### Point-set topology

- compactness, connectedness, local connectedness, components, quotient topology, and normality;
- `ContinuousMap`;
- `Topology.IsEmbedding`, open and closed embeddings;
- `Homeomorph`;
- homotopies and fundamental groups;
- covering maps;
- singular homology;
- CW complexes.

The `GeometricTopology` roadmap records the relevant current files and declaration names.

### Manifold infrastructure

- `ModelWithCorners`;
- `ChartedSpace`;
- `continuousGroupoid`;
- `IsManifold`;
- manifold interior and boundary-point predicates;
- smooth manifolds and `Diffeomorph`;
- smooth embeddings.

Use these directly in the topological and smooth comparison layers.

### Finite permutations and actions

- `Equiv.Perm`;
- cycles and `SameCycle`;
- generated subgroups;
- finite group actions;
- orbits and transitivity;
- finite equivalences and cardinality;
- circular orders.

Mathlib’s `CircularOrder` is useful for geometric cyclic order, but a finite boundary word still requires an explicit cyclic-sequence API carrying repeated labels.

### Simplicial and homological infrastructure

- `PreAbstractSimplicialComplex` and `AbstractSimplicialComplex`;
- geometric simplicial complexes in vector spaces;
- simplicial sets and their realization adjunction;
- simplicial-set chain complexes and homology;
- general homological complexes;
- Euler characteristics of graded objects, chain complexes, and their homology.

Mathlib has the raw abstract and geometric complex structures but not the finite realization, star/link/subdivision, PL map, or combinatorial-surface theory required here.

### Graph infrastructure

- the mature `SimpleGraph` theory, including connectivity and vertex coloring;
- `SimpleGraph.Coloring`, `Colorable`, and `chromaticNumber`;
- the newer multigraph `Graph α β`, which supports loops and multiple edges;
- elementary multigraph subgraphs, deletion, and vertex maps.

Mathlib’s `SimpleGraph.Dart` is an oriented adjacent vertex pair and has a reversal involution. It is useful when a graph is already simple, but it cannot serve as the foundational dart type for cellular maps because it forgets edge identity and excludes loops.

## Inventory: what this roadmap adds

Mathlib does not currently provide a first-class theory of:

- polygonal or full Jordan–Schönflies;
- planar tameness;
- sphere recognition or Moore decompositions;
- triangulations of topological surfaces;
- subdivisions, common subdivisions, stars, links, or PL maps for finite abstract complexes at the needed level;
- the two-dimensional Hauptvermutung or Pachner moves;
- finite surface cellulations with multi-incidence;
- rotation systems, combinatorial maps, hypermaps, or Gmaps;
- realization of those objects as surfaces;
- polygonal schemas and their elementary-move calculus;
- finite cellular homology attached to these presentations;
- the classification of compact surfaces;
- topological graph embeddings, planarity, graph genus, or planar duality;
- Kuratowski, Whitney uniqueness, Mac Lane planarity, or planar coloring theorems;
- mapping class groups and Dehn twists.

The layers below are the build list.

## Dependency graph

The roadmap is not a single linear proof. Its principal dependencies are:

```text
L0 ──→ L1 ──→ L3 ──→ L6
 │      │      │       │
 │      │      └──→ L4 │
 │      └───────────────┘
 │
 ├──→ L2 ──→ L3, L4
 │
 ├──→ L5 ──→ L6 ──→ L8
 │                    ↑
 └──→ L7 ─────────────┘

L8 ──→ L9 ──→ L11
 │      │       ↑
 ├──→ L10 ──────┘
 │
 └────────────→ L14

L6, L11 ──→ L12 ──→ L13
L10, L11 ─────────→ L13
L8, L10, L11 ─────→ L14
```

Layer 4 is a point-set-topology branch. It is part of the roadmap but is not placed on the critical path to triangulation and classification.

## The layers, bottom-up from Mathlib

### Layer 0: shared conventions and low-dimensional interfaces

This layer prevents each later branch from introducing its own circle, polygon, isotopy, cyclic word, or finite-orbit API.

#### From Mathlib

`Homeomorph`, `ContinuousMap`, `Topology.IsEmbedding`, homotopies, compactness and connectedness; standard intervals, circles, Euclidean spaces, spheres, quotients; `Equiv.Perm`, group actions, `Fintype`, `Finset`, and `CircularOrder`.

#### What to build

- Standard topological models and named homeomorphisms among the square, closed disk, triangle, circle, line, half-plane, sphere, and one-point compactification of the plane.
- Arcs and simple closed curves as embeddings, with oriented and unoriented variants.
- Paths and arcs relative to a subset.
- Isotopy, relative isotopy, ambient isotopy, supported ambient isotopy, and their composition and restriction APIs.
- Finite cyclic sequences and cyclic words, including rotation, reversal, cutting, concatenation, signed letters, renaming, and occurrence counts.
- Finite orbit partitions for one or several permutations.
- Transitivity and connected-component lemmas for generated permutation actions.
- Finite partitions and Euler-style alternating counts.
- Standard polygons and piecewise-affine maps on a finite subdivision.
- A precise namespace and terminology policy distinguishing:
  - continuous maps;
  - combinatorial maps;
  - graph embeddings;
  - cell embeddings;
  - manifold boundary;
  - topological frontier;
  - combinatorial boundary.

#### Illustrative targets

```lean
def IsArc (f : C(I, X)) : Prop := Topology.IsEmbedding f
def IsSimpleClosedCurve (f : C(Circle, X)) : Prop := Topology.IsEmbedding f

def IsotopicRel (A : Set X) (f g : C(X, Y)) : Prop := ...
def AmbientIsotopicRel (A : Set X) (e₀ e₁ : X → Y) : Prop := ...

def CyclicWord (α : Type*) : Type* := ...
def CyclicWord.reverse : CyclicWord α → CyclicWord α
def CyclicWord.rename (e : α ≃ β) : CyclicWord α → CyclicWord β

def orbitPartition (p : Fin ι → Equiv.Perm D) : Finpartition D := ...
```

#### Design notes

Do not represent a cyclic word as a bare list and then repeatedly prove invariance under changing the first element. Do not define isotopy separately for arcs, curves, PL maps, and homeomorphisms. The general relation comes first.

#### Unlocks

Every subsequent layer.

---

### Layer 1: polygonal planar topology

This is the finite geometric engine. It proves separation and disk results before arbitrary continuous curves enter.

#### From Mathlib and Layer 0

Euclidean and affine geometry, finite sets and polygonal paths, continuity and compactness, finite cyclic words, embeddings, and isotopies.

#### What to build

- Polygonal arcs, polygonal paths, polygonal loops, and polygonal simple closed curves.
- Subdivision and reparameterization of polygonal paths.
- General position for finite families of polygonal arcs:
  - avoidance of a finite vertex set;
  - transverse intersections;
  - elimination of overlapping segments;
  - perturbation relative to fixed endpoints.
- Local two-sidedness of a polygonal simple closed curve.
- Crossing parity and algebraic crossing number with a generic ray or arc.
- Winding number for polygonal loops, with local constancy on the complement.
- The one-ended-arc principle for finite graphs.
- Polygonal Jordan separation:
  - the complement has exactly two connected components;
  - one is bounded and one is unbounded;
  - the curve is the boundary of each.
- Point-in-polygon criteria through parity and winding.
- Triangulation of a polygonal disk, both allowing and avoiding new interior vertices where the hypotheses permit.
- Polygonal Schönflies:
  - the closure of the bounded component is a PL disk;
  - an embedding of a polygonal circle extends to a PL homeomorphism of the disk;
  - the ambient plane or sphere version.
- Elementary disk and annulus gluing lemmas:
  - two disks glued along a boundary arc form a disk;
  - a disk cut along a properly embedded polygonal arc becomes a disk;
  - the region between nested polygonal circles is an annulus.
- Polygonal Alexander trick and relative PL disk isotopies.

Cannon’s polygonal arguments emphasize general position and finite crossing counts as prototypes for later two-dimensional topology.

#### Illustrative targets

```lean
def IsPolygonalArc (γ : C(I, ℝ × ℝ)) : Prop := ...
def IsPolygonalJordanCurve (γ : C(Circle, ℝ × ℝ)) : Prop := ...

theorem polygonal_jordan
    (hγ : IsPolygonalJordanCurve γ) :
    Nat.card (ConnectedComponents (Set.range γ)ᶜ) = 2 := ...

theorem polygonal_jordan_common_boundary
    (hγ : IsPolygonalJordanCurve γ) :
    ∀ U ∈ components (Set.range γ)ᶜ, frontier U = Set.range γ := ...

theorem polygonal_schoenflies
    (hγ : IsPolygonalJordanCurve γ) :
    ∃ h : (ℝ × ℝ) ≃ₜ (ℝ × ℝ),
      h '' standardCircle = Set.range γ := ...
```

#### Acceptance examples

The triangle, a nonconvex simple polygon, and a polygon with many reflex vertices must all instantiate the same API. The inside/outside classifier must agree with winding number and crossing parity.

#### Unlocks

Controlled approximation, full Jordan–Schönflies, polygonal cell realizations, PL disks and annuli, and planar graph arguments.

---

### Layer 2: continua, arcs, circles, and Cantor sets

This layer develops the point-set topology used by Cannon’s sphere-recognition and decomposition arguments. It excludes the measure-theoretic material on positive-area curves.

#### From Mathlib and Layer 0

Compact metric spaces, connectedness, local connectedness, components, continuous images, products, quotients, and embeddings.

#### What to build

- Continua and Peano continua using Mathlib predicates rather than a redundant bundled type.
- Components of open subsets of locally connected spaces are open.
- Local arc-connectedness results in locally connected metric continua.
- Hahn–Mazurkiewicz:
  - every Peano continuum is a continuous image of the interval;
  - the image of the interval is compact, connected, locally connected, and metrizable.
- Characterization of the arc by its separation and endpoint properties.
- Characterization of the simple closed curve by its local and global separation properties.
- Characterization of the Cantor set as a nonempty compact metrizable perfect totally disconnected space.
- Nested finite decompositions used to construct homeomorphisms of Cantor sets.
- Irreducible arcs joining disjoint closed sets.
- Crosscuts and properly embedded arcs in disks and Jordan domains.
- Elementary monotone decomposition facts needed in Layer 4.

#### Illustrative targets

```lean
def IsPeanoContinuum (X : Type*) [TopologicalSpace X] : Prop :=
  CompactSpace X ∧ ConnectedSpace X ∧ LocallyConnectedSpace X ∧ MetrizableSpace X

theorem peano_continuum_iff_interval_image :
    IsPeanoContinuum X ↔ ∃ f : C(I, X), Function.Surjective f := ...

theorem cantor_characterization
    (hcompact : IsCompact (Set.univ : Set X))
    (hperfect : PerfectSpace X)
    (htd : TotallyDisconnectedSpace X)
    (hne : Nonempty X) :
    Nonempty (X ≃ₜ CantorSpace) := ...
```

#### Design notes

Use existing Mathlib properties whenever possible. `PeanoContinuum` may be a readable abbreviation, but theorems should expose the standard hypotheses if that gives better reuse.

#### Unlocks

Sphere recognition, planar tameness, Moore decompositions, and alternative proofs of Jordan–Schönflies.

---

### Layer 3: controlled approximation, Jordan separation, and Schönflies

This layer passes from finite polygonal topology to arbitrary embedded arcs and circles. It deliberately places Schönflies before surface triangulation.

Cannon’s triangulation proof likewise assumes a previously established Schönflies theorem.

#### From earlier layers

Polygonal Jordan–Schönflies, finite general position, continua and irreducible arcs, ambient isotopy, compactness, uniform continuity, and Mathlib’s topological components.

#### What to build

- Quantitative polygonal approximation of an embedded arc or circle:
  - prescribed mesh;
  - prescribed neighborhood of the image;
  - relative control on selected subarcs;
  - preservation of injectivity;
  - a controlled correspondence between complementary regions.
- Approximation of finite families of disjoint arcs and curves simultaneously.
- Small ambient isotopies taking sufficiently controlled curves to polygonal curves.
- Arc non-separation in \(S^2\) and \(\mathbb R^2\).
- The full Jordan curve theorem:
  - exactly two complementary components;
  - one bounded and one unbounded in the plane;
  - the curve is the full boundary of each;
  - every path from one component to the other meets the curve.
- The Jordan domain API:
  - interior and exterior;
  - closure of the interior;
  - crosscuts;
  - prime-end-free boundary lemmas needed topologically.
- The full Schönflies theorem in equivalent forms:
  - every Jordan curve bounds a topological disk;
  - both complementary closures in \(S^2\) are disks;
  - every embedding \(S^1\hookrightarrow S^2\) extends to a self-homeomorphism of \(S^2\);
  - the plane version with control of the point at infinity;
  - a boundary homeomorphism of disks extends to a disk homeomorphism.
- Relative and supported versions:
  - fix a chosen boundary arc or finite set;
  - localize the ambient homeomorphism to a prescribed neighborhood where possible.
- Alexander’s trick for topological disks.
- Uniqueness up to isotopy of a properly embedded spanning arc in a disk.

#### Illustrative targets

```lean
theorem jordan_curve
    (e : Circle → ℝ × ℝ)
    (he : Topology.IsEmbedding e) :
    Nat.card (ConnectedComponents (Set.range e)ᶜ) = 2 := ...

theorem jordan_curve_frontier
    (e : Circle → ℝ × ℝ)
    (he : Topology.IsEmbedding e) :
    ∀ U ∈ components (Set.range e)ᶜ,
      frontier U = Set.range e := ...

theorem schoenflies_sphere
    (e : Circle → Sphere2)
    (he : Topology.IsEmbedding e) :
    ∃ h : Sphere2 ≃ₜ Sphere2, h ∘ standardCircle = e := ...

theorem schoenflies_disk_extension
    (h : Circle ≃ₜ Circle) :
    ∃ H : Disk2 ≃ₜ Disk2, H.restrictBoundary = h := ...
```

#### Design notes

The exact Lean-Eval Jordan statement counts connected components, but the library theorem must expose the stronger common-boundary and path-separation conclusions. The count alone is not a sufficient API.

The proof may reuse ideas or code from `jordan_pick`, but the accepted result is the full topological theory listed here rather than an isolated bridge theorem.

#### Unlocks

Radó triangulation, disk and annulus recognition, tameness, sphere recognition, and planar graph topology.

---

### Layer 4: sphere recognition, planar tameness, Moore decomposition, and two-dimensional invariance of domain

This is the principal point-set-topology branch.

#### From Layers 2 and 3

Peano continua, arcs and simple closed curves, Jordan separation, Schönflies, disk isotopies, quotient spaces, and compact metrizable topology.

#### What to build

##### Characterization of the 2-sphere

Prove the Cannon–Zippin-style characterization:

A nondegenerate Peano continuum in which no arc separates and in which every simple closed curve separates into exactly two components with common boundary is homeomorphic to \(S^2\).

Also prove that the closure of each complementary domain of a simple closed curve is a disk. Cannon derives Schönflies as an immediate special case of this sphere characterization; this roadmap already has Schönflies from Layer 3, so the two routes must be reconciled rather than left as unrelated theorems.

##### Tameness

- Every arc in \(S^2\) is tame.
- Every embedding of the standard Cantor set in the plane is ambiently equivalent to the standard embedding.
- Finite families of disjoint disks are ambiently equivalent under the appropriate incidence and orientation hypotheses.
- Disk isotopy and isotopy-extension results in dimension two.
- Local flatness of embedded one-manifolds in surfaces.

Cannon’s planar Cantor theorem states that every Cantor-set embedding extends to a plane homeomorphism.

##### Moore decomposition

Develop:

- decompositions of a compact space;
- decomposition maps and decomposition spaces;
- upper semicontinuity;
- monotone decompositions;
- nonseparating continua.

Prove Moore’s theorem in the form:

> An upper-semicontinuous decomposition of \(S^2\) into nonseparating continua has decomposition space homeomorphic to \(S^2\), under the standard nondegeneracy hypotheses.

The proof should use the sphere-characterization API, following Cannon’s architecture.

##### Invariance of domain in dimension two

Prove:

- a continuous injective map between open subsets of \(\mathbb R^2\) is open;
- a continuous injective map between 2-manifolds is open;
- a compact injective map from a surface to a Hausdorff surface is a topological embedding;
- local dimension and boundary type are invariant under surface charts.

Name the main theorem `invarianceOfDomain₂` or another unambiguous variant, not merely `openMappingTheorem`, which would collide with complex analysis.

#### Illustrative targets

```lean
theorem sphere_characterization
    [IsPeanoContinuum X]
    (harc : ∀ A : Arc X, ¬ Separates A.range X)
    (hjordan : ∀ J : SimpleClosedCurve X, JordanSeparates J) :
    Nonempty (X ≃ₜ Sphere2) := ...

theorem planar_cantor_tame
    (e : CantorSpace → ℝ × ℝ)
    (he : Topology.IsEmbedding e) :
    ∃ h : (ℝ × ℝ) ≃ₜ (ℝ × ℝ), h ∘ standardCantorEmbedding = e := ...

theorem moore_decomposition
    (G : USCDecomposition Sphere2)
    (h_nonseparating : ∀ g ∈ G, ¬ Separates g Sphere2)
    (h_nondegenerate : ...) :
    Nonempty (G.Quotient ≃ₜ Sphere2) := ...

theorem invarianceOfDomain₂
    {U V : Set (ℝ × ℝ)}
    (hU : IsOpen U) (hV : IsOpen V)
    (f : U → V)
    (hf : Continuous f)
    (hinj : Function.Injective f) :
    IsOpenMap f := ...
```

#### Unlocks

Point-set-topological applications independent of triangulation; local-flatness and isotopy results used by surface cutting; comparison tests for the general `GeometricTopology` embedding API.

---

### Layer 5: finite two-dimensional PL complexes

This layer builds the finite PL substrate on Mathlib’s abstract and geometric simplicial complexes. It contains the dimension-two theory needed by this roadmap, not the general-dimensional `PLGroupoid`.

Rourke–Sanderson’s opening chapters provide the reference model: polyhedra and PL maps; complexes and subdivisions; regular neighborhoods; collapse, shelling, orientation, and connected sum; pairs and isotopies; and general position.

#### From Mathlib

`PreAbstractSimplicialComplex`, `AbstractSimplicialComplex`, geometric simplicial complexes, convex hulls, affine maps, quotient topology, simplicial sets, finite types, and homological complexes.

#### What to build

##### Basic finite-complex API

- Dimension and face dimension.
- Pure complexes.
- Facets and codimension-one faces.
- Skeletons.
- Simplicial maps, embeddings, and isomorphisms.
- Induced and full subcomplexes.
- Star, closed star, open star, and link of a face.
- Join, cone, and suspension.
- Opposite orientation of a simplex.
- Boundary of a pure complex.

##### Realization

Construct a functorial topological realization of a finite abstract complex. The preferred construction is the canonical affine realization in a finite-dimensional coordinate space, provided the resulting API relates cleanly to Mathlib’s geometric simplicial complexes.

Prove:

- each simplex realizes to a closed disk;
- intersections of realized simplices are realized common faces;
- the realization is compact and Hausdorff;
- simplicial maps induce continuous maps;
- simplicial isomorphisms induce homeomorphisms;
- joins and cones realize to topological joins and cones.

##### Subdivision

- Subdivision as a complex together with a realization-preserving simplicial refinement map.
- Stellar subdivision.
- Barycentric subdivision.
- Relative subdivision fixing a subcomplex.
- Common refinement of finitely many subdivisions.
- Subdivision of finite diagrams of injective PL maps.
- A PL map becomes simplicial after subdivisions of source and target.
- Two triangulations of the same PL polyhedron have a common subdivision.

##### Combinatorial one- and two-manifolds

Define local combinatorial manifold conditions using links.

For a finite pure 2-complex:

- an interior vertex has link a combinatorial circle;
- a boundary vertex has link a combinatorial interval;
- a codimension-one face belongs to one or two top-dimensional faces as appropriate;
- the boundary subcomplex is a combinatorial 1-manifold.

Prove:

\[
K\text{ satisfies the 2-manifold link condition}
\quad\Longrightarrow\quad
|K|\text{ is a topological surface with boundary}.
\]

The converse for a triangulation of a topological surface belongs to Layer 6.

##### PL disks, annuli, and regular neighborhoods

- Combinatorial and PL 1-manifold classification.
- Recognition of triangulated disks, circles, and annuli from links plus global hypotheses.
- Collars of the boundary of a PL surface.
- Regular neighborhoods of subcomplexes and embedded graphs in a PL surface.
- Uniqueness of regular neighborhoods up to ambient PL isotopy.
- Collapse and elementary expansion.
- Shelling of triangulated disks.
- A collapsible compact connected PL 2-manifold is a disk.
- The combinatorial annulus theorem.
- PL Schönflies as a comparison theorem with Layer 3.

#### Illustrative targets

```lean
def AbstractSimplicialComplex.realization
    (K : AbstractSimplicialComplex V) [Fintype V] : TopCat := ...

def AbstractSimplicialComplex.link
    (K : AbstractSimplicialComplex V) (σ : Finset V) :
    PreAbstractSimplicialComplex V := ...

def IsCombinatorialSurface
    (K : AbstractSimplicialComplex V) : Prop := ...

theorem realization_isSurface
    (hK : IsCombinatorialSurface K) :
    IsTopologicalSurfaceWithBoundary K.realization := ...

def IsPLMap (f : K.realization → L.realization) : Prop :=
  ∃ K' L', IsSubdivision K' K ∧ IsSubdivision L' L ∧
    ∃ F : SimplicialMap K' L', realizationMap F = f

theorem regularNeighborhood_unique
    (A : Subcomplex K) (N₁ N₂ : RegularNeighborhood A K) :
    AmbientPLIsotopicRel A N₁.carrier N₂.carrier := ...
```

#### Design notes

Do not define a new general abstract-complex type merely because Mathlib’s current API is incomplete. Extend the existing types.

A link condition is a finite combinatorial predicate. The theorem that its realization is locally Euclidean is substantial and should not be hidden in a constructor.

#### Unlocks

Triangulation, Hauptvermutung, Pachner moves, flag triangulations of Gmaps, regular neighborhoods of embedded graphs, and finite homology.

---

### Layer 6: triangulation, Hauptvermutung, and Pachner moves for surfaces

This layer proves that arbitrary compact topological surfaces enter the finite PL world and that the resulting PL structure is unique.

#### From earlier layers

Jordan–Schönflies and controlled approximation; finite PL complexes and their realization; collars, regular neighborhoods, disk and annulus theorems; topological surfaces with boundary.

#### What to build

##### Radó triangulation

Prove:

- every compact topological surface without boundary has a finite triangulation;
- every compact topological surface with boundary has a finite triangulation in which the manifold boundary is a subcomplex;
- locally finite versions for second-countable noncompact surfaces only insofar as they are needed in the proof; classification of noncompact surfaces is excluded;
- prescribed finite sets and tame embedded arcs can be made subcomplexes after subdivision;
- a finite family of embedded curves in general position can be included in a triangulation.

The proof route uses Jordan–Schönflies to build and triangulate disk regions. Cannon explicitly treats triangulation as the major first step toward classification and assumes Schönflies as the difficult planar input.

##### Two-dimensional Hauptvermutung

Prove equivalent forms:

- any two triangulations of a compact topological surface are compatible;
- the identity map between their realizations is PL after subdivisions;
- every homeomorphism between triangulated surfaces is isotopic to a PL homeomorphism;
- relative versions fix the boundary or a specified subcomplex;
- topological invariants defined through a triangulation are independent of that triangulation once subdivision invariance is proved.

Distinguish this theorem from the common-subdivision theorem for two triangulations of an already fixed PL polyhedron, which belongs to Layer 5. The new content is that an arbitrary topological homeomorphism of surfaces respects the unique PL structure.

##### Pachner theorem in dimension two

Define the surface bistellar moves:

- \(1\leftrightarrow3\);
- \(2\leftrightarrow2\);
- boundary moves for triangulated surfaces with boundary.

Prove:

- each move preserves the PL homeomorphism type;
- every stellar subdivision of a surface triangulation is a finite composition of bistellar moves;
- any two PL-homeomorphic closed triangulated surfaces are related by a finite sequence of \(1\leftrightarrow3\) and \(2\leftrightarrow2\) moves;
- the relative boundary version.

Pachner’s theorem provides a complete finite local calculus for triangulated PL manifolds.

##### Simplicial approximation in dimension two

- Approximate continuous maps between triangulated surfaces by simplicial maps after subdivision.
- Relative approximation.
- Approximation of embeddings by simplicial embeddings under the appropriate dimension-two hypotheses.
- Compatibility with homotopy and degree where those notions are present.

#### Illustrative targets

```lean
structure TopologicalTriangulation (M : Type*) where
  vertex : Type
  finite_vertex : Fintype vertex
  complex : AbstractSimplicialComplex vertex
  isSurface : IsCombinatorialSurface complex
  homeomorph : complex.realization ≃ₜ M

theorem rado_triangulation
    [CompactSpace M] [TopologicalSurfaceWithBoundary M] :
    Nonempty (TopologicalTriangulation M) := ...

theorem surface_hauptvermutung
    (T₁ T₂ : TopologicalTriangulation M) :
    CompatibleTriangulations T₁ T₂ := ...

inductive SurfacePachnerMove (K L : AbstractSimplicialComplex V)
  | oneThree : ...
  | threeOne : ...
  | twoTwo : ...
  | boundary : ...

theorem pachner₂
    (K L : FiniteTriangulatedSurface)
    (h : Nonempty (K.realization ≃ₚₗ L.realization)) :
    Relation.ReflTransGen SurfacePachnerMove K L := ...
```

#### Acceptance tests

- Triangulate \(S^2\), \(T^2\), \(\mathbb{RP}^2\), the Klein bottle, a disk, and an annulus.
- Relate two visibly different triangulations of the torus through the compatibility theorem.
- Express a barycentric subdivision of a triangulated surface as a finite Pachner sequence.
- Recover triangulation-independent Euler characteristic before using classification.

#### Unlocks

All finite presentations of arbitrary compact surfaces, topological invariance of finite combinatorial constructions, and the Top–PL comparison.

---

### Layer 7: finite cellulations, maps, hypermaps, and Gmaps

This layer develops the finite permutation topology independently of any particular classification proof.

Damiand–Lienhardt treats \(n\)-Gmaps and \(n\)-maps as explicit representations of subdivided objects, with sew/unsew operations, duality, orientability, classification invariants, simplicial interpretation, and Euler–Poincaré characteristic. 

Lando–Zvonkin develops the classical map as a topological embedded graph and as a permutation object, emphasizing that an abstract graph does not contain the cyclic order or face data of a map. 

#### From Mathlib and earlier layers

Finite permutations and group actions, finite cyclic words, Mathlib multigraphs, finite complexes, and standard topological surfaces.

#### What to build

##### Finite 2-cellulations

Implement the `Cellulation2` convention above and develop:

- vertices, edges, faces, flags, corners, and sides;
- incidence and multi-incidence;
- face boundary walks;
- boundary edges and boundary walks;
- connected components;
- dual incidence data;
- local vertex links;
- `IsSurfaceCellulation`;
- orientability;
- Euler cell count;
- isomorphisms and automorphisms;
- disjoint union and connected sum at the data level.

##### Oriented maps

Implement:

```lean
structure CombinatorialMap (D : Type*) [Fintype D] where
  vertexPerm : Equiv.Perm D
  edgeFlip : Equiv.Perm D
  edgeFlip_involutive : ∀ d, edgeFlip (edgeFlip d) = d
  edgeFlip_ne : ∀ d, edgeFlip d ≠ d
```

Develop:

- vertex, edge, and face orbit types;
- incidence and degree;
- connectedness;
- rooted maps;
- orientation reversal;
- dual and Petrie-style permutation operations where they stay within scope;
- map isomorphisms and automorphisms;
- underlying multigraph;
- Euler count;
- deletion and contraction at the raw finite level.

##### Hypermaps

Develop:

- hypervertices, hyperedges, hyperfaces;
- connectedness;
- isomorphism and automorphism;
- ordinary maps as a subtype or predicate;
- bipartite-map correspondence;
- canonical triangulation;
- conversion to a 2-Gmap.

##### \(n\)-Gmaps and 2-Gmaps

Develop the dimension-polymorphic algebraic definition and:

- \(i\)-cells as orbits;
- incidence and adjacency;
- components;
- submaps and restriction to dart subsets under closure hypotheses;
- fixed-point and \(i\)-free darts;
- boundary maps;
- orientability and orientations;
- duality;
- Gmap isomorphisms and automorphisms;
- dimension increase and decrease where algebraically natural;
- specialization to 0-, 1-, and 2-Gmaps;
- `IsSurfaceGMap`;
- closed and boundary surface subclasses.

##### Conversions among finite descriptions

- `CombinatorialMap → Hypermap`.
- `CombinatorialMap → GMap 2`.
- Orientable closed `GMap 2 → CombinatorialMap`, with the required choice of orientation.
- `Cellulation2 → GMap 2`.
- `GMap 2 → Cellulation2` under the no-folding or surface hypotheses needed for well-defined cells.
- Polygonal schema to one-face map.
- Map to underlying Mathlib `Graph`.
- Conversion of loopless simple maps to `SimpleGraph`.

Every conversion has naturality under isomorphism.

#### Illustrative targets

```lean
def GMap.Cell (G : GMap n D) (i : Fin (n + 1)) : Type* := ...
def GMap.component : D → Finset D := ...
def GMap.IsConnected (G : GMap n D) : Prop := ...
def GMap.IsOrientable (G : GMap n D) : Prop := ...
def GMap.Orientation (G : GMap n D) : Type* := ...

def GMap2.IsSurface (G : GMap 2 D) : Prop := ...
def GMap2.eulerChar (G : GMap 2 D) : ℤ := ...

structure GMapIso (G : GMap n D) (H : GMap n E) where
  dartEquiv : D ≃ E
  commutes : ∀ i, dartEquiv ∘ G.adj i = H.adj i ∘ dartEquiv

def CombinatorialMap.toGraph
    (M : CombinatorialMap D) : Graph M.Vertex M.Edge := ...
```

#### Design notes

A “dart” in an oriented map is a directed half-edge. A “dart” in a Gmap is closer to a complete flag. The API and documentation must not silently identify them. Conversion theorems make the relation explicit.

An incidence graph is not a substitute for the ordered models because it loses multi-incidence. Damiand–Lienhardt gives concrete examples in which a face is incident twice to an edge or has two corners at the same vertex.

#### Acceptance tests

Construct finite encodings of:

- sphere;
- disk;
- annulus;
- torus;
- projective plane;
- Klein bottle;
- a one-vertex one-face torus;
- a map with a bridge;
- a map with a loop;
- a map with parallel edges.

Compute the cells, boundary components, orientability, and Euler characteristic in each representation and prove the conversions preserve them.

#### Unlocks

The realization bridge, finite surface operations, classification by combinatorial moves, embedded graph theory, and computational invariants.

---

### Layer 8: realization, round trips, and conservative operations

This is the architectural hinge of the roadmap.

#### From earlier layers

Finite 2-complex realization, combinatorial surface recognition, PL topology, triangulated surfaces, cellulations, maps, hypermaps, and Gmaps.

#### What to build

##### Flag triangulation and realization

For a 2-Gmap \(G\):

- create one abstract triangle for each dart or complete flag;
- label its three codimension-one faces by \(0,1,2\);
- glue equally labeled sides according to \(\alpha_0,\alpha_1,\alpha_2\);
- express the result as a finite abstract simplicial or semi-simplicial structure without silently identifying distinct simplices that share the same vertex set;
- construct its topological realization.

The implementation must confront the distinction between an abstract simplicial complex and a \(\Delta\)-complex or semi-simplicial set. One-vertex surface triangulations and repeated incidences cannot always be represented faithfully by a plain set-of-vertex-sets complex before subdivision. The canonical flag subdivision should land in an honest abstract simplicial complex.

Prove:

\[
G\text{ is a surface Gmap}
\quad\Longleftrightarrow\quad
\operatorname{Flag}(G)\text{ is a combinatorial surface}.
\]

Then prove that the realization is a compact topological surface.

##### Cellulation realization

Construct the polygon-attachment realization of a `Cellulation2`. Prove:

- compactness and Hausdorffness under the surface hypotheses;
- local disk or half-disk neighborhoods;
- agreement with the flag-triangulation realization;
- preservation under cellulation isomorphism.

##### Round-trip theorems

- A triangulated surface produces a flag Gmap.
- The flag complex of that Gmap is simplicially isomorphic to a barycentric subdivision of the original triangulation.
- A surface Gmap converted to a cellulation and then back to a Gmap is isomorphic to the appropriate canonical subdivision of the original.
- An oriented map converted to a Gmap and back after choosing the induced orientation recovers the original map up to isomorphism.
- The underlying topological surface is unchanged by every round trip.

The expected central statements have the shape:

```lean
def GMap2.flagComplex (G : GMap 2 D) :
    AbstractSimplicialComplex (FlagVertex G) := ...

theorem isSurface_iff_flagComplex
    (G : GMap 2 D) :
    G.IsSurface ↔ G.flagComplex.IsCombinatorialSurface := ...

theorem triangulation_toGMap_flagComplex
    (K : FiniteTriangulatedSurface) :
    Nonempty
      (K.toGMap.flagComplex ≅ K.complex.barycentricSubdivision) := ...
```

##### Conservative operations

Develop surface-preserving operations with exact hypotheses:

- sew and unsew;
- insertion and expansion;
- removal and contraction;
- edge subdivision and face triangulation;
- vertex splitting;
- safe deletion of a dangling tree;
- duality;
- cutting along an embedded combinatorial arc;
- cutting along a two-sided or one-sided simple closed curve;
- sewing boundary components;
- capping a boundary circle with a disk;
- puncturing a surface;
- connected sum;
- orientation double cover at the finite level.

For each operation prove:

- how \(V,E,F\) and boundary components change;
- whether connectedness and orientability are preserved;
- how Euler characteristic changes;
- a homeomorphism or PL homeomorphism between the new realization and the intended topological cut, quotient, or gluing.

Damiand–Lienhardt’s removal, contraction, insertion, expansion, sewing, duality, triangulation, and classification machinery is the principal design reference. 

##### Embedded graph regular neighborhoods

For a finite graph embedded as a subcomplex of a surface:

- construct its regular neighborhood;
- compute the induced boundary components and rotation system;
- prove uniqueness up to ambient isotopy;
- recover the surface-with-boundary represented by the corresponding ribbon graph;
- prove that every compact connected surface with nonempty boundary has a finite graph spine.

Cannon includes the graph-spine statement as an exercise immediately after triangulation.

#### Design notes

The realization theorem, not the raw permutation structure, determines whether the library is topologically sound.

Do not attach an arbitrary `realization : Type*` field to a finite structure. Realization is a canonical construction, and compatibility is a theorem. The existing surface-classification formalization adopted this design after removing arbitrary realization fields from its finite cell complex.

#### Acceptance criteria

For all standard examples, prove that:

\[
\chi_{\mathrm{orbit}}
=
\chi_{\mathrm{cell}}
=
\chi_{\mathrm{simplicial}}.
\]

Prove that dualizing twice recovers the original finite map up to isomorphism and that the realizations are canonically homeomorphic.

#### Unlocks

A representation-independent classification proof, finite homology, and graph embeddings on surfaces.

---

### Layer 9: polygonal schemas and existence of normal forms

This layer proves that every compact connected surface has a canonical kind of finite presentation. It is the existence half of classification.

#### From earlier layers

Triangulation, cellulations, Gmaps, regular neighborhoods, cutting and sewing, cyclic words, and realization-preserving finite operations.

#### What to build

##### Reduction to a polygonal schema

For a connected closed cellulated surface:

1. choose a spanning tree in the 1-skeleton and contract it safely;
2. choose a spanning tree in the dual graph and delete the corresponding primal edges safely;
3. reduce to a one-vertex, one-face cellulation;
4. extract a polygonal boundary word in which each edge label occurs exactly twice;
5. prove that the polygonal quotient is homeomorphic to the original surface.

For surfaces with boundary, produce a corresponding schema with explicit unpaired boundary components or reduce to a punctured closed normal form.

##### Elementary word moves

Formalize a complete finite move calculus, including:

- cyclic rotation and reversal;
- renaming of letters;
- cancellation of adjacent inverse pairs;
- cutting and regluing a polygon along an embedded diagonal;
- collecting an orientable handle pair;
- collecting a crosscap;
- trading a handle plus a crosscap for three crosscaps;
- moving boundary components into standard position.

Each move has a polygonal-realization homeomorphism.

##### Normal forms

Define finite normal forms:

- sphere;
- orientable genus \(g\):
  \[
  a_1b_1a_1^{-1}b_1^{-1}\cdots
  a_gb_ga_g^{-1}b_g^{-1};
  \]
- nonorientable crosscap number \(k\):
  \[
  a_1a_1a_2a_2\cdots a_ka_k;
  \]
- the corresponding forms with \(b\) boundary components.

Prove existence of reduction to one of these forms.

##### Connected sums

Define connected sum topologically, PL-combinatorially, and by polygonal words. Prove these definitions agree.

Prove:

- \(S\#S^2\cong S\);
- associativity and commutativity up to homeomorphism;
- orientable genus is additive;
- crosscap number is additive in the nonorientable family;
- the Klein bottle is \(\mathbb{RP}^2\#\mathbb{RP}^2\);
- \(T^2\#\mathbb{RP}^2\cong\#^3\mathbb{RP}^2\).

Cannon presents the surface structure theorem through connected sums of spheres, tori, projective planes, and Klein bottles, then reduces the list using these relations. 

#### Illustrative targets

```lean
structure PolygonalSchema where
  edgeLabel : Type
  finite_edgeLabel : Fintype edgeLabel
  boundaryWord : CyclicWord (Signed edgeLabel)
  paired : ∀ e, occurrenceCount e = 2
  localSurface : ...

def PolygonalSchema.realization : TopCat := ...

theorem cellulation_has_schema
    (C : ConnectedClosedSurfaceCellulation) :
    ∃ P : PolygonalSchema,
      Nonempty (C.realization ≃ₜ P.realization) := ...

inductive SchemaMove : PolygonalSchema → PolygonalSchema → Prop := ...

theorem schema_reduces_to_normalForm
    (P : ConnectedSurfaceSchema) :
    ∃ N : SurfaceNormalForm,
      Relation.ReflTransGen SchemaMove P N.schema := ...
```

#### Design notes

Spanning-tree and dual-tree reduction is preferable to a theorem-specific sequence of arbitrary edge operations because it supplies a reusable canonical proof pattern and directly connects to graph-on-surface theory.

#### Unlocks

Existence classification, explicit homology calculations, and algorithms computing the normal-form invariants of a finite map.

---

### Layer 10: finite combinatorial homology, Euler characteristic, and orientability

This layer develops only the homology naturally required by finite one- and two-dimensional topology.

It does not attempt to replace Mathlib’s general singular homology.

#### From Mathlib and earlier layers

Free modules, matrices, chain complexes and homology, Euler characteristic of homological complexes, finite simplicial complexes, oriented cells, cellulations, subdivisions, and normal forms.

#### What to build

##### Simplicial chains

For a finite abstract complex and a commutative ring \(R\):

- oriented simplices;
- free \(R\)-modules of \(k\)-chains;
- boundary maps with signs;
- \(\partial^2=0\);
- induced chain maps for simplicial maps;
- chain isomorphisms for simplicial isomorphisms;
- subdivision chain maps;
- relation to `SSet.chainComplex`.

##### Cellular chains in dimension two

For an oriented finite cellulation:

\[
C_2 \xrightarrow{\partial_2} C_1 \xrightarrow{\partial_1} C_0.
\]

Define:

- \(\partial_1\) from oriented edge endpoints;
- \(\partial_2\) from signed occurrences in face boundary words;
- proof that \(\partial_1\partial_2=0\);
- chain maps induced by cellulation isomorphisms and conservative operations;
- comparison with the flag triangulation chain complex.

##### \(H_0\) and components

Prove that reduced \(H_0\) detects connected components for finite complexes and cellulations. Relate the computational component partition to homology.

##### Euler–Poincaré

Define:

\[
\chi(K)=f_0-f_1+f_2
\]

and prove:

\[
\chi(K)
=
\operatorname{rank} H_0(K;F)
-
\operatorname{rank} H_1(K;F)
+
\operatorname{rank} H_2(K;F)
\]

over a field \(F\), using Mathlib’s homological Euler-characteristic API.

Prove equality among:

\[
\chi_{\mathrm{Gmap\ orbit}},
\quad
\chi_{\mathrm{cell}},
\quad
\chi_{\mathrm{simplicial}},
\quad
\chi_{\mathrm{homological}}.
\]

Prove invariance under:

- subdivision;
- Pachner moves;
- cell insertion/removal pairs;
- map and Gmap isomorphism;
- homeomorphism of compact surfaces, using the two-dimensional Hauptvermutung.

Cannon first proves invariance under simple subdivision and then invokes topological invariance.

##### Orientability and the fundamental cycle

For a connected closed surface \(S\), prove equivalent:

- coherent orientation of all 2-cells;
- Gmap dart bipartition;
- every closed curve preserves local orientation;
- existence of an integral cellular 2-cycle using every face with coefficient \(\pm1\);
- \(H_2(S;\mathbb Z)\cong\mathbb Z\).

For a connected nonorientable closed surface, prove:

\[
H_2(S;\mathbb Z)=0.
\]

For every connected closed surface, prove:

\[
H_2(S;\mathbf F_2)\cong\mathbf F_2.
\]

For surfaces with boundary, formulate and prove the corresponding relative fundamental-cycle statement only if the necessary relative chain-complex API is built explicitly in this layer. Do not silently invoke general Poincaré–Lefschetz duality.

##### Homology of normal forms

Compute \(H_0,H_1,H_2\) over \(\mathbb Z\) and \(\mathbf F_2\) for:

- \(S^2\);
- the disk and annulus;
- orientable genus-\(g\) surfaces;
- nonorientable crosscap-\(k\) surfaces;
- versions with boundary.

In particular:

\[
H_1(\Sigma_g;\mathbb Z)\cong\mathbb Z^{2g},
\]

and for the closed nonorientable surface \(N_k\),

\[
H_1(N_k;\mathbb Z)\cong
\mathbb Z^{k-1}\oplus\mathbb Z/2\mathbb Z.
\]

##### Low-dimensional complement applications

As comparisons with Cannon’s homological route, prove:

- an arc does not separate \(S^2\);
- a Jordan curve complement has reduced \(H_0\cong\mathbb Z\);
- the two Jordan components correspond to the two generators modulo the reduced relation.

These are applications after Jordan–Schönflies has already been proved geometrically; they are not prerequisites for Layer 3.

#### Illustrative targets

```lean
def FiniteSimplicialComplex.chainComplex
    (K : FiniteSimplicialComplex) (R : Type*) [CommRing R] :
    ChainComplex (ModuleCat R) ℕ := ...

def Cellulation2.chainComplex
    (C : Cellulation2) (R : Type*) [CommRing R] :
    ChainComplex (ModuleCat R) ℕ := ...

theorem cellular_boundary_sq_zero :
    C.boundary₂ ≫ C.boundary₁ = 0 := ...

theorem eulerChar_eq_homologyEulerChar
    (C : FiniteSurfaceCellulation) (F : Type*) [Field F] :
    C.eulerChar = (C.chainComplex F).homologyEulerChar := ...

theorem orientable_iff_topHomology
    (C : ConnectedClosedSurfaceCellulation) :
    C.IsOrientable ↔
      Nonempty ((C.chainComplex ℤ).homology 2 ≅ ModuleCat.of ℤ ℤ) := ...
```

#### Design notes

Do not prove topological invariance of the cell count by appealing to classification; that would make Euler characteristic unusable in the uniqueness proof. Use subdivision/Hauptvermutung or the chain-complex comparison.

The full theories of singular homology, Mayer–Vietoris, universal coefficients, cohomology, cup products, and Poincaré duality remain outside this roadmap.

#### Unlocks

Uniqueness classification, mapping-class actions on \(H_1\), Mac Lane’s criterion, and graph-genus bounds.

---

### Layer 11: classification of compact one- and two-manifolds

This layer combines the normal-form existence theorem with the independent invariants.

#### From earlier layers

Radó triangulation, Hauptvermutung, polygonal normal forms, Euler characteristic, orientability, boundary components, and finite homology.

#### What to build

##### Compact 1-manifolds

Prove:

- every connected compact 1-manifold without boundary is homeomorphic to \(S^1\);
- every connected compact 1-manifold with nonempty boundary is homeomorphic to a closed interval;
- every compact 1-manifold is a finite disjoint union of circles and intervals.

Relate this to combinatorial 1-manifolds and boundary links.

##### Closed connected surfaces

Prove the complete classification:

Two compact connected closed surfaces are homeomorphic if and only if they have the same Euler characteristic and the same orientability.

Equivalently, every such surface is uniquely one of:

- \(S^2\);
- an orientable genus-\(g\) surface;
- a nonorientable crosscap-\(k\) surface.

Cannon states the classification in exactly the Euler-characteristic-plus-orientability form.

##### Surfaces with boundary

Prove:

Two compact connected surfaces with boundary are homeomorphic if and only if they have:

- the same orientability;
- the same Euler characteristic;
- the same number of boundary components.

Equivalently, classify them by:

- orientable genus \(g\) and boundary count \(b\); or
- nonorientable crosscap number \(k\) and boundary count \(b\).

Prove:

\[
\chi(\Sigma_{g,b})=2-2g-b,
\qquad
\chi(N_{k,b})=2-k-b.
\]

##### Disconnected compact surfaces

Classify a disconnected compact surface by the finite multiset of the classification data of its connected components.

Do not encode this as an ordered list.

##### Compatibility of classification presentations

Prove that the following algorithms or constructions return the same invariants:

- classification from a triangulation;
- classification from a cellulation;
- classification from a Gmap;
- classification from a polygonal word;
- classification from finite homology plus orientability and boundary count.

##### Lean-Eval bridge

Supply a short bridge from the clean library theorem to the exact Lean-Eval statement, including the specified quotient representatives.

The bridge is not the classification proof and should contain little mathematics.

#### Illustrative targets

```lean
inductive ConnectedSurfaceType
  | orientable (genus boundary : ℕ)
  | nonorientable (crosscaps : ℕ) (hpos : 0 < crosscaps) (boundary : ℕ)

def ConnectedSurfaceType.model : ConnectedSurfaceType → TopCat := ...

theorem compact_connected_surface_classification
    [CompactSpace M] [ConnectedSpace M]
    [TopologicalSurfaceWithBoundary M] :
    ∃! t : ConnectedSurfaceType,
      Nonempty (M ≃ₜ t.model) := ...

theorem homeomorphic_iff_invariants
    (M N : CompactConnectedSurface) :
    Nonempty (M ≃ₜ N) ↔
      M.IsOrientable = N.IsOrientable ∧
      M.eulerChar = N.eulerChar ∧
      M.boundaryComponentCount = N.boundaryComponentCount := ...
```

#### Acceptance criteria

The theorem must cover:

- empty and nonempty boundary;
- orientable and nonorientable cases;
- exact uniqueness, not only existence of a normal form;
- the standard sphere, torus, projective plane, and Klein bottle examples;
- compatibility with the finite map and Gmap classifiers.

#### Unlocks

Top–PL–Smooth comparison, mapping class groups, graph embeddings by surface type, and explicit graph-genus invariants.

---

### Layer 12: comparison of topological, PL, and smooth surfaces

The slogan “Top \(=\) PL \(=\) Smooth in dimension two” needs precise statements. The categories with all continuous, PL, and smooth maps are not literally equivalent because their morphism sets differ.

This layer proves equivalence at the level of structures, isomorphism classes, and isotopy groupoids.

#### From Mathlib and earlier layers

Topological and smooth manifold infrastructure; surface triangulation; the two-dimensional Hauptvermutung; PL approximation and isotopy; compact surface classification.

#### What to build

##### Topological to PL

- Every compact topological surface admits a PL structure, supplied by a triangulation.
- Any two triangulations of the same topological surface determine equivalent PL structures.
- Every homeomorphism of compact PL surfaces is isotopic to a PL homeomorphism.
- Relative versions fix the boundary pointwise or preserve a specified subcomplex.
- Consequently, homeomorphism classes and PL-homeomorphism classes of compact surfaces agree.

##### PL to smooth

- Every PL surface admits a compatible smooth structure.
- A PL homeomorphism is isotopic to a diffeomorphism after choosing compatible smooth structures.
- Any two compatible smooth structures on a fixed PL surface are diffeomorphic.
- Relative boundary versions.
- Consequently, PL-homeomorphism classes and diffeomorphism classes of compact surfaces agree.

The proof route should be combinatorial smoothing of triangulations and PL transition maps, not uniformization or a classification by constant-curvature metrics.

##### Mapping-class comparison

For a compact surface \(S\), prove natural isomorphisms among:

- homeomorphisms modulo topological isotopy;
- PL homeomorphisms modulo PL isotopy;
- diffeomorphisms modulo smooth isotopy;

with orientation-preserving and boundary-fixing versions.

##### Precise categorical statement

Define groupoids whose:

- objects are compact surfaces with the indicated structure;
- morphisms are isomorphisms in that structure;
- or, at the mapping-class level, isotopy classes of those isomorphisms.

Prove equivalences of these groupoids after forgetting structure. Do not claim an equivalence between categories containing all continuous, PL, and smooth maps.

#### Illustrative targets

```lean
theorem topologicalSurface_hasPLStructure
    (M : CompactTopologicalSurface) :
    Nonempty (PLStructure₂ M) := ...

theorem plStructure_unique
    (P Q : PLStructure₂ M) :
    Nonempty (P ≃ₚₗ Q) := ...

theorem homeomorph_isotopic_pl
    (h : M ≃ₜ N) :
    ∃ hPL : M ≃ₚₗ N, Isotopic h hPL.toHomeomorph := ...

theorem plSurface_hasSmoothStructure
    (M : CompactPLSurface) :
    Nonempty (SmoothStructure M) := ...

theorem plHomeomorph_isotopic_diffeomorph
    (h : M ≃ₚₗ N) :
    ∃ f : M ≃ₘ N, Isotopic h.toHomeomorph f.toHomeomorph := ...

theorem mappingClassGroup_top_pl_smooth :
    MappingClassGroupTop M ≃*
      MappingClassGroupPL M ≃*
      MappingClassGroupSmooth M := ...
```

#### Design notes

The smooth structure is used only to establish the low-dimensional comparison. Tangent bundles, curvature, Riemannian metrics, and analytic surface theory are not developed.

The adapter from this intrinsic two-dimensional PL structure to `GeometricTopology`’s general chart-based `PLGroupoid` is owned by that roadmap.

#### Unlocks

A representation-independent mapping class group and the surface-diffeomorphism input required by Heegaard splittings.

---

### Layer 13: curves on surfaces and mapping class groups

This layer develops the first structural theory above classification while staying entirely topological and combinatorial.

#### From earlier layers

Surface classification, regular neighborhoods, cutting and sewing, Top–PL–Smooth comparison, finite homology, torus models, and ambient isotopy.

#### What to build

##### Embedded curves and arcs

- Simple closed curves and properly embedded arcs in a surface.
- One-sided and two-sided curves, characterized through their regular neighborhoods.
- Separating and nonseparating curves.
- Null-homotopic, boundary-parallel, and essential curves.
- Cutting along an arc or simple closed curve.
- Effect of cutting on connectedness, boundary count, orientability, and Euler characteristic.
- Classification of regular neighborhoods of curves as annuli or Möbius bands.
- Bigon removal and minimal position.
- Geometric intersection number as the minimum over isotopy classes.
- Basic properties and invariance under homeomorphisms.

##### Curves on the torus

Following Cannon’s torus chapter:

- explicit quotient model \(\mathbb R^2/\mathbb Z^2\);
- straightening of essential simple closed curves;
- classification by primitive integer pairs up to sign;
- slope notation;
- intersection number
  \[
  i((p,q),(r,s))=|ps-qr|.
  \]

Cannon constructs and straightens simple closed curves of rational slope on the torus.

##### Mapping class groups

Define:

- homeomorphism group;
- orientation-preserving subgroup;
- boundary-fixing subgroup;
- mapping class group as isotopy classes;
- action on isotopy classes of curves;
- action on \(H_1\).

Use Layer 12 to identify the topological, PL, and smooth versions.

##### Dehn twists

- Construction from an annular neighborhood.
- Independence of the chosen annular chart up to isotopy.
- Inverse twist.
- Conjugacy under homeomorphisms.
- Commutation for disjoint curves.
- Basic braid relation for curves intersecting once.
- Action on \(H_1\).
- Triviality for inessential curves under the precise boundary hypotheses.

##### Torus mapping class group

Prove:

\[
\operatorname{Mod}(T^2)\cong GL(2,\mathbb Z),
\qquad
\operatorname{Mod}^+(T^2)\cong SL(2,\mathbb Z).
\]

Identify standard Dehn twists with elementary matrices.

##### Dehn–Lickorish generation

For each compact connected orientable surface of finite type, prove that the orientation-preserving mapping class group is generated by a standard finite family of Dehn twists, with a boundary-fixing version.

The theorem statement must name the explicit standard curves and the corresponding finite generating set.

#### Illustrative targets

```lean
def SimpleClosedCurve (M : Type*) := { e : Circle → M // Topology.IsEmbedding e }

def SimpleClosedCurve.IsTwoSided (c : SimpleClosedCurve M) : Prop := ...
def SimpleClosedCurve.IsEssential (c : SimpleClosedCurve M) : Prop := ...
def geometricIntersection
    (a b : IsotopyClass (SimpleClosedCurve M)) : ℕ := ...

def MappingClassGroup (M : Type*) :=
  QuotientGroup (HomeomorphGroup M) (isotopyNormalSubgroup M)

def dehnTwist (c : OrientedTwoSidedCurve M) : MappingClassGroup M := ...

theorem torus_mappingClassGroup :
    MappingClassGroupPlus Torus ≃* Matrix.SpecialLinearGroup (Fin 2) ℤ := ...

theorem dehn_lickorish
    (S : CompactConnectedOrientableSurface) :
    Subgroup.closure (standardDehnTwists S) = ⊤ := ...
```

#### Design notes

The group quotient should use an explicitly proved normal subgroup of homeomorphisms isotopic to the identity. Isotopy classes of curves and mapping classes should not be represented by arbitrary chosen normal forms.

Nielsen–Thurston classification, curve complexes, Teichmüller theory, and pseudo-Anosov dynamics are excluded.

#### Unlocks

Heegaard splitting constructions and a robust language for surface automorphisms.

---

### Layer 14: graphs embedded in surfaces and planar graph theorems

Graphs on surfaces are not the primary subject of the roadmap, but a selected structural layer is necessary both to validate the map library and to reach several celebrated theorems.

Mohar–Thomassen’s relevant core consists of Jordan–Schönflies, planarity criteria, 3-connected planar graphs and duality, rotation systems and embedding schemes, cellular embeddings, graph genus, and elementary coloring results. The roadmap does not attempt to formalize the book’s width theory, algorithms, or general excluded-minor machinery.

#### From Mathlib and earlier layers

Mathlib multigraphs and simple graphs, graph coloring, finite homology, regular neighborhoods, surface maps and Gmaps, surface classification, duality, and Euler characteristic.

#### What to build

##### Topological realization of a multigraph

For a finite `Graph V E`, construct its one-dimensional CW or simplicial realization, preserving loop and edge identity.

Prove:

- vertices embed as 0-cells;
- edge interiors are disjoint;
- graph isomorphisms induce homeomorphisms;
- connectedness agrees with connectedness of the realization;
- subdivisions of edges preserve the homeomorphism type.

##### Graph embeddings

Define a graph embedding in a surface as an embedding of its realization.

Develop:

- equivalence under graph isomorphism and surface homeomorphism;
- faces as connected components of the complement;
- cellular or 2-cell embeddings;
- boundary walks of faces;
- induced map or Gmap;
- induced rotation system;
- orientability signatures;
- deletion and contraction of embedded edges;
- restriction to subgraphs;
- bridges and loops;
- graph genus and nonorientable genus.

##### Rotation systems and embedding schemes

For an orientable surface, a rotation system is a cyclic order of incident darts at every vertex.

For nonorientable surfaces, use a signed rotation system or embedding scheme carrying the edge-twist data.

Prove the Heffter–Edmonds–Ringel rotation principle:

- every cellular embedding of a finite graph in an oriented surface induces a rotation system;
- every rotation system determines an oriented cellular surface embedding;
- the constructions are inverse up to map isomorphism and orientation-preserving homeomorphism;
- the signed version classifies cellular embeddings in arbitrary compact surfaces.

This theorem should be phrased through the combinatorial-map realization developed in Layers 7–8, not by constructing a second embedding theory.

##### Euler formula and genus

For a connected cellular embedding in a compact connected closed surface:

\[
|V|-|E|+|F|=\chi(S).
\]

For orientable genus \(g\):

\[
|V|-|E|+|F|=2-2g.
\]

For nonorientable crosscap number \(k\):

\[
|V|-|E|+|F|=2-k.
\]

Lando–Zvonkin presents Euler characteristic as a map invariant and exhibits the same abstract graph embedded with different genera, illustrating why embedding data is essential. 

Develop the standard consequences:

- edge bounds for simple planar graphs;
- triangle-free planar edge bounds;
- existence of a vertex of degree at most five;
- nonplanarity of \(K_5\) and \(K_{3,3}\);
- lower bounds for graph genus.

##### Planar duality

For a connected plane map:

- construct the dual multigraph;
- prove duality exchanges vertices and faces;
- identify loops with primal bridges and vice versa;
- prove double duality up to map isomorphism;
- prove cut–cycle duality over \(\mathbf F_2\);
- relate primal and dual spanning trees.

##### Kuratowski’s theorem

For finite simple graphs, prove:

\[
G\text{ is planar}
\quad\Longleftrightarrow\quad
G\text{ contains no subdivision of }K_5\text{ or }K_{3,3}.
\]

The roadmap must build all required notions:

- graph subdivision and topological minor;
- planar embedding;
- bridges of a subgraph;
- overlap or conflict graph of bridges relative to a cycle;
- the finite induction or decomposition used in the selected proof.

A theorem only for one implication does not discharge this target.

##### Whitney’s unique-embedding theorem

Prove:

> Every 3-connected planar graph has a unique embedding in \(S^2\), up to a graph automorphism, an ambient homeomorphism of \(S^2\), and reversal of orientation.

Name it `whitney_uniqueEmbedding` or similar, not `whitney_embedding`, to avoid collision with the manifold embedding theorem.

Develop the required structural theory:

- blocks and 2-/3-connectivity;
- facial cycles;
- peripheral cycles;
- preservation of facial cycles under embeddings;
- equivalence of rotation systems.

##### Mac Lane’s planarity criterion

For a finite graph, define its cycle space over \(\mathbf F_2\). Prove:

> A finite graph is planar if and only if its cycle space has a basis in which every edge occurs in at most two basis cycles.

Relate the forward direction to face boundaries and the reverse direction to construction of a surface cellulation with Euler characteristic two.

This target is an important test that the finite homology and graph-embedding layers communicate.

##### Five-color theorem

Prove every finite planar simple graph is 5-colorable, using Mathlib’s existing coloring API for the statement.

The proof should use:

- Euler’s degree bound;
- induction on vertices;
- Kempe-chain recoloring.

```lean
theorem planar_five_color
    (G : SimpleGraph V) [Fintype V]
    (hG : IsPlanar G) :
    G.Colorable 5 := ...
```

#### Design notes

Planarity is existence of an embedding into \(S^2\), not existence of coordinates in \(\mathbb R^2\). Plane maps carry a chosen embedding; planar graphs merely admit one.

The dual of a simple plane graph need not be simple. Duality therefore lives in the multigraph/map layer even when the primal theorem begins with a `SimpleGraph`.

The graph-genus minimum is defined only after proving that the set of embedding genera is nonempty for finite graphs. A finite graph always embeds in some compact orientable surface by thickening an arbitrary drawing or rotation system.

#### Excluded graph-on-surface material

This roadmap does not build:

- planarity-testing algorithms;
- minimum-genus algorithms;
- face-width or edge-width theory;
- treewidth;
- the Robertson–Seymour graph-minor structure theorem;
- excluded-minor characterizations for each fixed surface;
- graph-linking theorems in higher-dimensional manifolds;
- maximum genus as a developed subject;
- complete-graph genus formulas;
- spectral graph theory on surfaces.

#### Unlocks

A mature finite-map library, celebrated planar graph theorems, and the precise substrate needed by a future map-coloring roadmap.

## Roadmap-for-a-roadmap: Four Color and Map Color

This section is motivation for a separate future roadmap. It is not work in this roadmap. Contributors should not claim or implement tasks from this section under the present roadmap.

The Four Color Theorem would consume:

- the plane-map and duality API;
- triangulation of plane maps;
- graph coloring;
- Kempe chains;
- reducible configurations;
- discharging;
- a formally specified finite computation with independently checkable certificates.

The Coq/Rocq proof demonstrates that hypermaps are an effective foundation and ships a substantial theory of combinatorial hypermaps alongside the theorem.

The Map Color Theorem would consume:

- classification of compact surfaces;
- rotation systems and embedding schemes;
- graph genus;
- Euler bounds;
- constructions of high-chromatic embeddings;
- the Four Color Theorem as the genus-zero case;
- the Ringel–Youngs theory for complete-graph embeddings.

These subjects are large enough to require a dedicated roadmap with its own finite-computation, enumeration, and construction conventions. The present roadmap ends with the five-color theorem and the structural embedding theory.

## Out of scope

The following subjects are not part of this roadmap.

### Metric and analytic surface geometry

- Riemannian metrics;
- intrinsic metrics on polyhedral surfaces;
- curvature;
- Gauss–Bonnet;
- constant-curvature geometry;
- hyperbolic surfaces;
- uniformization;
- Teichmüller theory;
- conformal structures;
- Riemann surfaces;
- complex curves;
- Hodge theory and global analysis.

The sole smooth content is the low-dimensional Top–PL–Smooth comparison of Layer 12.

### Convex polytope theory

- regular solids;
- face lattices of convex polytopes as a subject;
- Steinitz’s theorem;
- realization spaces;
- shellability motivated primarily by convex polytopes.

Convex simplices and polygonal cells remain available as technical devices in PL topology.

### Noncompact-surface classification

The Kerékjártó–Richards classification by spaces of ends and genus accumulating at ends is excluded. Locally finite triangulations of noncompact surfaces may appear as lemmas used by Radó-style arguments, but no classification theorem for noncompact surfaces is targeted.

### General high-dimensional PL topology

- arbitrary-dimensional Hauptvermutung;
- high-dimensional triangulation obstructions;
- Kirby–Siebenmann classes;
- general smoothing obstruction theory;
- Whitehead torsion;
- \(h\)- and \(s\)-cobordism;
- Zeeman’s conjecture;
- arbitrary-dimensional regular-neighborhood theory.

These belong to `GeometricTopology`.

### General algebraic topology

- a second implementation of singular homology;
- general Mayer–Vietoris;
- cohomology and cup products;
- universal coefficient theorems;
- general Poincaré or Poincaré–Lefschetz duality;
- spectral sequences;
- characteristic classes.

Only the finite one- and two-dimensional chain theory of Layer 10 is built here.

### Dessins, branched coverings, and map enumeration

The permutation representation also leads to constellations, branched coverings, dessins d’enfants, Hurwitz numbers, moduli spaces, matrix integrals, and Galois actions. Lando–Zvonkin develops these connections, but they are outside this roadmap. Only the foundational map and hypermap theory in its first chapter is used.

### Computational geometry and image processing

Geometric embedding attributes, CAD data structures, mesh optimization, image segmentation, chamfering for graphics, and numerical surface algorithms are outside scope. Damiand–Lienhardt remains a reference for the topological data structures and operations, not for its application layers.

### Higher mapping-class theory

- Nielsen–Thurston classification;
- pseudo-Anosov maps;
- curve and arc complexes;
- Teichmüller and moduli spaces;
- measured foliations;
- representation stability of mapping class groups.

## Acceptance criteria

The roadmap is complete when all of the following hold.

### Dependency and code quality

- Every file imports only Mathlib, Tau Ceti core utilities, or earlier files belonging to this roadmap.
- There are no `sorry`s or new axioms.
- Definitions use Mathlib vocabulary and existing structures where available.
- Every first-class object has extensionality, isomorphism, transport, and basic construction APIs.
- Current open Mathlib work has been checked before duplicating an API; the Tau Ceti implementation follows the upstream shape where one exists.
- Existing external formalizations are attributed and integrated only after satisfying Tau Ceti’s coordination and licensing requirements.

### Plane topology

The library proves:

- polygonal Jordan separation;
- polygonal Schönflies;
- full Jordan separation with common-boundary conclusions;
- full ambient Schönflies;
- arc non-separation;
- characterization of the 2-sphere;
- tameness of arcs in \(S^2\);
- tameness of planar Cantor sets;
- Moore’s decomposition theorem;
- invariance of domain in dimension two.

### PL topology

The library provides:

- finite realizations of abstract simplicial complexes;
- stars, links, joins, cones, subdivisions, and barycentric subdivision;
- PL maps via subdivisions;
- local recognition of combinatorial surfaces;
- collars and regular neighborhoods in surfaces;
- collapses and shellings needed for disks and annuli;
- finite triangulations of compact surfaces with boundary;
- the two-dimensional Hauptvermutung;
- relative two-dimensional Pachner equivalence.

### Finite combinatorial topology

The library provides:

- finite 2-cellulations with multi-incidence;
- oriented maps;
- hypermaps;
- dimension-polymorphic Gmaps and a complete 2-Gmap surface API;
- boundary, orientation, duality, cells, components, and Euler characteristic;
- conservative operations with semantic preservation theorems;
- conversions among maps, hypermaps, Gmaps, cellulations, polygonal schemas, and triangulations;
- round-trip theorems identifying the resulting subdivisions;
- canonical realization as compact surfaces.

For the sphere, disk, annulus, torus, projective plane, and Klein bottle, all representations compute the expected cells, boundary count, orientability, and Euler characteristic.

### Classification and homology

The library proves:

- classification of compact 1-manifolds;
- existence and uniqueness of normal forms for compact connected surfaces;
- classification with boundary;
- disconnected classification by finite multisets of component types;
- \(\chi=V-E+F\) in every finite presentation;
- Euler–Poincaré;
- \(H_0,H_1,H_2\) of all normal forms over \(\mathbb Z\) and \(\mathbf F_2\);
- equivalence of combinatorial and homological orientability;
- agreement of orbit, cellular, simplicial, and homological Euler characteristics;
- a short bridge to the exact Lean-Eval surface-classification statement.

### Structure comparison and mapping classes

The library proves:

- existence and uniqueness of PL structures on compact topological surfaces;
- existence and uniqueness of compatible smooth structures on compact PL surfaces;
- isotopy of homeomorphisms to PL homeomorphisms;
- isotopy of PL homeomorphisms to diffeomorphisms;
- agreement of topological, PL, and smooth mapping class groups;
- torus-curve classification and intersection formula;
- the torus mapping class group computation;
- Dehn–Lickorish generation.

### Embedded graphs

The library proves:

- realization and embedding theory for finite multigraphs;
- equivalence between cellular embeddings and combinatorial maps;
- the orientable and signed Heffter–Edmonds–Ringel rotation principles;
- planar duality and cut–cycle duality;
- Euler’s formula for cellular embeddings;
- graph-genus basics;
- Kuratowski’s theorem;
- Whitney’s unique-embedding theorem;
- Mac Lane’s criterion;
- the five-color theorem.

### Documentation and discoverability

- Every named theorem has a module docstring explaining its mathematical role and principal dependencies.
- Every representation-conversion file states which information is forgotten or chosen.
- Composition conventions for permutations are documented and tested on examples.
- Boundary conventions are documented in one place and reused.
- The main README includes a diagram showing the conversions among representations.
- Executable examples demonstrate orbit computation, orientability, Euler characteristic, normal-form classification, and duality.

## References

### Plane and surface topology

- James W. Cannon, *Topology as Fluid Geometry: Two-Dimensional Spaces, Volume 2*, AMS, 2017. Chapters 5–16 are the principal breadth and exposition reference for the point-set-topological branch.
- Edwin E. Moise, *Geometric Topology in Dimensions 2 and 3*, Springer, 1977. Principal reference for low-dimensional triangulation, PL approximation, and surface topology.
- James R. Munkres, *Topology*, 2nd ed., Prentice Hall, 2000. General point-set topology and the standard polygonal classification proof. 
- M. H. A. Newman, *Elements of the Topology of Plane Sets of Points*, Cambridge University Press.
- R. L. Wilder, *Topology of Manifolds*, AMS Colloquium Publications 32, 1949.
- Vicente Muñoz, Ángel González-Prieto, and Juan Ángel Rojo, *Geometry and Topology of Manifolds: Surfaces and Beyond*, AMS GSM 208, 2020. Chapters 1 and the finite/simplicial portions of Chapter 2 are in scope; the metric, complex, and analytic chapters are not.
- S. S. Cairns, “An elementary proof of the Jordan–Schönflies theorem,” *Proceedings of the AMS* 2 (1951), 860–867.
- T. Radó, “Über den Begriff der Riemannschen Fläche,” *Acta Litterarum ac Scientiarum Regiae Universitatis Hungaricae Francisco-Josephinae* 2 (1925), 101–121.
- P. H. Doyle and D. A. Moran, “A short proof that compact 2-manifolds can be triangulated.”
- Kushal Lalwani, *Triangulation and Classification of 2-Manifolds*.
- David B. A. Epstein, “Curves on 2-manifolds and isotopies,” *Acta Mathematica* 115 (1966), 83–107.

### PL topology and triangulations

- C. P. Rourke and B. J. Sanderson, *Introduction to Piecewise-Linear Topology*, Springer, 1972/1982.
- James R. Munkres, *Elementary Differential Topology*, Annals of Mathematics Studies 54, Princeton University Press, 1966.
- Udo Pachner, “P.L. homeomorphic manifolds are equivalent by elementary shellings,” *European Journal of Combinatorics* 12 (1991), 129–145.
- W. B. R. Lickorish, “Simplicial moves on complexes and manifolds,” *Geometry & Topology Monographs* 2 (1999), 299–320.
- Jean Gallier and Dianna Xu, *A Guide to the Classification Theorem for Compact Surfaces*. The polygonal-normalization and finite cell-complex arguments are especially relevant. 
- Allen Hatcher, *Algebraic Topology*, Cambridge University Press, 2002, for standard finite simplicial and cellular homology background.

### Combinatorial maps and surface cellulations

- Guillaume Damiand and Pascal Lienhardt, *Combinatorial Maps: Efficient Data Structures for Computer Graphics and Image Processing*, CRC Press, 2014. Chapters 2–6, 8, and 9 are the principal Gmap and conversion reference.
- Sergei K. Lando and Alexander K. Zvonkin, *Graphs on Surfaces and Their Applications*, Springer, 2004. Chapter 1 is the principal reference for maps, hypermaps, constellations, and the permutation model.
- A. Vince, “Combinatorial maps,” *Journal of Combinatorial Theory, Series B* 34 (1983), 1–21.
- J. R. Edmonds, “A combinatorial representation for polyhedral surfaces,” *Notices of the AMS* 7 (1960), 646.
- W. T. Tutte, “A census of planar maps,” *Canadian Journal of Mathematics* 15 (1963), 249–271.
- Christophe Dehlinger and Jean-François Dufourd, “Formalizing generalized maps in Coq,” *Theoretical Computer Science* 323 (2004), 351–397.
- Christophe Dehlinger and Jean-François Dufourd, “Formalizing the trading theorem in Coq,” *Theoretical Computer Science* 323 (2004), 399–442.
- Christophe Dehlinger and Jean-François Dufourd, “Formal specification and proofs for the topology and classification of combinatorial surfaces,” *Computational Geometry* 47 (2014), 869–890.
- Jean-François Dufourd, “Discrete Jordan Curve Theorem: A proof formalized in Coq with hypermaps,” *LIPIcs STACS 2008*, 253–264.
- Thomas Hales, “Hypermap,” in *Dense Sphere Packings: A Blueprint for Formal Proofs*, Cambridge University Press, 2012, 72–111.

### Graphs on surfaces and planarity

- Bojan Mohar and Carsten Thomassen, *Graphs on Surfaces*, Johns Hopkins University Press, 2001. The relevant material is Jordan–Schönflies, planarity, 3-connected plane graphs, duality, rotation systems, embedding schemes, cellular embeddings, graph genus, and elementary coloring.
- Jonathan Gross and Thomas Tucker, *Topological Graph Theory*, Wiley, 1987.
- Hassler Whitney, “Congruent graphs and the connectivity of graphs,” *American Journal of Mathematics* 54 (1932), 150–168.
- Kazimierz Kuratowski, “Sur le problème des courbes gauches en topologie,” *Fundamenta Mathematicae* 15 (1930), 271–283.
- Saunders Mac Lane, “A structural characterization of planar combinatorial graphs,” *Duke Mathematical Journal* 3 (1937), 460–472.
- Øystein Ore, *The Four-Color Problem*, Academic Press, 1967, for classical planar coloring arguments.
- Georges Gonthier, “Formal proof—the Four-Color Theorem,” *Notices of the AMS* 55 (2008), 1382–1393.
- The Rocq package `coq-fourcolor`, which contains the checked Four Color proof and its combinatorial hypermap theory.

### Mapping class groups

- Benson Farb and Dan Margalit, *A Primer on Mapping Class Groups*, Princeton University Press, 2012.
- W. B. R. Lickorish, “A finite set of generators for the homeotopy group of a 2-manifold,” *Proceedings of the Cambridge Philosophical Society* 60 (1964), 769–778.
- Max Dehn, classical papers on surface transformations and Dehn twists.
- David B. A. Epstein, “Curves on 2-manifolds and isotopies,” *Acta Mathematica* 115 (1966), 83–107.

### Formalization prior art

- SF Lean Meetup, `mccorvie/classification-of-surfaces`, complete Lean proof of the compact connected surface-classification Lean-Eval target.
- Rado Kirov et al., `rkirov/jordan_pick`, Lean formalizations of polygonal and continuous Jordan separation and related theorems.
- Georges Gonthier, Coq/Rocq Four Color development.
- Jean-François Dufourd and collaborators, Coq generalized-map and hypermap developments.
- Thomas Hales, HOL Light hypermap development for the Flyspeck project.