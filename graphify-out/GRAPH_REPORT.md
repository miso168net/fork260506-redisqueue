# Graph Report - .  (2026-05-06)

## Corpus Check
- 15 files · ~5,520 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 88 nodes · 131 edges · 14 communities detected
- Extraction: 62% EXTRACTED · 38% INFERRED · 0% AMBIGUOUS · INFERRED: 50 edges (avg confidence: 0.81)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_License and Initial Release|License and Initial Release]]
- [[_COMMUNITY_Consumer Configuration|Consumer Configuration]]
- [[_COMMUNITY_Consumer Lifecycle|Consumer Lifecycle]]
- [[_COMMUNITY_Consumer Registration|Consumer Registration]]
- [[_COMMUNITY_Producer Implementation|Producer Implementation]]
- [[_COMMUNITY_Redis Client Layer|Redis Client Layer]]
- [[_COMMUNITY_Fork Branch Strategy|Fork Branch Strategy]]
- [[_COMMUNITY_v2 Module Upgrade|v2 Module Upgrade]]
- [[_COMMUNITY_Message Type|Message Type]]
- [[_COMMUNITY_Universal Client Support|Universal Client Support]]
- [[_COMMUNITY_Reclaim ID Fix|Reclaim ID Fix]]
- [[_COMMUNITY_Package Doc|Package Doc]]
- [[_COMMUNITY_Build Tools|Build Tools]]
- [[_COMMUNITY_Release v1.1.0|Release v1.1.0]]

## God Nodes (most connected - your core abstractions)
1. `Consumer` - 14 edges
2. `Consumer` - 10 edges
3. `TestRun()` - 9 edges
4. `NewProducerWithOptions()` - 7 edges
5. `newRedisClient()` - 7 edges
6. `NewConsumer()` - 6 edges
7. `NewConsumerWithOptions()` - 6 edges
8. `Producer` - 6 edges
9. `redisqueue (project)` - 5 edges
10. `TestNewConsumerWithOptions()` - 4 edges

## Surprising Connections (you probably didn't know these)
- `TestRun()` --calls--> `NewConsumer()`  [INFERRED]
  consumer_test.go → consumer.go
- `TestRun()` --calls--> `NewConsumerWithOptions()`  [INFERRED]
  consumer_test.go → consumer.go
- `NewProducerWithOptions()` --calls--> `newRedisClient()`  [INFERRED]
  producer.go → redis.go
- `NewProducerWithOptions()` --calls--> `redisPreflightChecks()`  [INFERRED]
  producer.go → redis.go
- `TestNewProducerWithOptions()` --calls--> `newRedisClient()`  [INFERRED]
  producer_test.go → redis.go

## Hyperedges (group relationships)
- **At-least-once delivery pipeline** — readme_consumer, readme_at_least_once, readme_visibility_timeout, readme_signal_handling [INFERRED 0.85]
- **Consumer tuning knobs** — readme_consumer_options, readme_visibility_timeout, readme_concurrency, readme_batch_size [EXTRACTED 1.00]
- **Fork lineage chain** — x_fork_branch_origin_repo, x_fork_branch_origin_intermediate, x_fork_branch_origin_upstream, x_fork_branch_origin_main_branch [INFERRED 0.75]

## Communities

### Community 0 - "License and Initial Release"
Cohesion: 0.16
Nodes (15): First version of producer and consumer, v1.0.0 (2019-07-13), MIT License, Robin Joseph (Copyright 2019), Producer.Enqueue, Message struct, NewProducerWithOptions, Producer (+7 more)

### Community 1 - "Consumer Configuration"
Cohesion: 0.22
Nodes (13): At-least-once delivery (claim/ack), Batch size (BufferSize), Concurrency setting, Consumer, ConsumerOptions, Errors channel, Multiple streams support, NewConsumerWithOptions (+5 more)

### Community 2 - "Consumer Lifecycle"
Cohesion: 0.29
Nodes (3): Consumer, newSignalHandler(), TestNewSignalHandler()

