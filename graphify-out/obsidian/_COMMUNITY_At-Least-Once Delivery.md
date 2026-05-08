---
type: community
cohesion: 0.50
members: 5
---

# At-Least-Once Delivery

**Cohesion:** 0.50 - moderately connected
**Members:** 5 nodes

## Members
- [[FR-Delivery- clause group (at-least-once, claimack, visibility timeout)]] - document - docs/superpowers/specs/2026-05-07-baseline-design.md
- [[US2 Consumer at-least-once processing]] - document - docs/superpowers/specs/2026-05-07-baseline-design.md
- [[at-least-once delivery feature (claimack)]] - rationale - README.md
- [[redisqueue v1.0.1 reclaim ID increment fix]] - document - CHANGELOG.md
- [[visibility timeout feature]] - rationale - README.md

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/At-Least-Once_Delivery
SORT file.name ASC
```

## Connections to other communities
- 2 edges to [[_COMMUNITY_Baseline Spec Design]]

## Top bridge nodes
- [[FR-Delivery- clause group (at-least-once, claimack, visibility timeout)]] - degree 5, connects to 1 community
- [[US2 Consumer at-least-once processing]] - degree 3, connects to 1 community