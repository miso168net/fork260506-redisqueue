---
type: community
cohesion: 0.18
members: 18
---

# Consumer Internals and Tests

**Cohesion:** 0.18 - loosely connected
**Members:** 18 nodes

## Members
- [[.Run()]] - code - consumer.go
- [[.Shutdown()]] - code - consumer.go
- [[.enqueue()]] - code - consumer.go
- [[.poll()]] - code - consumer.go
- [[.process()]] - code - consumer.go
- [[.reclaim()]] - code - consumer.go
- [[.work()]] - code - consumer.go
- [[Consumer]] - code - consumer.go
- [[TestEnqueue()]] - code - producer_test.go
- [[TestNewProducer()]] - code - producer_test.go
- [[TestNewProducerWithOptions()]] - code - producer_test.go
- [[TestNewSignalHandler()]] - code - signals_test.go
- [[incrementMessageID()]] - code - redis.go
- [[newSignalHandler()]] - code - signals.go
- [[producer_test.go]] - code - producer_test.go
- [[redis.go]] - code - redis.go
- [[signals.go]] - code - signals.go
- [[signals_test.go]] - code - signals_test.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Consumer_Internals_and_Tests
SORT file.name ASC
```

## Connections to other communities
- 11 edges to [[_COMMUNITY_Producer and Redis Client]]
- 6 edges to [[_COMMUNITY_Consumer Registration]]

## Top bridge nodes
- [[.Run()]] - degree 17, connects to 2 communities
- [[Consumer]] - degree 10, connects to 1 community
- [[TestNewProducerWithOptions()]] - degree 4, connects to 1 community
- [[.Shutdown()]] - degree 3, connects to 1 community
- [[TestEnqueue()]] - degree 3, connects to 1 community