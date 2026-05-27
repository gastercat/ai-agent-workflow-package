# 主文件：AI Agent Workflow 導入說明

## 1. 文件目的

作為整份資料包的核心文件，定義 AI Agent Workflow 的架構、操作原則、治理方式與課堂導入流程。

本文件給教授、助教、同學與專題小組共同閱讀。它不是工具廣告，也不是單純比較哪個 Agent 比較好，而是說明如何把 AI Agent 放進可審查、可測試、可追溯的工作流程。

後續文件會從本文件延伸：

- `docs/03-同學版使用指南.md`：把流程轉成同學可操作步驟。
- `docs/04-建置指南.md`：補充工具與環境建置。
- `docs/05-XML-Prompt模板.md`：提供 XML prompt 範本。
- `docs/06-風險控管與評分建議.md`：提供評分與治理方式。

目標讀者：

- 教授
- 助教
- 同學
- 專題小組

## 2. AI Agent Workflow 是什麼

AI Agent Workflow 是把 AI Agent 放入既有工程流程的一種方法。Agent 可以協助閱讀檔案、整理問題、提出計畫、產生局部修改、補測試或整理文件，但不能取代人類對需求、架構、驗證與責任的判斷。

一個基本流程如下：

```text
Inspect -> Plan -> Approve -> Patch -> Test -> Review -> Commit
   |         |         |         |       |        |          |
   |         |         |         |       |        |          +-- 人類提交紀錄
   |         |         |         |       |        +------------- 人類閱讀 diff
   |         |         |         |       +---------------------- 測試或檢查
   |         |         |         +------------------------------ 限定範圍修改
   |         |         +---------------------------------------- 人類批准
   |         +-------------------------------------------------- Agent 提出計畫
   +------------------------------------------------------------ Agent 先觀察
```

這個流程的重點是：每一步都可以被記錄、檢查與退回，不讓 Agent 直接跨越人類審查。

## 3. 為什麼不是工具崇拜，而是 Workflow 升級

單純把 AI 工具加入課堂，並不會自動提升學習品質。若沒有規則，學生可能只會把題目貼給 AI；若只禁止，學生又缺少面對現代工具的訓練。

Workflow 升級的重點是把使用 AI 的行為變成可評分的工程過程：

| 不建議的做法 | 建議的做法 |
| --- | --- |
| 直接要求 AI 完成整份作業 | 先 inspect，再 plan，經人批准後才 patch。 |
| 只看最後答案 | 同時看 prompt、plan、diff、測試與反思紀錄。 |
| 相信 Agent 產出 | 用 test harness 與人工 review 驗證。 |
| 讓 Agent 直接改主分支 | 使用 branch，禁止 Agent 自動 push main。 |
| 把工具當成能力本身 | 把工具使用納入架構、測試與治理訓練。 |

## 4. Human-led Architecture

Human-led Architecture 是本資料包的核心主張：人類主導架構、邊界、驗證與責任；Agent 只在明確限制下協助執行。

| 面向 | 人類責任 | Agent 可協助 |
| --- | --- | --- |
| 需求 | 定義問題、成功條件、限制 | 整理需求、指出模糊處 |
| 架構 | 決定模組邊界、資料流、權限 | 提供選項、列出取捨 |
| 修改 | 批准修改範圍 | 產生局部 patch |
| 測試 | 決定驗證策略與 edge cases | 產生測試草稿、執行檢查 |
| 審查 | 閱讀 diff，拒絕不合理變更 | 摘要變更重點 |
| 提交 | 整理 commit 與說明 | 協助草擬訊息 |

這代表學生不能把「AI 說可以」當成理由。學生必須能說明為什麼這樣改、如何驗證、哪些規則不能被破壞。

## 5. Prompt Engineering 的角色

Prompt Engineering 在這套流程中不是「咒語」，而是任務規格化。好的 prompt 應該把目標、範圍、限制、輸出格式與驗證方式說清楚。

建議 prompt 至少包含：

