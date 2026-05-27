# PROJECT_CONTEXT.md

## 專案名稱

TODO: 填入專案名稱。

## 專案目的

TODO: 用 3 到 5 句說明本專案要解決的問題、主要使用者、核心功能與交付成果。

## 技術棧

| 類別 | 內容 |
| --- | --- |
| 前端 | TODO: 例如 React、Vue、HTML/CSS。 |
| 後端 | TODO: 例如 Node.js、Python、Java。 |
| 資料庫 | TODO: 例如 PostgreSQL、SQLite、無資料庫。 |
| 測試 | TODO: 例如 pytest、Jest、Playwright。 |
| 部署 | TODO: 例如 GitHub Pages、Docker、尚未部署。 |

## 主要資料夾

| 路徑 | 用途 |
| --- | --- |
| `src/` | TODO: 主要程式碼。 |
| `tests/` | TODO: 測試檔案。 |
| `docs/` | TODO: 文件。 |
| `scripts/` | TODO: 開發或維護腳本。 |

## 目前進度

- 已完成：TODO
- 進行中：TODO
- 尚未完成：TODO
- 已知風險：TODO

## 已知限制

- TODO: 目前不支援的功能。
- TODO: 尚未驗證的情境。
- TODO: 技術債或需要重構的區域。
- TODO: 外部服務、帳號或 API 限制。

## 本地模型 / Context Window 注意事項

- 不要假設本地模型能一次理解整個大型 codebase。
- 大任務請切成 inspect、plan、patch、test、review。
- 若 context window 不夠，請先產生摘要，再分段處理。
- 對重要結論重新 inspect 或測試，不要只依賴舊對話。
- 本地模型速度、品質與可用 context 受 RAM、VRAM、模型大小與量化方式影響。

## Memory Governance

本檔案是專案外部記憶的一部分，目的是提供 Agent 穩定背景，但不能取代實際 inspect。

維護原則：

- 重要架構與規則寫在 repo 文件中，不只放在聊天記憶。
- 過期資訊要移除或標記。
- 重要決策要寫清楚日期與原因。
- Agent 若發現本檔案與實際檔案不一致，應停止並回報。

## 不應放進長期記憶的資訊

- API key、密碼、token。
- `.env` 內容。
- 私人資料、個資、未授權資料。
- 暫時性錯誤推測。
- 尚未確認的工具版本或安裝細節。
- 課堂作業答案或不應共享的評分資料。

## 如何更新本檔案

更新時機：

- 專案架構有明顯改變。
- 新增或移除主要資料夾。
- 技術棧改變。
- 重要限制、風險或決策改變。
- Agent 多次誤解同一個背景。

更新流程：

1. 先確認目前 Git 狀態。
2. 修改本檔案。
3. 檢查 diff。
4. 在 commit message 說明更新原因。

```bash
git status
git diff
```
