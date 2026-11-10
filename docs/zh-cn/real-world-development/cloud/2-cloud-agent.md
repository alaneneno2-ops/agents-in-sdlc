---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初承包网站开发的团队没有留下足够的文档，事实上几乎完全没有文档。首先，团队希望为应用中的所有导出函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页中显示同一类别的其他游戏，让用户可以继续浏览。当前不需要完善的实现，只需要能用于用户体验（UX）验收测试的版本。虽然这是一个阻碍，但目前还有优先级更高的 issue。

这些任务很容易被搁置，因此很适合交给 Copilot 云智能体异步处理。开发者可以先专注其他工作，之后再回来审查 Copilot 的结果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub issue 分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿 PR 跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。建议方案准备好供审查时，Copilot 会在 PR 中标记你。

## 清晰界定任务范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能包含复杂细节，因此[为 Copilot 云智能体界定任务范围时要谨慎][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、持续学习和尝试，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。如果你已完成自定义说明练习，就已看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中的说明文件，以便稍后观察它们的影响。

在 GitHub Web 界面或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` issue 分配给云智能体时，留意 PR 中的 TSDoc 注释、TypeScript 规范和文件头注释。这些要求来自上述说明文件；稍后审查 PR 时还会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保项目处于良好状态。将任务交给 Copilot 时也是如此；它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行，以便安装所需的开发工具和依赖项。实验已预先配置此工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库。项目是单个 Astro 应用：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理 PR 前由 GitHub Actions 执行。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这很适合提前测试，而不必等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务常常无人处理，降低开发效率，也会增加维护代码库或帮助新开发者加入团队的难度。Copilot 擅长生成文档，因此很适合将这项工作交给云智能体；稍后的练习会审查它生成的文档。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项清单。

> [!NOTE]
> Copilot 创建待办事项可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理问题的方案。

13. 查看待办事项清单及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定方案，以及编写代码。

这可能需要几分钟。云智能体的主要优势之一是可以异步执行任务，让开发者处理其他工作。现在利用这一点：再分配一个任务给云智能体，然后着手为应用添加功能。

## 为游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏功能不一定需要很长时间，也可以先交给云智能体，以便你腾出时间做其他工作。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，issue 中的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二个请求。云智能体的工作方式类似软件工程师，你无需持续监控，而是在它完成后审查结果。现在继续了解如何创建和使用自定义智能体。

## 总结和下一步

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将 issue 分配给云智能体，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云智能体正在后台工作，现在可以开始[创建和使用自定义智能体][next-lesson]。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可读取前面练习中配置的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
