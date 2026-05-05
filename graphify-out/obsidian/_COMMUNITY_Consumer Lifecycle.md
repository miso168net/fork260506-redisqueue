---
type: community
cohesion: 0.29
members: 12
---

# Consumer Lifecycle

**Cohesion:** 0.29 - loosely connected
**Members:** 12 nodes

## Members
- [[.Run()]] - code - consumer.go
- [[.Shutdown()]] - code - consumer.go
- [[.enqueue()]] - code - consumer.go
- [[.poll()]] - code - consumer.go
- [[.process()]] - code - consumer.go
- [[.reclaim()]] - code - consumer.go
- [[.work()]] - code - consumer.go
- [[Consumer]] - code - consumer.go
- [[TestNewSignalHandler()]] - code - signals_test.go
- [[newSignalHandler()]] - code - signals.go
- [[signals.go]] - code - signals.go
- [[signals_test.go]] - code - signals_test.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Consumer_Lifecycle
SORT file.name ASC
```

## Connections to other communities
- 6 edges to [[_COMMUNITY_Consumer Registration]]
- 5 edges to [[_COMMUNITY_Producer Implementation]]
- 4 edges to [[_COMMUNITY_Redis Client Layer]]

## Top bridge nodes
- [[.Run()]] - degree 17, connects to 3 communities
- [[Consumer]] - degree 10, connects to 1 community
- [[.reclaim()]] - degree 4, connects to 1 community
- [[.Shutdown()]] - degree 3, connects to 1 community