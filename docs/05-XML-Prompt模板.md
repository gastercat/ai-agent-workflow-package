# XML Prompt 模板

## 1. 文件目的

本文件提供同學與專題組可直接複製使用的 XML Prompt 模板庫，用來控制 AI Agent 的工作範圍、輸出格式、禁止事項與驗證方式。

這些模板可放入 Codex、Claude Code、Cline、VS Code Copilot Agent 或其他支援長 prompt 的工具中使用。本文件不推薦單一工具，重點是建立可審查、可測試、可回退的 Agent workflow。

## 2. XML Prompt 是什麼

XML Prompt 是用 XML 標籤把任務拆成不同區塊的 prompt 寫法。它不是新的模型能力，而是一種把自然語言任務規格化的方法。

範例：

```xml
<task>
  <goal>請先 inspect 專案，不要修改檔案。</goal>
  <allowed_scope>只能讀取目前 repo。</allowed_scope>
  <forbidden_scope>不得建立、修改或刪除檔案。</forbidden_scope>
  <output_format>請回報專案結構、主要檔案、風險與下一步。</output_format>
</task>
```

## 3. 為什麼 XML 可以提供語意約束

XML 的價值在於把不同意義的內容分區，讓 Agent 比較不容易把背景、限制、任務與輸出格式混在一起。

主要優點：

- 把任務、背景、限制、驗證方式分區。
- 降低 Agent 把背景說明誤認成指令的機率。
- 讓硬性限制更清楚。
- 方便複用。
- 方便團隊共用。
- 方便放進 `AGENTS.md`、`TASK_TEMPLATE.xml` 或專案任務卡。
- 方便後續被工具或腳本解析。

限制也要清楚：

- XML 不是保證 Agent 一定聽話。
- XML 不能取代測試。
- XML 不能取代 `git diff` review。
- XML 不能取代人類判斷。
- Prompt 太長仍然可能受 context window 限制。

## 4. XML 和一般自然語言 Prompt 的差異

| 寫法 | 優點 | 風險 |
| --- | --- | --- |
| 一般自然語言 | 容易寫、適合簡單問題 | 限制容易被埋在段落中，任務邊界不清楚。 |
| XML Prompt | 任務、限制、驗證、輸出格式清楚分區 | 較長，需要維護模板。 |

簡單問題可以用自然語言；涉及改檔、Git、測試、專題任務時，建議使用 XML Prompt。

## 5. 一個標準 Agent Prompt 應包含哪些區塊

建議包含：

1. Agent 角色
2. Workflow 原則
3. 任務目標
4. 專案背景
5. 允許範圍
6. 禁止範圍
7. Invariants
8. Edge cases
9. 執行規則
10. 驗證方式
11. 輸出格式
12. 停止條件

## 6. 核心 XML 標籤說明

| 標籤 | 用途 |
| --- | --- |
| `<agent_role>` | 定義 Agent 的角色，例如 coding assistant、reviewer、documentation assistant。 |
| `<workflow_principle>` | 強調 Human-led Architecture、先 inspect、先 plan、等待批准。 |
| `<task>` | 描述這次任務的目標與類型。 |
| `<project_context>` | 放專案背景、技術棧、目前狀態或已知問題。 |
| `<allowed_scope>` | 明確列出可讀、可分析、可修改的範圍。 |
| `<forbidden_scope>` | 明確列出不可做的事情，例如不可刪檔、不可 push、不可改 main。 |
| `<invariants>` | 列出不能被破壞的規則，例如 API contract、資料格式、安全限制。 |
| `<edge_cases>` | 列出必須考慮的邊界情境。 |
| `<execution_rules>` | 定義執行順序，例如 inspect → plan → approve → patch。 |
| `<verification>` | 定義測試、檢查或人工驗證方式。 |
| `<output_format>` | 規定 Agent 回答格式，方便 review。 |
| `<stop_conditions>` | 定義何時必須停止並詢問人類。 |

## 7. Inspect-only 模板

用途：只讀取與理解專案，不得修改檔案。此處為摘要範例，完整可複製模板請使用 `prompts/01-inspect-only.xml`。

```xml
<prompt>
  <agent_role>你是專案 inspect assistant，只能觀察與整理資訊。</agent_role>
  <workflow_principle>先理解現況，不修改檔案，等待人類下一步指示。</workflow_principle>
  <task>請 inspect 目前專案。</task>
  <allowed_scope>允許讀取目前 repo 的檔案與目錄結構。</allowed_scope>
  <forbidden_scope>不得修改、建立、刪除檔案；不得 git commit；不得 git push。</forbidden_scope>
  <output_format>回報專案結構、主要檔案、風險、下一步建議、需要確認的問題。</output_format>
</prompt>
```

