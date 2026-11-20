---
title: "课程 3 - 智能体模式：计划和自动驾驶"
description: "使用计划模式确定方案、自动驾驶模式根据议题构建筛选功能，并在交互模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有工作流，帮助我们以正确方式构建正确功能。本课是系列课程的第一课，你将遵循典型的智能体驱动开发流程：从议题着手开发新功能、确认代码有效且功能符合预期，最后成功合并到项目。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本研讨会会采用捷径，让你专注于核心概念。

本课将：

- 根据 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩充，访客需要按类别和出版商筛选游戏。待办事项议题描述了此功能，但组合类别等细节需要在编写代码前确定。你将使用计划模式敲定这些决定，然后授权自动驾驶模式执行范围明确的实现。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则，事实上这些原则变得更加重要。大多数开发人员的流程大致如下：

1. 从描述需求的已提交议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体步骤会因团队和组织而异，但大多数流程都与上述流程类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并经历与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **交互模式**：你与智能体共同工作。智能体提出更改建议，并等待你输入后再继续。
- **计划模式**：智能体先创建计划，且不能编辑项目文件。
- **自动驾驶模式**：智能体自主编写代码、运行测试并反复迭代，直至任务完成。

先使用计划模式并审查计划，然后使用自动驾驶模式实现。

## 从议题开始

开始筛选工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键突出显示标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示词并返回 **Session** 选项卡。

注意提示词现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方案，再交给 Copilot。处理复杂任务时，花时间规划通常是有益的。现在切换到计划模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步加入的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能应该如何工作。根据你的偏好作答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，让 Copilot 修改。

## 批准自动驾驶模式

计划撰写并审查完成后，现在可以开始实现。使用自动驾驶模式让 Copilot 执行计划。

自动驾驶模式允许 Copilot 反复处理问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相同的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后实现会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，应像审查其他代码一样，在合并前审查。检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能实现和测试。
3. 对照议题和规划阶段做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但是否能正常运行？像之前一样启动应用程序，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用程序：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、出版商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果状态符合议题和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你已使用不同的对话模式构建并审查功能。本课中，你：

- 根据 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来[使用自定义说明][next-lesson]，深入了解如何让代码遵循书面实践。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
