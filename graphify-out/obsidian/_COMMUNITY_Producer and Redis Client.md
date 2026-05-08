---
type: community
cohesion: 0.22
members: 14
---

# Producer and Redis Client

**Cohesion:** 0.22 - loosely connected
**Members:** 14 nodes

## Members
- [[.Enqueue()]] - code - producer.go
- [[NewConsumerWithOptions()]] - code - consumer.go
- [[NewProducer()]] - code - producer.go
- [[NewProducerWithOptions()]] - code - producer.go
- [[Producer]] - code - producer.go
- [[ProducerOptions]] - code - producer.go
- [[TestNewConsumerWithOptions()]] - code - consumer_test.go
- [[TestNewRedisClient()]] - code - redis_test.go
- [[TestRedisPreflightChecks()]] - code - redis_test.go
- [[TestRun()]] - code - consumer_test.go
- [[newRedisClient()]] - code - redis.go
- [[producer.go]] - code - producer.go
- [[redisPreflightChecks()]] - code - redis.go
- [[redis_test.go]] - code - redis_test.go

## Live Query (requires Dataview plugin)

```dataview
TABLE source_file, type FROM #community/Producer_and_Redis_Client
SORT file.name ASC
```

## Connections to other communities
- 11 edges to [[_COMMUNITY_Consumer Internals and Tests]]
- 6 edges to [[_COMMUNITY_Consumer Registration]]

## Top bridge nodes
- [[TestRun()]] - degree 8, connects to 2 communities
- [[TestNewConsumerWithOptions()]] - degree 4, connects to 2 communities
- [[NewProducerWithOptions()]] - degree 7, connects to 1 community
- [[newRedisClient()]] - degree 7, connects to 1 community
- [[NewConsumerWithOptions()]] - degree 6, connects to 1 community