# Source Notes

## 查證日期

- 2026-05-28

## 使用原則

- 本檔案記錄文件撰寫時參考的官方來源。
- 工具功能與安裝方式可能變動，使用前請再次確認官方文件。
- 本資料包中的工具介紹為教學與比較用途，不代表唯一推薦。
- 不在本檔案寫死工具版本號、安裝細節或短期內容易變動的功能承諾。
- 若來源不足、功能快速變動，或需要精確教學步驟，請標明「需再查證」。

## Sources

### Codex CLI

- 官方來源：
  - OpenAI Codex official GitHub: https://github.com/openai/codex
- 用於支援本資料包中的哪些描述：
  - Codex CLI 作為 CLI coding agent 的工具例示。
  - 在 repository 中讀取、修改、測試與回報變更的 agent workflow。
  - AGENTS.md 作為專案規則檔的概念說明。
  - Skills 相關描述僅作為 Codex 生態系能力封裝的概念提示；若要寫具體使用方式需再查證官方文件。
- 注意事項：
  - 安裝方式、登入方式、模型支援、權限設定與 Skills 細節可能變動，使用前請再次確認官方 repo 或官方文件。
  - 不應把 Codex CLI 寫成唯一推薦工具。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/05-XML-Prompt模板.md

### Claude Code

- 官方來源：
  - Anthropic Claude Code overview: https://docs.anthropic.com/en/docs/claude-code/overview
  - Anthropic Claude Code MCP documentation: https://docs.anthropic.com/en/docs/claude-code/mcp
- 用於支援本資料包中的哪些描述：
  - Claude Code 作為 terminal / agentic coding tool 的工具例示。
  - Claude Code 可與 MCP 搭配的概念說明。
  - CLI 使用者、需要 agentic coding workflow 的使用情境比較。
- 注意事項：
  - 指令、授權、模型、MCP 設定與安全預設可能更新，安裝前請確認官方文件。
  - 不應把 Claude Code 寫成唯一推薦工具。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### VS Code Copilot Agent

- 官方來源：
  - VS Code Copilot Chat Agent Mode documentation: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
  - GitHub Copilot documentation: https://docs.github.com/en/copilot
- 用於支援本資料包中的哪些描述：
  - VS Code 內建 Copilot Chat / Agent Mode 作為 IDE 內 agent workflow 的工具例示。
  - 適合 VS Code 使用者、課堂 demo、初學者熟悉 IDE 情境的工具定位。
- 注意事項：
  - VS Code extension、GitHub Copilot plan、Agent Mode 功能與 UI 可能變動，使用前請確認 Microsoft / VS Code / GitHub 官方文件。
  - 不應把 VS Code Copilot Agent 寫成唯一推薦工具。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md

### Cline

- 官方來源：
  - Cline official docs: https://docs.cline.bot/
  - Cline official GitHub: https://github.com/cline/cline
- 用於支援本資料包中的哪些描述：
  - Cline 作為 open-source coding agent 的工具例示。
  - Plan / Act 工作模式、MCP、本地模型與 VS Code extension 情境比較。
  - 適合想觀察 agent 修改流程、需要本地模型或自訂模型入口的使用者。
- 注意事項：
  - 支援模型、MCP 設定、Plan / Act UI 與 extension 權限可能更新，使用前請確認官方文件。
  - 本地模型能力不等同雲端 coding agent，需依模型、硬體與 context window 評估。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### Continue

- 官方來源：
  - Continue official docs: https://docs.continue.dev/
  - Continue official GitHub: https://github.com/continuedev/continue
- 用於支援本資料包中的哪些描述：
  - Continue 作為 VS Code / JetBrains 內可配置 assistant / agent workflow 的延伸工具例示。
  - rules、models、MCP 與自訂模型設定的概念說明。
  - 作為主要四種工具之外的補充路線。
- 注意事項：
  - Continue 的 agent 功能、設定格式、模型供應商與 MCP 支援可能變動，使用前請確認官方文件。
  - 本資料包只把 Continue 作為補充工具，不列為主要四種比較工具之一。
- 相關文件：
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### Ollama

- 官方來源：
  - Ollama official docs: https://ollama.com/docs
  - Ollama official GitHub: https://github.com/ollama/ollama
- 用於支援本資料包中的哪些描述：
  - Ollama 作為本地模型執行與本地部署的工具例示。
  - 本地模型部署需要考慮模型大小、硬體資源、context window 與推理速度。
