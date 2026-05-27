# AI Agent Workflow 資料包

## 目的

這份資料包用來協助教授、同學與實作者，以可審查、可測試、可追溯的方式導入 AI Agent Workflow。

核心目標不是讓 AI 取代學習，而是建立一套 Human-led Architecture：由人負責需求、判斷、審查與責任，Agent 作為協助 inspect、規劃、修改、測試與文件整理的工具。

## 這份資料包不是什麼

- 不是鼓勵學生把作業丟給 AI。
- 不是單一工具推薦文。
- 不是取代 Git、測試、review 的捷徑。
- 不是保證任何 Agent 都會產生正確結果的承諾。
- 不是不需要理解程式與系統設計的自動化方案。

## 適合誰閱讀

| 讀者 | 適合閱讀原因 |
| --- | --- |
| 教授與助教 | 評估課堂導入方式、風險控管、評分 rubric 與學習價值。 |
| 同學 | 學習如何安全使用 Agent，並保留可審查的學習與修改紀錄。 |
| 專題小組 | 建立團隊協作規則、分支流程、測試流程與 Agent 任務邊界。 |
| 工具建置者 | 建置 Agent 工具、MCP、Skills、本地模型與測試環境。 |

## 5 分鐘 Quick Start

第一次拿到這份 repo 的同學，可以先照這個最小流程做：

1. 先讀完本 README，確認這份資料包的用途與限制。
2. 在自己的專案中確認 Git 狀態：

```bash
git status
```

3. 使用 Agent 前，先保存目前進度：

```bash
git add .
git commit -m "Save current work before agent changes"
```

4. 為本次 Agent 任務開 branch：

```bash
git switch -c agent-task/example-task
```

5. 複製 `prompts/01-inspect-only.xml`，先要求 Agent 只 inspect，不要修改檔案。
6. 若後續批准 Agent 修改，修改後一定要檢查：

```bash
git status
git diff
```

7. 不要讓 Agent 直接 push 到 `main`。需要同步到 GitHub 時，先 review，再由人類決定是否 push branch 或開 pull request。

## 如果你是教授

建議閱讀順序：

1. `docs/01-教授版摘要.md`
2. `docs/02-主文件-AI-Agent-Workflow導入說明.md`
3. `docs/06-風險控管與評分建議.md`
4. `docs/03-同學版使用指南.md`

閱讀重點：

- 這套流程如何維持人主導。
- 如何要求學生留下 Git diff、測試結果與決策紀錄。
- 如何把 AI Agent 使用納入評分，而不是放任不可追溯的產出。

## 如果你是同學

建議閱讀順序：

1. `docs/03-同學版使用指南.md`
2. `docs/05-XML-Prompt模板.md`
3. `docs/06-風險控管與評分建議.md`
4. `docs/02-主文件-AI-Agent-Workflow導入說明.md`

閱讀重點：

- 使用 Agent 前先確認需求、限制與 Git 狀態。
- 修改後一定要看 `git diff`。
- 不要讓 Agent 直接 push 到 `main`。
- 用 branch、commit、測試與 review 保留自己的學習紀錄。

## 如果你要實際建置工具

建議閱讀順序：

1. `docs/04-建置指南.md`
2. `docs/02-主文件-AI-Agent-Workflow導入說明.md`
3. `docs/05-XML-Prompt模板.md`
4. `examples/AGENTS.md`
5. `examples/PROJECT_CONTEXT.md`
6. `examples/INVARIANTS.md`
7. `references/source-notes.md`

閱讀重點：

- 工具只是工作流的一部分，不是完整治理方案。
- Codex、Claude Code、VS Code Copilot Agent、Cline 應作為不同情境下的工具例示。
- 涉及安裝、MCP、Ollama、本地模型限制與版本資訊時，安裝前請確認官方文件。

## 如果你不熟 Git/GitHub

如果還不熟 Git 與 GitHub，請先閱讀：

- `docs/03-同學版使用指南.md` 的 Git/GitHub 基礎章節
- `docs/04-建置指南.md` 的環境建置與版本控制章節
- `docs/06-風險控管與評分建議.md` 的審查與分支保護章節

這些章節會說明 Git、GitHub、repository、commit、branch、remote、pull、push、pull request、`.gitignore`，以及為什麼使用 Agent 前要先 commit、修改後要檢查 `git diff`。

## 六份文件導覽

