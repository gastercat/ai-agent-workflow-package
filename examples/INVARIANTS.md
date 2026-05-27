# INVARIANTS.md

## Invariants 定義

Invariants 是任何修改前後都必須維持的不變條件。Agent、同學與 reviewer 都必須先讀這份文件，再進行 patch、refactor、debug 或 test generation。

如果任務需要破壞或改變某個 invariant，必須先停止，提出原因與替代方案，等待人類批准。

## 安全 Invariants

- 不得提交 API key、密碼、token、`.env` 或私人資料。
- 登入失敗時不得洩漏密碼、token 或過度詳細的內部錯誤。
- 權限檢查不可被移除或繞過。
- 錯誤訊息不得暴露敏感資料。
- 不得讓測試或 demo 使用真實密鑰。

## 資料 Invariants

- 使用者資料格式不可未經批准改變。
- 既有資料遷移必須有明確計畫與驗證方式。
- 不得刪除 production 或共享資料。
- 不得用假資料覆蓋真實資料。
- 日期、金額、識別碼等欄位格式必須保持一致。

## API / Schema Invariants

- API response schema 不可未經批准改變。
- Public API 的欄位名稱、型別與錯誤格式不可任意修改。
- Database schema 變更必須有 migration 或明確說明。
- 前後端 contract 改變時，必須同步更新測試與文件。

## 測試 Invariants

- 不得刪除既有測試來讓修改通過。
- 不得降低 assertion 強度來掩蓋問題。
- 新功能應包含正常案例、edge cases 與錯誤案例。
- Bug fix 應優先補上可重現測試。
- 若無法執行測試，必須記錄原因與替代驗證方式。

## Git / GitHub Invariants

- 使用 Agent 前應先 commit 目前進度。
- 每個 Agent 任務應使用 branch。
- main branch 不可直接 push，除非團隊規則明確允許且已人工 review。
- Agent 不得自動 `git commit`。
- Agent 不得自動 `git push`。
- PR 或提交說明應包含修改目的、測試結果與已知風險。

## Agent Permission Invariants

- Review-only、inspect-only、plan-only 任務不得修改檔案。
- Agent 不得讀取無關私人檔案。
- Agent 不得修改 `.env`。
- Agent 不得自動 deploy。
- Agent 不得執行 `sudo` 或高風險命令，除非人工批准。
- Agent 不得自行修改測試來掩蓋 production code 問題。
- Agent 若需要超出 allowed scope，必須停止並詢問人類。

## Edge Cases Checklist

任務涉及功能修改時，至少檢查：

- [ ] 空輸入。
- [ ] 無效輸入。
- [ ] 極長輸入。
- [ ] 重複資料。
- [ ] 權限不足。
- [ ] 未登入狀態。
- [ ] 外部服務失敗。
- [ ] 網路 timeout。
- [ ] 並發或重複提交。
- [ ] 舊資料或 migration 情境。
- [ ] 錯誤訊息是否安全。

## 修改前檢查清單

- [ ] 我知道這次任務的 goal。
- [ ] 我知道 allowed scope。
- [ ] 我知道 forbidden scope。
- [ ] 我已確認目前 Git 狀態。
- [ ] 我已在需要時先 commit。
- [ ] 我已建立或切換到任務 branch。
- [ ] 我知道本次修改不能破壞哪些 invariants。
- [ ] 我知道要測哪些 edge cases。

## 修改後檢查清單

- [ ] 我已閱讀 `git diff`。
- [ ] 我已確認沒有修改未批准檔案。
- [ ] 我已確認沒有提交 API key、`.env` 或私人資料。
- [ ] 我已確認 invariants 沒有被破壞。
- [ ] 我已執行測試，或記錄無法執行的原因。
- [ ] 我已檢查 edge cases。
- [ ] 我能解釋這次修改。
- [ ] 我沒有讓 Agent 自動 commit。
- [ ] 我沒有讓 Agent 自動 push main。
- [ ] 我已記錄未解決風險與待確認事項。
