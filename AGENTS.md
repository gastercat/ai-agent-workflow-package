# AGENTS.md

## Project Goal

This repository is a documentation package for introducing AI Agent Workflow to a professor and classmates.

The deliverables are six Markdown documents plus XML prompt templates and example project governance files.

This package should be an actionable operation manual, not only a concept summary or slide-style overview.

## Language

Write all user-facing documents in Traditional Chinese.

Use clear Markdown structure.

Avoid hype. Emphasize Human-led Architecture, safety, review, testing, and learning value.

## Working Rules

- Do not invent unsupported facts.
- When mentioning current products, mark them as tool examples, not universal recommendations.
- Present Codex, Claude Code, VS Code Copilot Agent, and Cline fairly. Do not frame any one tool as the only recommendation.
- Compare tools by use case where helpful, such as beginners, CLI users, VS Code users, local model users, MCP needs, classroom demos, and capstone projects.
- For current tools, installation steps, MCP, Ollama, and local model limits, verify against official sources before writing specific instructions.
- If not verified, do not write fixed version numbers, fragile commands, or claims that may expire. Mark such sections with "安裝前請確認官方文件".
- Prefer concise explanations with tables and workflows.
- Do not overwrite files unless explicitly asked.
- Do not delete files.
- Do not run destructive commands.
- Do not run git commit or git push unless explicitly requested.
- Always summarize changed files after editing.
- Write one document at a time when drafting full content. Do not generate all full-length documents in one pass.
- After each edit, report a diff summary.

## Required Documents

Create and maintain:

1. docs/01-教授版摘要.md
2. docs/02-主文件-AI-Agent-Workflow導入說明.md
3. docs/03-同學版使用指南.md
4. docs/04-建置指南.md
5. docs/05-XML-Prompt模板.md
6. docs/06-風險控管與評分建議.md

## Required Concepts

The documents should cover:

- Human-led Architecture
- Prompt Engineering
- XML semantic constraints
- Codex / Claude Code / VS Code Copilot Agent / Cline
- OpenClaw vs Hermes Agent
- Local model deployment
- Ollama / local model memory limits
- MCP
- Skills
- AGENTS.md
- Invariants
- Edge cases
- Context window overflow
- Memory governance
- Git diff review
- Test harness
- Education and grading rubric

## Tool Coverage Rules

The documents should cover these tool examples as comparable options:

- Codex
- Claude Code
- VS Code Copilot Agent
- Cline

Discuss them by learning and project context instead of ranking them as universal recommendations.

## OpenClaw vs Hermes Agent Scope

Cover OpenClaw and Hermes Agent as advanced Agent architecture case studies, not as primary coding agents.

- OpenClaw: Agent entry point, communication platform, MCP bridge, personal assistant gateway.
- Hermes Agent: persistent memory, self-improving agent, skills, long-term user model.
- Focus on conceptual comparison. Do not write full installation tutorials unless later requested.

## Git and GitHub Coverage

Add Git and GitHub basics for classmates who are not yet familiar with version control.

Git/GitHub content should appear in:

- docs/03-同學版使用指南.md
- docs/04-建置指南.md
- docs/06-風險控管與評分建議.md

README.md may point beginners to the Git/GitHub sections.

At minimum, cover:

- What Git is.
- What GitHub is.
- Meanings of repository, commit, branch, remote, pull, push, and pull request.
- git init
- git status
- git add
- git commit
- git diff
- git log
- git branch
- git checkout or git switch
- git remote add origin
- git push
- git pull
- .gitignore
- Why to commit before using an Agent.
- Why to inspect git diff after Agent edits.
- Why not to let an Agent push directly to main.
- How to protect main with branches.
- How to organize Agent edits into reviewable records.

## Workflow

Use this work pattern:

1. Inspect current files.
2. Propose a plan.
3. Wait for approval before large rewrites.
4. Modify only requested files.
5. Report what changed.