- 任務目標：要解決什麼問題。
- 允許範圍：可以讀哪些檔案、可以改哪些檔案。
- 禁止事項：不能刪檔、不能改主分支、不能跳過測試。
- 輸出格式：要回報 plan、diff summary、測試結果。
- 驗證方式：要執行哪些測試或檢查。

## 6. XML 語意約束的價值

XML semantic constraints 的目的，是用明確標籤降低任務模糊度。它不保證 Agent 一定正確，但能讓任務邊界更容易被閱讀、審查與重複使用。

簡化概念如下：

```xml
<task>
  <goal>說明任務目標</goal>
  <allowed_files>列出可處理範圍</allowed_files>
  <forbidden_actions>列出禁止事項</forbidden_actions>
  <verification>列出測試或檢查方式</verification>
  <reporting>要求回報 diff summary</reporting>
</task>
```

完整模板放在 `docs/05-XML-Prompt模板.md` 與 `prompts/`，本文件只說明其角色。

## 7. Codex / Claude Code / VS Code Copilot Agent / Cline 的定位比較

以下工具應視為「不同情境下的例示」，不是唯一推薦。實際功能、介面與授權可能變動；安裝前請確認官方文件。

| 工具例示 | 較適合情境 | 教學定位 | 注意事項 |
| --- | --- | --- | --- |
| Codex | 熟悉 CLI、需要在 repo 中 inspect、patch、test 的使用者 | 示範終端機型 coding agent 如何進入 Git workflow | 應限制檔案範圍與命令權限；安裝前請確認官方文件。 |
| Claude Code | 熟悉 terminal、想把 Agent 放進腳本或命令列流程的使用者 | 示範 agentic coding tool 與 shell workflow 的結合 | 命令執行與權限邊界要明確；安裝前請確認官方文件。 |
| VS Code Copilot Agent | VS Code 使用者、初學者、課堂 demo | 示範 IDE 內 Agent 如何協助編輯、解釋與修改 | 容易快速修改多檔，仍需 Git diff review。 |
| Cline | VS Code / editor 使用者、本地模型或多 provider 實驗者 | 示範 editor 內開源 Agent 與工具調用流程 | 模型、成本、權限與工具調用都要由使用者管理。 |

官方文件入口：

- Codex：OpenAI Codex / Codex CLI 官方文件與說明。
- Claude Code：Anthropic Claude Code 官方文件。
- VS Code Copilot Agent：GitHub Copilot / VS Code 官方文件。
- Cline：Cline 官方文件。

課堂上可用「情境」而非「排名」比較工具：

| 情境 | 可觀察重點 |
| --- | --- |
| 初學者 | 是否容易看懂修改、是否能配合 Git diff review。 |
| CLI 使用者 | 是否適合 inspect、patch、test 的命令列流程。 |
| VS Code 使用者 | 是否能在 IDE 中清楚查看檔案與變更。 |
| 本地模型使用者 | 是否能設定 local model，並理解 RAM / VRAM 限制。 |
| 需要 MCP 的使用者 | 是否能安全連接外部工具與資料來源。 |
| 課堂 demo | 是否容易展示人類批准、測試與 review。 |
| 專題實作 | 是否能與 branch、PR、測試、文件紀錄整合。 |

## 8. OpenClaw vs Hermes Agent 的概念比較

OpenClaw 與 Hermes Agent 在本資料包中只作為進階 Agent 架構案例，不作為主要 coding agent，也不提供完整安裝教學。

| 架構案例 | 概念定位 | 可討論重點 | 不建議混淆之處 |
| --- | --- | --- | --- |
| OpenClaw | Agent 入口、通訊平台、MCP bridge、personal assistant gateway | 如何把不同通訊介面或個人入口接到 Agent 工作流 | 不應把它當成單純的 coding agent。 |
| Hermes Agent | persistent memory、self-improving agent、skills、長期使用者模型 | 長期記憶、技能生成、使用者模型與治理問題 | 不應把長期記憶視為一定正確或無風險。 |

這兩者適合放在「Agent 系統設計」討論中：當 Agent 不只是一次性的程式助理，而是長期入口或個人助理時，memory governance、permission boundary 與可撤回性會變得更重要。