- 注意事項：
  - 模型清單、指令、硬體需求、context window 與效能會依模型和平台變動，使用前請確認官方文件與模型頁面。
  - 本地模型可能較便宜或較能保留資料控制權，但不代表能力一定等同 Codex、Claude Code 或其他雲端 coding agent。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### Open WebUI

- 官方來源：
  - Open WebUI official docs: https://docs.openwebui.com/
  - Open WebUI official GitHub: https://github.com/open-webui/open-webui
- 用於支援本資料包中的哪些描述：
  - Open WebUI 作為 self-hosted AI platform 的補充路線。
  - 可作為實驗室或課堂共用入口，連接 Ollama 或 OpenAI-compatible APIs 的概念說明。
- 注意事項：
  - 部署方式、支援後端、權限控管與 API 設定可能變動，建置前請確認官方文件。
  - 若作為多人共用入口，需額外設計帳號、權限、資料隔離與紀錄保留策略。
- 相關文件：
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### MCP

- 官方來源：
  - Model Context Protocol documentation: https://modelcontextprotocol.io/docs/getting-started/intro
  - Model Context Protocol GitHub organization: https://github.com/modelcontextprotocol
- 用於支援本資料包中的哪些描述：
  - MCP 是讓 AI applications / agents 連接 tools、data sources 與 external systems 的標準協定。
  - MCP 可連接 filesystem、Git、GitHub、database、browser、docs 等外部能力的概念說明。
  - MCP 權限應最小化，初學者不應一次接太多 server。
- 注意事項：
  - MCP server 生態、設定方式與安全建議會持續變動，實作前請確認官方文件與各 server 文件。
  - MCP 不是越多越好；每個 server 都應有明確用途與權限邊界。
- 相關文件：
  - README.md
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md
  - docs/05-XML-Prompt模板.md

### OpenClaw

- 官方來源：
  - OpenClaw official docs: https://docs.openclaw.ai/
  - OpenClaw official GitHub: https://github.com/openclaw/openclaw
  - OpenClaw MCP docs: https://docs.openclaw.ai/cli/mcp
- 用於支援本資料包中的哪些描述：
  - OpenClaw 作為 advanced Agent architecture case study，而不是主要 coding agent。
  - Agent entry point、communication platform、channels、Gateway、MCP bridge、personal assistant gateway 等概念比較。
- 注意事項：
  - 需再查證：OpenClaw 功能、CLI、Gateway、channels、MCP bridge 與部署方式仍可能快速變動。
  - 本資料包只做概念比較，不提供完整安裝教學。
  - 若後續要寫安裝或 demo，必須重新查證官方 docs 與 GitHub。
- 相關文件：
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md

### Hermes Agent

- 官方來源：
  - Hermes Agent official docs: https://hermes-agent.nousresearch.com/docs/
  - Nous Research GitHub organization: https://github.com/nousresearch
  - Hermes Agent GitHub repository: https://github.com/NousResearch/hermes-agent
- 用於支援本資料包中的哪些描述：
  - Hermes Agent 作為 advanced Agent architecture case study，而不是主要 coding agent。
  - persistent memory、self-improving agent、skills、long-term user model 等概念比較。
- 注意事項：
  - 需再查證：persistent memory、skills、自我改進機制、支援平台與部署方式可能快速變動。
  - 本資料包只做概念比較，不提供完整安裝教學。
  - 若要討論具體安全風險、資料儲存位置或部署步驟，必須重新查證官方 docs 與 GitHub。
- 相關文件：
  - docs/02-主文件-AI-Agent-Workflow導入說明.md
  - docs/06-風險控管與評分建議.md

### Git / GitHub

- 官方來源：
  - Git official documentation: https://git-scm.com/doc
  - GitHub Get started documentation: https://docs.github.com/en/get-started
  - GitHub Git basics: https://docs.github.com/en/get-started/git-basics
  - GitHub Pull requests documentation: https://docs.github.com/en/pull-requests
- 用於支援本資料包中的哪些描述：
  - Git、GitHub、repository、commit、branch、remote、pull、push、pull request 等基本概念。
  - 使用 Agent 前先 commit、每個任務開 branch、修改後看 git diff、不要讓 Agent 直接 push main。
  - 以 commit、diff、branch、PR 保留可審查紀錄的教學。
- 注意事項：
  - Git 指令本身穩定，但 GitHub UI、branch protection、PR review 設定可能變動；涉及平台畫面操作時請確認官方文件。
  - 教學範例中的 remote URL、branch name 與 commit message 應依專案調整。
- 相關文件：
  - README.md
  - docs/03-同學版使用指南.md
  - docs/04-建置指南.md
  - docs/06-風險控管與評分建議.md
