---
type: community
cohesion: 0.24
members: 11
---

# Consumer Registration

**Cohesion:** 0.24 - loosely connected
**Members:** 11 nodes

## Members
- [[.Register()]] - code - consumer.go
- [[.RegisterWithLastID()]] - code - consumer.go
- [[ConsumerFunc]] - code - consumer.go
- [[ConsumerOptions]] - code - consumer.go
- [[NewConsumer()]] - code - consumer.go
- [[TestNewConsumer()]] - code - consumer_test.go
- [[TestRegister()]] - code - consumer_test.go
- [[TestRegisterWithLastID()]] - code - consumer_test.go
- [[consumer.go]] - code - consumer.go
- [[consumer_test.go]] - code - consumer_test.go
- [[registeredConsumer]] - code - consumer.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Consumer_Registration
SORT file.name ASC
```

## Connections to other communities
- 6 edges to [[_COMMUNITY_Consumer Internals and Tests]]
- 6 edges to [[_COMMUNITY_Producer and Redis Client]]

## Top bridge nodes
- [[consumer.go]] - degree 6, connects to 2 communities
- [[.Register()]] - degree 4, connects to 2 communities
- [[NewConsumer()]] - degree 6, connects to 1 community
- [[consumer_test.go]] - degree 5, connects to 1 community
- [[TestRegister()]] - degree 4, connects to 1 community