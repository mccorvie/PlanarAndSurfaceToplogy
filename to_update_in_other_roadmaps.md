### Cross-roadmap statements to record

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
  automatic. A simple closed curve has an annular neighbourhood when it is two-sided and a
  Möbius-band neighbourhood when it is one-sided.
- `GeometricTopology` layer 1: a line recording that the topological case of the boundary
  characterization rests on invariance of domain, proved here in dimension two.
- `ConformalMapping` layer 5 and `STATUS.md`: replace the open frontier item with a pointer
  here, naming `J ⊆ closure (filledHull J \ J)` as the milestone.






### Cross-roadmap statements to record

- `GeometricTopology` layer 9: record that the surface classification and the surface mapping class group listed as inputs are owned here.
- The same cross-links to `GeometricTopology` layers 1, 2, and 11 and to `ConformalMapping` that [PlanarTopology](../PlanarTopology/README.md) proposes; land them in whichever pull request merges first and reference both new roadmaps.



## Coordination note

Search the Lean Zulip for combinatorial maps, hypermaps, rotation systems, planarity, and
Kuratowski before starting layers 1 and 10. Check the status of Mathlib's multigraph
`Graph α β` before layer 5: layer 5 and layer 10 both depend on it, and if it is not yet
mature, the roadmap should derive its own graph type from maps and note the reversal.





zulip thread "planar hypergraphs"
https://leanprover.zulipchat.com/#narrow/channel/217875-Is-there-code-for-X.3F/topic/planar.20graphs/with/492352425

https://leanprover.zulipchat.com/#narrow/channel/116395-maths/topic/Combinatorial.20maps/with/462832296

https://leanprover.zulipchat.com/#narrow/channel/116395-maths/topic/Combinatorial.20maps/with/462832296


make the representative theorem names descriptive and put the eponyms in the comments