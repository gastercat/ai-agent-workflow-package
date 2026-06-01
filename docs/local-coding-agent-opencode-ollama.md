# 本機 Coding Agent 備援教學：OpenCode + Ollama + 本地 Tool-Capable Coding Model

> 安裝前請確認官方文件：OpenCode、Ollama 與各模型的工具呼叫支援會持續變動。本文是泛用專案教學，不針對任何特定個人專案。

## 目的

這份教學說明如何把 OpenCode、Ollama 與本地 coding model 組成一個「備援型」本機 coding agent 工作流。

它適合用在：

- 課堂示範本地模型與 coding agent 的分工。
- 需要避免把程式碼送到雲端模型的練習環境。
- 想比較雲端 coding agent 與本機模型限制的專題。
- 需要在網路不穩或 API 額度不足時保留最低限度輔助能力。

它不適合被當成「完全自動改程式」方案。本地模型的工具使用、長上下文理解、跨檔案修改與測試修復能力通常仍需要更多人工審查。

## 三個元件的分工

| 元件 | 角色 | 負責事項 | 不應負責事項 |
| --- | --- | --- | --- |
| OpenCode | Coding agent 介面與工具層 | 讀取專案、提出計畫、呼叫工具、套用修改、執行檢查指令。 | 決定需求是否正確、直接 push、跳過人工審查。 |
| Ollama | 本地模型執行環境 | 在本機載入與服務模型，提供 chat 或 OpenAI-compatible API。 | 保證每個模型都能可靠使用工具。 |
| Local tool-capable coding model | 推理與工具呼叫決策 | 根據上下文判斷是否要讀檔、搜尋、修改、執行測試。 | 取代測試、review、版本控制與安全限制。 |

建議把 OpenCode 視為「有工具的工作台」，Ollama 視為「本地模型服務」，模型則是「可被替換的推理核心」。

## Tool-Capable Model 與一般 Instruct/Chat Model 的差異

| 類型 | 常見能力 | 在 coding agent 中的限制 |
| --- | --- | --- |
| 一般 instruct/chat model | 回答問題、解釋程式、產生片段、摘要文件。 | 可能只把工具呼叫格式當成文字輸出，不會真正觸發讀檔、改檔或測試工具。 |
| Tool-capable model | 能以模型支援的格式要求呼叫工具，例如讀檔、搜尋、執行命令、套用 patch。 | 仍可能因模型、量化版本、context 長度、provider 設定或 agent 權限而失敗。 |

判斷重點不是模型名稱看起來像 coding model，而是：

- 官方模型頁或文件是否明確支援 tool calling / function calling。
- OpenCode 的 provider 設定是否把該模型接到可用的工具呼叫介面。
- OpenCode 的 permissions 是否允許必要工具，例如 read、grep、edit、bash。
- 實際測試時，模型是否真的呼叫工具，而不是只輸出 JSON 或 shell 指令文字。

## 最小安全工作流

1. 進入一個泛用練習專案，不要從重要 production branch 開始。
2. 使用 Agent 前先確認工作樹：

```bash
git status
```

3. 若已有重要修改，先由人類整理並 commit，或另開 branch 保存。
4. 啟動 OpenCode 後，先要求只 inspect，不要修改。
5. 批准修改前，要求 Agent 列出修改範圍、風險與驗證方式。
6. 修改後一定由人類執行：

```bash
git status
git diff
```

7. 不要要求 Agent commit。
8. 不要要求 Agent push。
9. 不要讓 Agent 直接推送到 main 或預設分支。

## 安裝與設定方向

安裝前請確認官方文件。以下只保留不綁定版本號的方向。

### 1. 安裝 OpenCode

OpenCode 官方文件提供多種安裝方式，例如 install script、npm、Homebrew、系統套件管理器或 Docker。若使用 npm 全域安裝遇到權限問題，優先修正 npm 權限或改用不需要 sudo 的安裝方式，不要直接用 `sudo npm install -g` 作為預設解法。

安裝後先確認 CLI 可用：

```bash
opencode --help
```

### 2. 安裝與啟動 Ollama

依 Ollama 官方文件安裝後，確認服務可用：

```bash
ollama --help
ollama list
```

再依模型官方頁面拉取支援 coding 與 tool calling 的模型。不要只看模型名稱；應確認該模型在 Ollama 與 OpenCode 整合下能實際觸發工具。

### 3. 設定 OpenCode 使用 Ollama

OpenCode 可透過設定檔配置 provider、model、permissions 與 agent。實際欄位請以 OpenCode 官方文件為準。

建議設定原則：

