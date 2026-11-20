---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你要求 Copilot 云智能体执行了三项任务，以改善用户体验并添加功能。云智能体虽然能够异步自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，还可以指导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务。
- 指导正在进行的会话，要求添加其他功能。

## 场景

分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。由于当前正在更新网站样式并添加切换功能，因此将这项功能一并纳入很合理。接下来要指导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 注意主面板和左侧面板中的任务列表。列表中应能看到你分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 工作进度和采用的方案而异。

4. 另请留意右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 指导云智能体

了解当前任务后，现在要求 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择表示正在添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。出现类似下图的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认提示词，并将请求纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！完成发送消息时正在执行的步骤后，它会将新请求纳入工作流程。

同样，这需要几分钟。你可以趁此暂停片刻，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。在这个控制中心可以跟踪所有活动和已完成的任务、审查正在执行的工作，还可以重定向进行中的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的成果 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
