# Graph Report - .  (2026-05-08)

## Corpus Check
- Corpus is ~8,258 words - fits in a single context window. You may not need a graph.

## Summary
- 105 nodes · 149 edges · 15 communities (9 shown, 6 thin omitted)
- Extraction: 66% EXTRACTED · 34% INFERRED · 0% AMBIGUOUS · INFERRED: 51 edges (avg confidence: 0.82)
- Token cost: 48,000 input · 2,700 output

## Community Hubs (Navigation)
- [[_COMMUNITY_Baseline Spec Design|Baseline Spec Design]]
- [[_COMMUNITY_Fork Meta and Graphify Session|Fork Meta and Graphify Session]]
- [[_COMMUNITY_Consumer Internals and Tests|Consumer Internals and Tests]]
- [[_COMMUNITY_Producer and Redis Client|Producer and Redis Client]]
- [[_COMMUNITY_Consumer Registration|Consumer Registration]]
- [[_COMMUNITY_Fork Chain and Module Identity|Fork Chain and Module Identity]]
- [[_COMMUNITY_At-Least-Once Delivery|At-Least-Once Delivery]]
- [[_COMMUNITY_Message Type|Message Type]]
- [[_COMMUNITY_Library Origin (v1.0.0)|Library Origin (v1.0.0)]]
- [[_COMMUNITY_Versioning Strategy|Versioning Strategy]]
- [[_COMMUNITY_Release v2.1.0|Release v2.1.0]]
- [[_COMMUNITY_Build Tools Idiom|Build Tools Idiom]]
- [[_COMMUNITY_Coverage Gate|Coverage Gate]]

## God Nodes (most connected - your core abstractions)
1. `Baseline Spec Design Document (2026-05-07)` - 20 edges
2. `graphify session 2026-05-06 (/graphify . --obsidian)` - 11 edges
3. `Consumer` - 10 edges
4. `TestRun()` - 8 edges
5. `NewProducerWithOptions()` - 7 edges
6. `newRedisClient()` - 7 edges
7. `NewConsumer()` - 6 edges
8. `NewConsumerWithOptions()` - 6 edges
9. `robinjoseph08/redisqueue (upstream original)` - 5 edges
10. `consumer.go role (Consumer + Register + Run + reclaim/scheduler/panic recovery)` - 5 edges

## Surprising Connections (you probably didn't know these)
- `redisqueue v1.0.1 reclaim ID increment fix` --semantically_similar_to--> `FR-Delivery-* clause group (at-least-once, claim/ack, visibility timeout)`  [INFERRED] [semantically similar]
  CHANGELOG.md → docs/superpowers/specs/2026-05-07-baseline-design.md
- `at-least-once delivery feature (claim/ack)` --semantically_similar_to--> `US2: Consumer at-least-once processing`  [INFERRED] [semantically similar]
  README.md → docs/superpowers/specs/2026-05-07-baseline-design.md
- `NewConsumer()` --calls--> `TestRun()`  [INFERRED]
  consumer.go → consumer_test.go
- `NewProducer()` --calls--> `TestNewProducer()`  [INFERRED]
  producer.go → producer_test.go
- `NewProducerWithOptions()` --calls--> `TestNewProducerWithOptions()`  [INFERRED]
  producer.go → producer_test.go

## Hyperedges (group relationships)
- **Fork chain: upstream -> intermediate -> personal fork** — fork_robinjoseph08_redisqueue_upstream, fork_go_admin_team_redisqueue_intermediate, fork_miso168net_redisqueue [EXTRACTED 1.00]
- **Consumer resilience triad (signals + panic recovery + at-least-once)** — readme_features_signal_handling, readme_features_panic_recovery, readme_features_at_least_once, baseline_use_case_us3_resilience [EXTRACTED 0.95]
- **Layer tag workflow ([Contract]/[Snapshot] + Constitution Principle II + traceability matrix)** — baseline_layer_tag_design, constitution_principle_ii_api_stability, baseline_sc_004_traceability_matrix, baseline_gap_report_appendix [EXTRACTED 0.95]

## Communities (15 total, 6 thin omitted)

