---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据 issue 实现筛选功能，再用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能。更大的更改则需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的开发流程，帮助我们以正确的方式构建正确的功能。从本课开始，你将遵循常见的智能体驱动开发流程：从 issue 着手实现新功能，确认代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能开发会继续使用同一对话和分支。通常，不同类型的文件会分别放在不同分支或 PR 中；本工作坊为了聚焦核心概念，简化了这一流程。

在本课中，你将：

- 根据 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加模型上下文协议（MCP）验证、创建质量保证（QA）智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 描述了功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式确定这些决策，再授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变软件开发的基本原则。事实上，这些原则变得更加重要。大多数开发者遵循的流程大致如下：

1. 从描述需求的现有 issue 开始。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述做法相似。

遵循这种标准流程，可确保 AI 生成的代码符合既定需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式，再审查计划，最后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能的工作前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

此时，提示中会出现 `#7`（或类似编号）。`#` 可将 GitHub 上的 issue 或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先明确功能或任务的实现方式，再交给 Copilot 执行。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定适当的实现方案并制定计划。

3. Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划制定并审查完成后，就可以开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你安装的版本中名称相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前经过审查。现在检查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 根据 issue 和规划阶段确定的决策检查实现结果。
4. 完成代码审查后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果；如果有失败项，请让 Copilot 修复。

## 体验新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及按计划确定的组合方式筛选。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结和下一步

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 根据 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，你将[深入了解代码生成方式，确保代码符合已记录的实践][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