### Community 3 - "Consumer Registration"
Cohesion: 0.24
Nodes (7): ConsumerFunc, ConsumerOptions, NewConsumer(), registeredConsumer, TestNewConsumer(), TestRegister(), TestRegisterWithLastID()

### Community 4 - "Producer Implementation"
Cohesion: 0.29
Nodes (8): TestRun(), NewProducer(), NewProducerWithOptions(), Producer, ProducerOptions, TestEnqueue(), TestNewProducer(), TestNewProducerWithOptions()

### Community 5 - "Redis Client Layer"
Cohesion: 0.33
Nodes (7): NewConsumerWithOptions(), TestNewConsumerWithOptions(), incrementMessageID(), newRedisClient(), redisPreflightChecks(), TestNewRedisClient(), TestRedisPreflightChecks()

### Community 6 - "Fork Branch Strategy"
Cohesion: 0.4
Nodes (5): HEAD 508101c (Go 1.20 upgrade), main branch (fork), master branch, Rationale: preserve master without rewriting history, Rationale: unify default branch name to main

### Community 7 - "v2 Module Upgrade"
Cohesion: 0.67
Nodes (3): go-redis/v7 + redisqueue/v2 upgrade, v2.0.0 (2020-05-26), github.com/robinjoseph08/redisqueue/v2 module

### Community 8 - "Message Type"
Cohesion: 1.0
Nodes (1): Message

### Community 9 - "Universal Client Support"
Cohesion: 1.0
Nodes (2): redis.UniversalClient support, v2.1.0 (2020-10-15)

### Community 10 - "Reclaim ID Fix"
Cohesion: 1.0
Nodes (2): reclaim: increment ID when looping, v1.0.1 (2019-08-03)

### Community 11 - "Package Doc"
Cohesion: 1.0
Nodes (0): 

### Community 12 - "Build Tools"
Cohesion: 1.0
Nodes (0): 

### Community 13 - "Release v1.1.0"
Cohesion: 1.0
Nodes (1): v1.1.0 (2020-05-26)

## Knowledge Gaps
- **23 isolated node(s):** `ConsumerFunc`, `registeredConsumer`, `ConsumerOptions`, `Message`, `ProducerOptions` (+18 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **Thin community `Message Type`** (2 nodes): `message.go`, `Message`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Universal Client Support`** (2 nodes): `redis.UniversalClient support`, `v2.1.0 (2020-10-15)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Reclaim ID Fix`** (2 nodes): `reclaim: increment ID when looping`, `v1.0.1 (2019-08-03)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Package Doc`** (1 nodes): `doc.go`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Build Tools`** (1 nodes): `tools.go`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Release v1.1.0`** (1 nodes): `v1.1.0 (2020-05-26)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Consumer` connect `Consumer Configuration` to `License and Initial Release`?**
  _High betweenness centrality (0.066) - this node is a cross-community bridge._
- **Why does `redisqueue (project)` connect `License and Initial Release` to `Consumer Configuration`?**
  _High betweenness centrality (0.042) - this node is a cross-community bridge._
- **Why does `Consumer` connect `Consumer Lifecycle` to `Consumer Registration`?**
  _High betweenness centrality (0.038) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `Consumer` (e.g. with `First version of producer and consumer` and `NewConsumerWithOptions`) actually correct?**
  _`Consumer` has 2 INFERRED edges - model-reasoned connections that need verification._
- **Are the 8 inferred relationships involving `TestRun()` (e.g. with `NewConsumer()` and `NewConsumerWithOptions()`) actually correct?**
  _`TestRun()` has 8 INFERRED edges - model-reasoned connections that need verification._
- **Are the 5 inferred relationships involving `NewProducerWithOptions()` (e.g. with `TestRun()` and `newRedisClient()`) actually correct?**
  _`NewProducerWithOptions()` has 5 INFERRED edges - model-reasoned connections that need verification._
- **Are the 6 inferred relationships involving `newRedisClient()` (e.g. with `NewConsumerWithOptions()` and `TestNewConsumerWithOptions()`) actually correct?**
  _`newRedisClient()` has 6 INFERRED edges - model-reasoned connections that need verification._