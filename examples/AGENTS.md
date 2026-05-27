# AGENTS.md

## 專案目標

本專案目標：TODO: 簡短描述專案要解決的問題、主要使用者與交付成果。

Agent 可以協助 inspect、plan、patch、debug、test generation 與 documentation，但不得取代人類對需求、架構、驗證與最終提交的判斷。

## Agent 工作原則

- 使用 Human-led Architecture：人類負責目標、架構、批准、review、測試與最終責任。
- 先 inspect，再 plan；未經人類批准前不要修改檔案。
- 任務必須遵守本檔案、`PROJECT_CONTEXT.md` 與 `INVARIANTS.md`。
- 如果任務範圍不清楚，先停止並詢問人類。
- 如果需要超出 allowed scope，先停止並要求批准。
- 修改後必須回報 diff summary、測試結果與未解決風險。

## 允許行為

- 讀取專案內與任務相關的檔案。
- 整理專案結構、風險與下一步建議。
- 提出實作計畫與測試計畫。
- 在人類批准後，修改明確允許的檔案。
- 產生或補強測試，但不得降低測試強度。
- 更新指定文件，並清楚標示已完成、未完成、待確認事項。

## 禁止行為

- 不得自動 `git commit`。
- 不得自動 `git push`。
- 不得自動 push main。
- 不得自動 deploy。
- 不得刪除檔案，除非人類明確批准。
- 不得修改 `.env`。
- 不得讀取無關私人檔案或使用者目錄。
- 不得提交 API key、密碼、token、`.env` 或私人資料。
- 不得執行 `sudo` 或高風險命令，除非人工批准。
- 不得自行更改測試來掩蓋問題。
- 不得把推測寫成已確認事實。

## Review-only 任務規則

如果任務標示為 review-only、inspect-only、plan-only 或 risk-review：

- 不得修改檔案。
- 不得建立新檔案。
- 不得刪除檔案。
- 不得執行 git commit。
- 不得執行 git push。
- 只能回報觀察、風險、建議與需要確認的問題。

## 驗證要求

修改後必須回報：

- 修改檔案清單。
- Diff summary。
- 執行過的測試或檢查。
- 無法執行測試的原因。
- 仍需人類確認的風險。

如果專案沒有測試，請提出可重現的手動驗證步驟。

## Invariants

所有修改都必須尊重 `INVARIANTS.md`。

如果修改可能破壞 invariants，請停止並回報：

1. 可能被破壞的規則。
2. 為什麼目前任務需要觸碰該規則。
3. 建議的人類決策選項。

## 回報格式

請用以下格式回報：

```markdown
## Summary
- TODO

## Changed Files
- TODO

## Verification
- TODO

## Risks / Follow-up
- TODO
```
