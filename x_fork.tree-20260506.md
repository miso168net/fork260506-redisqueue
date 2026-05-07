# 專案檔案結構與用途註解(2026-05-06 snapshot)

本檔案對應 `tree` 命令在 `fork260506-redisqueue/` 根目錄的輸出,
為每個檔案/目錄補上一行用途註解,作為閱讀程式碼前的索引。

- 命名沿用 `x_fork.graphify-20260506.md` 的「snapshot 日期」慣例。
- `graphify-out/` 為 graphify 工具自動生成的知識圖譜輸出(共 ~110 檔),
  此處僅以目錄層級一句話帶過,不深入逐檔列出
  (詳細結構與內容見 `x_fork.graphify-20260506.md` 與
  `graphify-out/GRAPH_REPORT.md`)。
- 其餘 Go 原始碼、設定、文件每一檔都附註解。

```
.
├── CHANGELOG.md                  # 人類可讀的版本變更紀錄(v1.0.0 2019-07-13 ~ v2.1.0 2020-10-15,git-chglog 產生)
├── CLAUDE.md                     # 給 Claude Code 看的專案指令(graphify 規則:回答架構題前讀 GRAPH_REPORT,改碼後跑 graphify update)
├── LICENSE.txt                   # MIT License(Robin Joseph, Copyright 2019,fork 沿用)
├── Makefile                      # 一站式入口:install / test / lint / enforce(coverage gate)/ html / coveralls / release
├── README.md                     # Library 對外說明:特色清單 + producer / consumer 完整範例(godoc 同步 doc.go 內容)
│
├── consumer.go                   # Consumer 主體(NewConsumer / NewConsumerWithOptions / Register / RegisterWithLastID / Run;reclaim 迴圈 + scheduler + panic recovery + errors channel 都在這)
├── consumer_test.go              # consumer.go 單元 + 整合測試(TestRun / TestNewConsumer / TestNewConsumerWithOptions / TestRegister / TestRegisterWithLastID 等)
├── doc.go                        # 套件層 GoDoc;貼 producer + consumer 完整範例,GoDoc 頁面的入口靠它
├── message.go                    # Message 結構(Stream / ID / Values map)— library 唯一公開值物件,producer / consumer 雙邊都用
├── producer.go                   # Producer 主體(NewProducer / NewProducerWithOptions / Enqueue;構造階段呼叫 newRedisClient + redisPreflightChecks)
├── producer_test.go              # producer.go 對應測試(TestEnqueue / TestNewProducer / TestNewProducerWithOptions)
├── redis.go                      # Redis client 層(newRedisClient 支援 redis.UniversalClient / redisPreflightChecks / incrementMessageID — reclaim 迴圈用)
├── redis_test.go                 # redis.go 對應測試(TestNewRedisClient / TestRedisPreflightChecks)
├── signals.go                    # newSignalHandler;攔截 SIGINT / SIGTERM,觸發 Consumer graceful shutdown
├── signals_test.go               # signals.go 對應測試(TestNewSignalHandler)
├── tools.go                      # `// +build tools` 工具相依宣告(import _ git-chglog / goveralls;讓 go.mod 鎖版本但不會被一般 build 編入)
│
├── go.mod                        # Go module 宣告(module path = github.com/robinjoseph08/redisqueue/v2;/v2 後綴 = 語意匯入版本)
│
├── docs/                         # 本 fork 為 SpecKit / superpowers 工作流新增的文件目錄(原上游無此目錄)
│   └── superpowers/
│       └── specs/
│           └── 2026-05-07-baseline-design.md   # baseline spec 的 design 草案(superpowers:brainstorming 產出;目前 untracked,1deb2ad commit 已退掉)
│
├── graphify-out/                 # graphify 知識圖譜輸出(整個目錄由工具生成,可由原始檔重建;不深入逐檔列出,詳見 x_fork.graphify-20260506.md)
│
├── scripts/
│   └── coverage.sh               # 強制 coverage ≥ 80% 的閘門;低於門檻則 exit 1(由 Makefile `enforce` target 呼叫)
│
├── x_fork.branch-origin.md       # fork 鏈紀錄:upstream(robinjoseph08)→ intermediate(go-admin-team)→ 本 fork(miso168net);說明 main / master 並存策略
├── x_fork.graphify-20260506.md   # graphify session 紀錄(2026-05-06 執行 `/graphify . --obsidian` 的 pipeline、community label、token 成本、發現)
└── x_fork.tree-20260506.md       # 本檔;repo 結構快照與註解
```

---

## 補充說明

**幾個重要慣例**:

- `*_test.go` 一律是 Go 標準測試檔;本 library 採「同套件白箱測試 + 真 Redis
  整合」混合風格,每支 test 在啟動前先 `newRedisClient` 連到 redis 服務。
- 沒有 `options.go` — 本 library 採「single struct + constructor variants」
  慣例(`Producer` / `Consumer` 各有 `NewXxx` 與 `NewXxxWithOptions`
  兩入口,options 結構直接定義在 `producer.go` / `consumer.go` 內)。
- 沒有 `deprecated.go` — library 規模小,過去版本變更走 module path 升級
  (v1 → v2,新建獨立路徑 `redisqueue/v2`),不靠 in-package alias。
- `tools.go` 的 `// +build tools` 是 Go 1.13+ 的 idiom;單純讓 `go.mod`
  追蹤 git-chglog / goveralls 版本而不影響正式 build。
- `x_fork.*` 開頭的檔案都是這份 fork 的 meta 紀錄,不影響任何程式邏輯,
  可安全忽略或刪除(可參考 `x_fork.branch-origin.md` 的尾註)。
- `main` 與 `master` 兩條長命分支並存:`master` 凍結保留上游歷史相容,
  `main` 是本 fork 的工作預設(規則見 `x_fork.branch-origin.md`)。

**最常被引用的 god nodes**(來自 graphify `GRAPH_REPORT.md`):

| 節點                          | 所在檔案                       | 邊數 |
|-------------------------------|-------------------------------|----:|
| `Consumer`                    | `consumer.go`                 | 14 |
| `Consumer`(另一脈絡)          | `consumer.go`                 | 10 |
| `TestRun()`                   | `consumer_test.go`            |  9 |
| `NewProducerWithOptions()`    | `producer.go`                 |  7 |
| `newRedisClient()`            | `redis.go`                    |  7 |
| `NewConsumer()`               | `consumer.go`                 |  6 |
| `NewConsumerWithOptions()`    | `consumer.go`                 |  6 |
| `Producer`                    | `producer.go`                 |  6 |
| `redisqueue (project)`        | (專案實體節點)                  |  5 |
| `TestNewConsumerWithOptions()`| `consumer_test.go`            |  4 |

讀程式碼建議從這幾個入口往外爬,效率最高;特別是 `Consumer` 與
`newRedisClient()` 是兩個 cross-community bridge,理解它們可以同時抓到
「Consumer Configuration」「Consumer Lifecycle」「Redis Client Layer」三個
社群的重點。

---

*本檔案於 2026-05-08 由 Claude Code 依 `tree` 在 2026-05-06 的輸出生成,
日期 `20260506` 沿用 fork 的 snapshot 命名慣例
(對齊 `x_fork.graphify-20260506.md`)。*