## 8. Plan-only 模板

用途：只提出實作計畫，不得修改檔案。此處為摘要範例，完整可複製模板請使用 `prompts/02-plan-only.xml`。

```xml
<prompt>
  <agent_role>你是 planning assistant。</agent_role>
  <task>請根據需求提出實作計畫，不要修改檔案。</task>
  <forbidden_scope>不得修改、建立、刪除檔案；不得執行 git commit 或 git push。</forbidden_scope>
  <output_format>列出預計修改檔案、步驟、測試計畫、風險，並等待人類批准。</output_format>
</prompt>
```

## 9. Patch 模板

用途：在批准後執行小範圍修改。此處為摘要範例，完整可複製模板請使用 `prompts/03-patch.xml`。

```xml
<prompt>
  <agent_role>你是 coding assistant。</agent_role>
  <task>請在已批准範圍內執行小範圍修改。</task>
  <allowed_scope>只能修改人類明確批准的檔案。</allowed_scope>
  <forbidden_scope>不得自動 git commit；不得自動 git push；不得修改未批准檔案。</forbidden_scope>
  <verification>執行或說明測試方式。</verification>
  <output_format>回報 diff summary、測試結果、風險。</output_format>
</prompt>
```

## 10. Debug 模板

用途：根據錯誤訊息定位問題，不要一開始就亂改。此處為摘要範例，完整可複製模板請使用 `prompts/04-debug.xml`。

```xml
<prompt>
  <agent_role>你是 debugging assistant。</agent_role>
  <task>請根據錯誤訊息定位問題。</task>
  <execution_rules>先分析錯誤，再提出假設與驗證方式；未批准前不要修改。</execution_rules>
  <output_format>回報錯誤摘要、可能原因、驗證方式、建議修改範圍。</output_format>
</prompt>
```

## 11. Test generation 模板

用途：產生測試案例。此處為摘要範例，完整可複製模板請使用 `prompts/05-test-generation.xml`。

```xml
<prompt>
  <agent_role>你是 test generation assistant。</agent_role>
  <task>請為指定功能產生測試案例。</task>
  <edge_cases>請包含正常案例、edge cases、錯誤案例。</edge_cases>
  <forbidden_scope>不得為了通過測試而修改 production code，除非另行批准。</forbidden_scope>
  <output_format>回報測試清單、覆蓋情境、需要確認的假設。</output_format>
</prompt>
```

## 12. Documentation 模板

用途：產生文件與報告。此處為摘要範例，完整可複製模板請使用 `prompts/06-documentation.xml`。

```xml
<prompt>
  <agent_role>你是 documentation assistant。</agent_role>
  <task>請產生或更新文件。</task>
  <forbidden_scope>不得捏造未執行的測試；不得假裝已完成未完成的功能。</forbidden_scope>
  <output_format>區分已完成、未完成、風險、待確認。</output_format>
</prompt>
```

## 13. Risk review 模板

用途：檢查 Agent 修改是否破壞 invariants、權限、安全或資料格式。

```xml
<prompt>
  <agent_role>你是 risk review assistant，負責審查 Agent 修改風險。</agent_role>
  <workflow_principle>只做審查與回報，不修改檔案。</workflow_principle>
  <task>
    請檢查目前變更是否破壞 invariants、權限、安全、資料格式或測試假設。
  </task>
  <project_context>
    TODO: 貼上專案背景、重要架構與目前 diff 摘要。
  </project_context>
  <allowed_scope>
    允許讀取 diff、測試輸出、相關檔案與專案規則。
  </allowed_scope>
  <forbidden_scope>
    不得修改檔案。
    不得執行 git commit。
    不得執行 git push。
  </forbidden_scope>
  <invariants>
    TODO: 列出不能被破壞的 API contract、資料格式、權限規則。
  </invariants>
  <edge_cases>
    TODO: 列出空值、權限不足、重複資料、網路失敗等情境。
  </edge_cases>
  <execution_rules>
    先列出風險，再列出需要人類確認的問題。
  </execution_rules>
  <verification>
    檢查是否有對應測試或可重現驗證步驟。
  </verification>
  <output_format>
    1. 高風險項目
    2. 中低風險項目
    3. 破壞的 invariants
    4. 缺少的 edge cases
    5. 建議補測試
    6. 是否建議合併
  </output_format>
  <stop_conditions>
    如果缺少 diff、測試輸出或 invariants，請停止並要求補資料。
  </stop_conditions>
</prompt>
```

