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
