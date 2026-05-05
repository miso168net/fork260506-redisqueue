---
type: community
cohesion: 0.33
members: 9
---

# Redis Client Layer

**Cohesion:** 0.33 - loosely connected
**Members:** 9 nodes

## Members
- [[NewConsumerWithOptions()]] - code - consumer.go
- [[TestNewConsumerWithOptions()]] - code - consumer_test.go
- [[TestNewRedisClient()]] - code - redis_test.go
- [[TestRedisPreflightChecks()]] - code - redis_test.go
- [[incrementMessageID()]] - code - redis.go
- [[newRedisClient()]] - code - redis.go
- [[redis.go]] - code - redis.go
- [[redisPreflightChecks()]] - code - redis.go
- [[redis_test.go]] - code - redis_test.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Redis_Client_Layer
SORT file.name ASC
```

## Connections to other communities
- 4 edges to [[_COMMUNITY_Producer Implementation]]
- 4 edges to [[_COMMUNITY_Consumer Lifecycle]]
- 3 edges to [[_COMMUNITY_Consumer Registration]]

## Top bridge nodes
- [[NewConsumerWithOptions()]] - degree 6, connects to 2 communities
- [[TestNewConsumerWithOptions()]] - degree 4, connects to 2 communities
- [[newRedisClient()]] - degree 7, connects to 1 community
- [[redisPreflightChecks()]] - degree 4, connects to 1 community
- [[TestRedisPreflightChecks()]] - degree 4, connects to 1 community