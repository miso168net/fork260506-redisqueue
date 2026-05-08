---
source_file: "x_fork.tree-20260506.md"
type: "document"
community: "Fork Meta and Graphify Session"
location: "L25"
tags:
  - graphify/document
  - graphify/INFERRED
  - community/Fork_Meta_and_Graphify_Session
---

# producer.go role (Producer + Enqueue, calls newRedisClient + redisPreflightChecks)

## Connections
- [[README Producer example (NewProducerWithOptions  Enqueue)]] - `references` [INFERRED]
- [[god node NewProducerWithOptions() (7 edges)]] - `references` [INFERRED]
- [[message.go role (Message struct, only public value object)]] - `shares_data_with` [EXTRACTED]
- [[redis.go role (newRedisClient  redisPreflightChecks  incrementMessageID)]] - `calls` [EXTRACTED]

#graphify/document #graphify/INFERRED #community/Fork_Meta_and_Graphify_Session