| 文件 | 用途 | 主要讀者 |
| --- | --- | --- |
| `docs/01-教授版摘要.md` | 快速說明教學目標、學習價值、風險與評分方向。 | 教授、助教 |
| `docs/02-主文件-AI-Agent-Workflow導入說明.md` | 定義整體架構、核心概念、工具定位與導入流程。 | 教授、助教、同學 |
| `docs/03-同學版使用指南.md` | 說明同學如何實際使用 Agent、Git/GitHub 與審查流程。 | 同學、專題小組 |
| `docs/04-建置指南.md` | 說明工具、專案、版本控制、本地模型與測試環境建置方向。 | 實作者、助教、專題小組 |
| `docs/05-XML-Prompt模板.md` | 說明如何用 XML prompt 限制任務範圍與輸出格式。 | 同學、助教 |
| `docs/06-風險控管與評分建議.md` | 提供風險控管、Git diff review、test harness 與評分 rubric。 | 教授、助教、同學 |

## examples/ 與 prompts/ 的用途

| 目錄 | 用途 |
| --- | --- |
| `examples/` | 放置專案治理檔案範例，例如 `AGENTS.md`、`PROJECT_CONTEXT.md`、`INVARIANTS.md` 與任務模板。 |
| `prompts/` | 放置 XML prompt templates，例如 inspect-only、plan-only、patch、debug、test generation、documentation。 |

`examples/` 用來定義專案規則與不可破壞的限制；`prompts/` 用來把單次 Agent 任務切成可檢查、可批准、可回報的步驟。

## 建議導入流程

1. Inspect：先檢查 repo、需求、限制、既有測試與風險。
2. Plan：提出修改計畫、影響範圍與驗證方式。
3. Approve：由人確認計畫是否可執行。
4. Patch：只在批准範圍內修改。
5. Test：執行測試或檢查，記錄結果。
6. Review：閱讀 `git diff`，確認修改是否合理。
7. Commit：由人整理 commit 訊息並提交。

## 核心原則

| 原則 | 說明 |
| --- | --- |
| Human-led Architecture | 人負責目標、判斷、批准、審查與最終責任；Agent 只負責協助執行受限任務。 |
| Git diff review | 每次 Agent 修改後都要檢查 diff，確認是否有越界、錯誤或無關變更。 |
| Test harness | 以測試、檢查指令或可重現步驟驗證修改結果。 |
| Invariants | 明確列出不能被破壞的規則，例如 API contract、資料格式、權限限制與安全要求。 |
| XML semantic constraints | 用結構化 prompt 限制任務範圍、輸入、輸出與禁止事項。 |
| Context / memory management | 控制 Agent 可用脈絡，避免 context window overflow、過期記憶或未審查資訊影響結果。 |

## 簡短 Glossary

| 術語 | 說明 |
| --- | --- |
| AI Agent | 能根據任務讀取脈絡、提出計畫、使用工具或協助修改的 AI 系統。 |
| Human-led Architecture | 人類主導需求、架構、批准、驗證與責任；Agent 只在受限範圍內協助。 |
| Prompt Engineering | 把任務、限制、背景、輸出格式與驗證方式寫清楚的規格化過程。 |
| XML semantic constraints | 用 XML 標籤區分任務、範圍、禁止事項、測試與輸出，降低任務模糊度。 |
| Invariants | 修改前後都不能被破壞的不變條件，例如 API contract、資料格式、權限規則。 |
| Edge Cases | 邊界條件與錯誤案例，例如空值、權限不足、重複資料、外部服務失敗。 |
| Test Harness | 用來驗證修改結果的測試、檢查指令或可重現流程。 |
| Context Window | Agent 單次可處理的對話與檔案脈絡容量。 |
| Memory Governance | 管理硬體記憶體、context、長期專案記憶與過期資訊的治理方式。 |
| MCP | Model Context Protocol，讓 Agent 連接外部工具、資料或服務的協定。 |
| Skills | 可重用的 Agent 能力或流程封裝。 |
| AGENTS.md | repo 內給 Agent 讀的專案規則檔。 |
| Git diff review | 使用 `git diff` 檢查 Agent 實際修改內容的人工審查流程。 |

## 後續維護事項

- 補充 `references/source-notes.md` 的官方資料來源與查證日期。
- 依課堂需求調整 examples/ 內的治理檔範例。