## 9. 本地開源模型部署概念

本地模型部署指的是在自己的電腦或伺服器上執行模型，例如透過 Ollama 或其他 local inference 工具。這可以降低部分資料外傳疑慮，也方便課堂實驗，但不代表沒有成本或限制。

本地模型部署要先理解：

| 面向 | 說明 |
| --- | --- |
| 模型大小 | 模型越大通常需要更多 RAM 或 VRAM。 |
| 推論速度 | 受 CPU、GPU、記憶體頻寬與模型量化方式影響。 |
| context window | 能一次放入的文字有限，越大的 context 通常消耗更多記憶體。 |
| 工具整合 | 本地模型仍需透過 Agent 工具、MCP 或 API 才能操作檔案與工具。 |
| 品質差異 | 本地模型不必然適合所有 coding 任務，仍要測試與 review。 |

涉及安裝、模型選擇、硬體需求與參數設定時，請以官方文件為準；本文件不寫死版本號或細部指令。

## 10. Memory 問題：RAM、VRAM、Context Window、專案長期記憶

AI Agent Workflow 中的 memory 至少有四種不同含義，容易混淆。

| 類型 | 意思 | 風險 |
| --- | --- | --- |
| RAM | 系統記憶體，影響本地模型或工具能否執行 | 不足時可能變慢、失敗或無法載入模型。 |
| VRAM | GPU 記憶體，影響模型載入與推論效率 | 不足時可能退回 CPU 或限制模型與 context。 |
| Context window | 單次對話可放入的 token 範圍 | 太長會爆掉；太短會漏脈絡。 |
| 專案長期記憶 | Agent 或工具保存的專案偏好、規則、過往決策 | 可能過期、錯誤、與新需求衝突。 |

治理原則：

- 不把長期記憶當成事實來源。
- 重要規則寫入 repo，例如 `AGENTS.md`、`PROJECT_CONTEXT.md`、`INVARIANTS.md`。
- 任務開始前先讓 Agent inspect 現況，而不是完全依賴記憶。
- context 太大時，改用摘要、索引、分段任務與明確檔案範圍。

## 11. MCP、Skills、AGENTS.md 的角色

| 元件 | 角色 | 風險控管 |
| --- | --- | --- |
| MCP | 讓 Agent 以標準化方式連接外部工具、資料或服務 | 要控制 server 來源、權限、資料範圍與工具副作用。 |
| Skills | 將常用流程或專門能力封裝成可重用指令 | 要審查 skill 內容，避免把錯誤流程固定化。 |
| AGENTS.md | repo 內的 Agent 工作規則 | 要寫清楚允許範圍、禁止事項、測試與回報格式。 |

MCP 與 Skills 讓 Agent 更有能力，但也讓風險更大。工具可被呼叫，不代表應該被呼叫；可自動化，不代表可以跳過人類批准。

## 12. Invariants、Edge Cases、Permission Boundary

Agent Workflow 必須先定義不可破壞的規則。

| 概念 | 說明 | 範例 |
| --- | --- | --- |
| Invariants | 系統永遠要維持的規則 | API 回傳格式不可改、權限檢查不可移除。 |
| Edge cases | 容易被忽略的邊界情境 | 空輸入、權限不足、網路失敗、資料重複。 |
| Permission boundary | Agent 能讀、能改、能執行的界線 | 只能改指定檔案、不能刪除資料、不能 push main。 |

建議把 invariants 寫進 `examples/INVARIANTS.md` 類型的檔案，讓 Agent 與 reviewer 都有共同檢查基準。

## 13. Git / GitHub 在 Agent Workflow 中的角色

Git 與 GitHub 不是附加工具，而是 Agent Workflow 的安全基礎。Agent 會快速修改檔案，因此更需要版本紀錄與審查流程。

本文件只做概念鋪陳；詳細操作留到 `docs/03-同學版使用指南.md` 與 `docs/04-建置指南.md`。