### Community 0 - "Baseline Spec Design"
Cohesion: 0.13
Nodes (20): anchor commit db9f9f9 (constitution v1.0.0 ratified), coverage depth C (public API + behavior + internal snapshot), Baseline Spec Design Document (2026-05-07), FR-Consumer-* clause group, FR-Errors-* clause group (Errors channel, validation, preflight), FR-Internal-* clause group (newRedisClient, redisPreflightChecks, etc.), FR-Lifecycle-* clause group (signals, shutdown, panic recovery), FR-Producer-* clause group (+12 more)

### Community 1 - "Fork Meta and Graphify Session"
Cohesion: 0.12
Nodes (19): main+master dual long-lived branches strategy, main branch created 2026-05-06 from master, graphify rules for project (read GRAPH_REPORT before architecture, run graphify update after edits), AMBIGUOUS edges upgraded to EXTRACTED via graph.json patch, god node Consumer (14 edges), god node NewProducerWithOptions() (7 edges), god node newRedisClient() (7 edges, cross-community bridge), god node TestRun() (9 edges) (+11 more)

### Community 2 - "Consumer Internals and Tests"
Cohesion: 0.18
Nodes (7): Consumer, TestEnqueue(), TestNewProducer(), TestNewProducerWithOptions(), incrementMessageID(), newSignalHandler(), TestNewSignalHandler()

### Community 3 - "Producer and Redis Client"
Cohesion: 0.22
Nodes (11): NewConsumerWithOptions(), TestNewConsumerWithOptions(), TestRun(), NewProducer(), NewProducerWithOptions(), Producer, ProducerOptions, newRedisClient() (+3 more)

### Community 4 - "Consumer Registration"
Cohesion: 0.24
Nodes (7): ConsumerFunc, ConsumerOptions, NewConsumer(), registeredConsumer, TestNewConsumer(), TestRegister(), TestRegisterWithLastID()

### Community 5 - "Fork Chain and Module Identity"
Cohesion: 0.43
Nodes (7): redisqueue v2.0.0 release (go-redis/v7, /v2 module path), go-admin-team/redisqueue (intermediate fork), miso168net/fork260506-redisqueue (this repo), robinjoseph08/redisqueue (upstream original), fork chain robinjoseph08 -> go-admin-team -> miso168net, MIT License (Robin Joseph 2019), module path github.com/robinjoseph08/redisqueue/v2

### Community 6 - "At-Least-Once Delivery"
Cohesion: 0.5
Nodes (5): FR-Delivery-* clause group (at-least-once, claim/ack, visibility timeout), US2: Consumer at-least-once processing, redisqueue v1.0.1 reclaim ID increment fix, at-least-once delivery feature (claim/ack), visibility timeout feature

## Knowledge Gaps
- **34 isolated node(s):** `ConsumerFunc`, `registeredConsumer`, `ConsumerOptions`, `Message`, `ProducerOptions` (+29 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **6 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Baseline Spec Design Document (2026-05-07)` connect `Baseline Spec Design` to `Fork Meta and Graphify Session`, `At-Least-Once Delivery`?**
  _High betweenness centrality (0.164) - this node is a cross-community bridge._
- **Why does `graphify session 2026-05-06 (/graphify . --obsidian)` connect `Fork Meta and Graphify Session` to `Baseline Spec Design`, `Fork Chain and Module Identity`?**
  _High betweenness centrality (0.161) - this node is a cross-community bridge._
- **Why does `fork chain robinjoseph08 -> go-admin-team -> miso168net` connect `Fork Chain and Module Identity` to `Fork Meta and Graphify Session`?**
  _High betweenness centrality (0.049) - this node is a cross-community bridge._
- **Are the 7 inferred relationships involving `TestRun()` (e.g. with `NewConsumer()` and `NewConsumerWithOptions()`) actually correct?**
  _`TestRun()` has 7 INFERRED edges - model-reasoned connections that need verification._
- **Are the 5 inferred relationships involving `NewProducerWithOptions()` (e.g. with `TestRun()` and `newRedisClient()`) actually correct?**
  _`NewProducerWithOptions()` has 5 INFERRED edges - model-reasoned connections that need verification._
- **What connects `ConsumerFunc`, `registeredConsumer`, `ConsumerOptions` to the rest of the system?**
  _34 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Baseline Spec Design` be split into smaller, more focused modules?**
  _Cohesion score 0.13 - nodes in this community are weakly interconnected._