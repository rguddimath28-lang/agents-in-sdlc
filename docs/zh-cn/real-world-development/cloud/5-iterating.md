---
title: "练习 5：迭代改进 GitHub Copilot 的成果"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改善用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。接下来将查看代码更改，必要时向 Copilot 提供反馈以改进成果。

### 场景

如前面多次强调的，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于此，在为团队其他成员启用审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全：

- Copilot 对存储库只有读取权限，并且仅对用于提交代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR）：为代码添加文档。此处将使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。你也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息片刻或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。浏览时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。
6. 检查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查列表。如果一切正常，单一 Astro 应用程序的项目检查就会通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像在团队成员评论中那样，在拉取请求评论或代码行内评论中标记 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法提供固定的请求文本。可以尝试要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用程序结构。

1. 在评论中标记 **@copilot** 并请求更改。可以使用上面的想法，或提出其他代码文档方面的建议。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并执行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为可供审查，然后合并到 main 分支。

![将 PR 转为可供审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求的检查列表。如果一切正常，单一 Astro 应用程序的项目检查就会通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代改进 Copilot 云智能体的成果。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此议题时还未更新自定义说明）。与之前一样，可以在 **Conversation** 选项卡中发表评论提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在 mission control 里要求的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更新没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求的检查列表。如果一切正常，单一 Astro 应用程序的项目检查就会通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，在浏览器中测试高对比度和浅色模式开关，确认功能正常且刷新页面后设置仍然保留。

注意，无障碍自定义智能体如何帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍方面的问题或改进空间，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实的积压工作，让你可以把注意力转向其他事情。可以在自己的存储库中创建更多议题并分配给 Copilot，以培养这种工作方式。以下是一些想法：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问帮助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习路径。在这些课程中，你：

- **检查了存储库提供的自定义说明文件**，以便之后观察云智能体输出中的影响。
- **将议题分配给 Copilot 云智能体**，观察它如何设置环境、制定计划并异步执行。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **将 Copilot 智能体页面用作 mission control**，监控并指导进行中的无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体练习路径。若想继续探索，其他练习路径可以补充本次实践的内容：

- 🖥️ **[VS Code 练习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验计划和自动驾驶模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