## 14. Refactor 模板

用途：要求 Agent 重構，但不得改變外部行為。

```xml
<prompt>
  <agent_role>你是 refactoring assistant，負責改善內部結構但不改變外部行為。</agent_role>
  <workflow_principle>先提出 refactor plan，等待批准後才修改。</workflow_principle>
  <task>
    請針對指定範圍提出重構計畫，目標是提升可讀性、降低重複或改善結構。
  </task>
  <project_context>
    TODO: 說明目前問題與相關檔案。
  </project_context>
  <allowed_scope>
    TODO: 只能分析或修改指定檔案。
  </allowed_scope>
  <forbidden_scope>
    不得改變 public API。
    不得改變資料格式。
    不得改變使用者可見行為。
    不得執行 git commit 或 git push。
  </forbidden_scope>
  <invariants>
    TODO: 列出外部行為、API contract、測試必須維持的規則。
  </invariants>
  <edge_cases>
    TODO: 列出重構後仍必須通過的邊界情境。
  </edge_cases>
  <execution_rules>
    先提出 plan；等待人類批准；修改必須小步進行。
  </execution_rules>
  <verification>
    重構前後必須執行相同測試，或說明無法執行測試的原因。
  </verification>
  <output_format>
    1. 重構目標
    2. 預計修改檔案
    3. 不變的外部行為
    4. 測試方式
    5. 風險
  </output_format>
  <stop_conditions>
    如果無法確認外部行為或測試方式，請停止並詢問人類。
  </stop_conditions>
</prompt>
```

## 15. 如何搭配 AGENTS.md

`AGENTS.md` 適合放長期規則；XML Prompt 適合放單次任務。

| 檔案 | 放什麼 |
| --- | --- |
| `AGENTS.md` | 專案固定規則、禁止事項、語言、測試要求。 |
| `TASK_TEMPLATE.xml` | 單一任務卡格式。 |
| `prompts/*.xml` | 可複製使用的任務模板。 |

建議做法：

1. 在 `AGENTS.md` 寫固定規則，例如「不要自動 push main」。
2. 每次任務用 XML Prompt 寫清楚本次 allowed scope。
3. 修改後要求 Agent 回報 diff summary。
4. 人類仍要做 Git diff review。

## 16. 如何搭配 Git branch / diff review

建議流程：

```text
git status -> commit 目前進度 -> 建 branch -> 使用 XML Prompt -> 看 git diff -> 測試 -> commit
```

XML Prompt 中應明確寫：

- 不得自動 `git commit`。
- 不得自動 `git push`。
- 只能修改 `<allowed_scope>`。
- 修改後必須回報 diff summary。
- 如果需要超出範圍，必須停止並詢問。

## 17. 如何避免 Prompt 太長

Prompt 太長可能受 context window 限制，也會增加 Agent 忽略限制的機率。

做法：

- 把長期規則放進 `AGENTS.md`。
- 把專案背景放進 `PROJECT_CONTEXT.md`。
- 把不可破壞規則放進 `INVARIANTS.md`。
- 單次 XML Prompt 只放本任務需要的內容。
- 大任務切成 inspect、plan、patch、test、review。

## 18. 常見錯誤與改寫

| 錯誤寫法 | 問題 | 改寫 |
| --- | --- | --- |
| 幫我完成整個專案 | 範圍太大，不可審查 | 使用 Inspect-only 模板，先回報架構與下一步。 |
| 幫我修 bug | 缺少錯誤訊息與驗證方式 | 使用 Debug 模板，先分析錯誤、假設與驗證方式。 |
| 直接幫我改完並 push | 跳過 review，可能污染 main | 使用 Patch 模板，禁止 git commit / git push。 |
| 幫我重構全部 | 可能改變外部行為 | 使用 Refactor 模板，明確列出 invariants。 |
| 幫我寫報告 | 容易捏造進度或測試 | 使用 Documentation 模板，要求區分已完成與未完成。 |

## 19. 使用建議

- 第一次使用 Agent，先用 Inspect-only。
- 要改檔前，先用 Plan-only。
- Patch 要小，最好一次只處理一個功能或 bug。
- Debug 先找根因，不要一開始就改。
- 測試模板不能取代人類設計測試。
- 文件模板不能捏造未完成事項。
- 所有模板都要搭配 Git diff review。
- XML 是約束工具，不是安全保證。
