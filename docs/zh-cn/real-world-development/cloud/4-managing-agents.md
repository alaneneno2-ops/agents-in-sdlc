---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个旨在改善用户体验和增加功能的任务。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制面板中，可以查看所有拥有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，也可以引导正在进行的会话。

在本练习中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以趁此添加浅色模式。既然正在更新网站样式并添加模式切换功能，加入浅色模式就很合适。现在引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包括你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成任务所需的各项操作。

> [!NOTE]
> 页面中显示的具体步骤会根据 Copilot 当前的工作状态和执行方式而变化。

4. 查看右侧显示的 PR 面板，以便进一步监控 PR 和更改的文件。

## 引导云智能体

现在你已查看正在进行的任务，可以让 Copilot 在实现高对比度模式时也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![智能体控制面板中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示设置已完成并开始执行任务。出现与下方示例相似的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务屏幕截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。在收到消息时，它会先完成当前步骤，再将新请求纳入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结和下一步

本练习介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过该控制面板跟踪正在进行和已完成的任务、查看工作内容，并重定向正在运行的任务来调整范围或提供额外指导。

你探索了以下内容：

- 使用智能体控制面板和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来审查并[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
