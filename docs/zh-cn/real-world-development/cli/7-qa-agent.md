---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，整合 issue 需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright 模型上下文协议（MCP）在浏览器中观察筛选体验。现在将这些能力整合到一个自定义智能体中，并为其定义清晰的质量保证（QA）流程。

在本课中，你将：

- 探索自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选功能 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调此 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择该角色。

你使用的自定义项各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件指导 Copilot 使用这些能力评估需求并报告发现。QA 配置文件不会取代其他自定义项，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这可以避免在你只想验证功能时，智能体进行不完整或范围过大的修改。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加它可以为智能体设置清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请先让默认智能体修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话，再选择 QA，这样它可以使用已有的 issue 和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中。仍可使用上述命令将其启用。

2. 输入以下提示，让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体开始工作。
4. 完成后阅读它提供的报告。

## 总结和下一步

你已在工作流中添加了可复用的专用角色，并审查了它的工作。在本课中，你：

- 探索了自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选功能 issue 审查了它的发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告均已准备好接受审查。接下来将它们[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
