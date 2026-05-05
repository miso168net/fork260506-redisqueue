---
type: community
cohesion: 0.29
members: 11
---

# Producer Implementation

**Cohesion:** 0.29 - loosely connected
**Members:** 11 nodes

## Members
- [[.Enqueue()]] - code - producer.go
- [[NewProducer()]] - code - producer.go
- [[NewProducerWithOptions()]] - code - producer.go
- [[Producer]] - code - producer.go
- [[ProducerOptions]] - code - producer.go
- [[TestEnqueue()]] - code - producer_test.go
- [[TestNewProducer()]] - code - producer_test.go
- [[TestNewProducerWithOptions()]] - code - producer_test.go
- [[TestRun()]] - code - consumer_test.go
- [[producer.go]] - code - producer.go
- [[producer_test.go]] - code - producer_test.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Producer_Implementation
SORT file.name ASC
```

## Connections to other communities
- 5 edges to [[_COMMUNITY_Consumer Lifecycle]]
- 4 edges to [[_COMMUNITY_Redis Client Layer]]
- 3 edges to [[_COMMUNITY_Consumer Registration]]

## Top bridge nodes
- [[TestRun()]] - degree 9, connects to 3 communities
- [[TestNewProducerWithOptions()]] - degree 4, connects to 2 communities
- [[NewProducerWithOptions()]] - degree 7, connects to 1 community
- [[TestEnqueue()]] - degree 4, connects to 1 community
- [[TestNewProducer()]] - degree 3, connects to 1 community