- 專案層設定只放本專案需要的限制。
- 預設讓 `bash` 需要人工批准。
- 對 `edit` 與 `write` 採取明確允許或明確拒絕，不依賴模糊預設。
- 初次測試先用小型、可丟棄的練習 repo。
- 若工具呼叫不穩，先用 inspect-only 工作流，不要直接讓 Agent 修改。

## 建議測試任務

用一個簡單泛用專案測試，例如小型 CLI、todo API、單頁前端或演算法練習。測試任務可採用：

1. 要求 Agent 讀取 README 與測試檔，只摘要專案結構。
2. 要求 Agent 找出一個小 bug，但不要修改。
3. 要求 Agent 提出 patch plan。
4. 人類批准後才允許修改單一檔案。
5. 修改後執行測試或最小檢查指令。
6. 人類閱讀 `git diff`，確認沒有無關修改。

## 安全 Prompt Template

可在 OpenCode 中貼上以下 prompt，作為本機 coding agent 的安全起點：

```xml
<task>
  <goal>
    Inspect this generic project and propose a small, reviewable change.
  </goal>

  <scope>
    You may read files needed to understand the project.
    Do not edit files until I explicitly approve a plan.
    Do not create commits.
    Do not push to any remote.
  </scope>

  <safety_rules>
    Treat the human as the final decision maker.
    Before any edit, list the exact files you intend to change.
    Keep changes minimal and related to the approved task.
    Do not modify secrets, credentials, lockfiles, generated files, or unrelated examples unless explicitly approved.
    After edits, report the commands run and summarize the diff.
  </safety_rules>

  <git_rules>
    Before edits, run git status.
    After edits, ask the human to inspect git status and git diff.
    Never run git commit.
    Never run git push.
    Never push to main or the default branch.
  </git_rules>

  <output_format>
    First provide: project summary, risks, proposed plan, validation steps.
    Wait for approval before modifying files.
  </output_format>
</task>
```

## Troubleshooting

### npm EACCES

症狀：

- 使用 npm 全域安裝 OpenCode 時出現 `EACCES` 或 permission denied。

處理方向：

- 先查 npm 官方建議的權限修正方式。
- 避免把 `sudo npm install -g` 當成教學預設，因為它可能造成後續檔案權限混亂。
- 可改用 OpenCode 官方提供的其他安裝方式，例如 Homebrew、install script、Docker 或使用 Node 版本管理工具。
- 修正後重新執行：

```bash
opencode --help
```

### model does not support tools

症狀：

- OpenCode 或 Ollama 回報模型不支援 tools。
- 模型把工具呼叫 JSON 印成普通文字。
- Agent 只說「我可以修改」，但沒有真正讀檔或改檔。

處理方向：

- 確認模型官方文件明確支援 tool calling / function calling。
- 確認使用的是支援工具呼叫的模型標籤，而不是只有 chat/instruct 的標籤。
- 確認 OpenCode provider 指向正確的 Ollama API 介面。
- 確認 OpenCode permissions 允許 read、grep、edit 或 bash 等必要工具。
- 減小任務範圍，先測試讀檔與搜尋，再測試修改。
- 若仍不穩，改用另一個 tool-capable coding model，或把本機模型限定為「解釋與建議」，由人類手動套用 patch。

### agent not found

症狀：

- OpenCode 回報找不到指定 agent。
- 使用 `@agent-name` 或自訂 agent 名稱時無法啟動。

處理方向：

- 確認 agent 設定檔是否存在於 OpenCode 官方文件指定的位置。
- 確認 agent 名稱、檔名與引用名稱一致。
- 確認目前專案設定與全域設定沒有互相覆蓋。
- 用 OpenCode 的 agent 管理指令檢查目前可用 agent。
- 若不確定，先使用 OpenCode 預設 agent 跑 inspect-only 任務，再逐步加入自訂 agent。

## 審查清單

每次使用本機 coding agent 後，至少檢查：

```bash
git status
git diff
```

人工 review 時確認：

- 是否只修改批准範圍內的檔案。
- 是否有未要求的格式化、搬移或重寫。
- 是否碰到 secrets、設定檔、產生檔或鎖定檔。
- 是否有測試或檢查結果。
- 是否留下可理解的修改理由。
- 是否需要把修改拆成更小的 commit。

最後由人類決定是否 commit。不要要求 Agent 直接 commit 或 push。

## 官方資料來源

查證日期：2026-06-01。

- OpenCode 官方文件：https://opencode.ai/docs/
- OpenCode tools 與 permissions：https://opencode.ai/docs/tools/
- OpenCode CLI agent 指令：https://opencode.ai/docs/cli/
- OpenCode config：https://opencode.ai/docs/config
- Ollama 官方文件：https://docs.ollama.com/
- Ollama tool calling：https://docs.ollama.com/capabilities/tool-calling