| 概念 | 在 Agent Workflow 中的作用 |
| --- | --- |
| repository | 保存專案與變更歷史。 |
| commit | 在使用 Agent 前後建立可回復的節點。 |
| branch | 隔離實驗與修改，保護 main。 |
| remote | 與 GitHub 等平台同步。 |
| pull request | 提供團隊 review、討論與紀錄。 |
| git diff | 檢查 Agent 實際改了什麼。 |

基本原則：

- 使用 Agent 前先確認 `git status`，必要時先 commit。
- Agent 修改後一定要看 `git diff`。
- 不允許 Agent 自動 push main。
- 大改動應使用 branch 與 pull request。
- commit message 應由人整理，說明目的、變更與驗證方式。

## 14. 標準流程：Inspect → Plan → Approve → Patch → Test → Review → Commit

| 步驟 | 目的 | 產出 |
| --- | --- | --- |
| Inspect | 讀取現況，不修改 | 現有檔案、風險、疑問 |
| Plan | 設計修改方式 | 步驟、影響範圍、測試方法 |
| Approve | 人類批准 | 明確允許範圍 |
| Patch | 執行小範圍修改 | 實際檔案變更 |
| Test | 驗證結果 | 測試輸出或檢查紀錄 |
| Review | 人類審查 | Git diff review 與取捨 |
| Commit | 建立紀錄 | 可追溯 commit |

建議在課堂中要求學生提交：

- 使用的 prompt 或 XML 任務。
- Agent 的 plan。
- 主要 diff summary。
- 測試或檢查結果。
- 人類 review 後的反思。

## 15. Context Window 爆掉時的處理方式

Context window overflow 常見於專案太大、對話太長、貼入太多檔案或 Agent 記憶混入太多舊資訊時。

處理方式：

1. 停止繼續堆疊對話。
2. 要求 Agent 產生目前任務摘要。
3. 明確列出下一步只需要的檔案與限制。
4. 分段處理，例如先 inspect 單一模組，再 patch 單一檔案。
5. 把穩定規則寫入 repo 文件，而不是依賴對話記憶。
6. 對重要結論重新驗證，不把舊 context 當成事實。

簡化做法：

```text
大任務 -> 任務摘要 -> 切小任務 -> 限定檔案 -> 測試 -> review -> 下一段
```

## 16. 產業趨勢：Context Engineering、Tool Use、Test Harness、Agentic Workflow

本資料包採取保守角度看待趨勢：這些趨勢值得學習，但不代表可以放棄工程紀律。

| 趨勢 | 對課堂的意義 |
| --- | --- |
| Context Engineering | 學生需要學會如何提供足夠但不過量的脈絡。 |
| Tool Use | Agent 能操作工具後，更需要 permission boundary。 |
| Test Harness | 自動化協助必須搭配可重現驗證。 |
| Agentic Workflow | 重點是人機分工與流程設計，不是讓 Agent 自行決定一切。 |

## 17. 建議導入路線

建議逐步導入，不要一開始就允許 Agent 大範圍修改。

| 階段 | 教學重點 | 建議限制 |
| --- | --- | --- |
| 第 1 階段 | inspect-only、plan-only | 不允許修改檔案。 |
| 第 2 階段 | 小範圍 patch 與 diff review | 只允許修改指定檔案。 |
| 第 3 階段 | test harness 與 edge cases | 修改必須附驗證紀錄。 |
| 第 4 階段 | branch、PR、團隊 review | 禁止直接 push main。 |
| 第 5 階段 | MCP、Skills、memory governance | 討論權限、長期記憶與工具風險。 |

## 18. 小結

AI Agent Workflow 的重點不是讓學生把工作交給 AI，而是讓學生學會如何在現代工具環境中維持工程責任。Agent 可以協助閱讀、規劃、修改與測試，但人類仍必須負責架構、邊界、驗證、review 與提交。

本文件建立整份資料包的共同基礎：以 Human-led Architecture 為主軸，搭配 prompt / XML 約束、Git diff review、test harness、invariants、edge cases、permission boundary 與 context / memory management。後續文件會把這些原則分別轉成同學操作指南、建置指南、XML 模板與評分建議。
