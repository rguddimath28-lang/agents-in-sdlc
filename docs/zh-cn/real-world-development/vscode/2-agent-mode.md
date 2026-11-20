---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件并再次更新，整个流程往往如此反复。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可在 IDE 中更自主地工作。它会先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来将了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和出版商筛选。上一课已经添加了出版商帮助函数，接下来将使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解其现有功能。

该网站是采用开发者主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用程序，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问帮助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、作用域明确的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单一 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回上一课使用的 Codespace。此时应已位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace（现已托管 Web 服务器）的专用隧道。

### 浏览网站

网站运行后，可以浏览现有功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、出版商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的对话。不同练习所用模式和模型各不相同，每个步骤会在相关处说明。

网站的初始实现已可以运行，但还需要增加功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题；请让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以保证良好的体验。但 LLM 具有概率性，因此建议可能与课程示例不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还会根据它认为对用户最有用的事项为你排列优先级。

## 查看说明文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的说明文件。项目中的[用户界面（UI）说明][ui-instructions]介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中有关添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，因此可以复用指导、将复杂说明拆分成更小的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选功能至少需要对应用程序进行三项更改：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面以加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件。它应会找到数据层帮助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示对应的 **Keep** 或 **Undo** 选项，因此不必在工作进行时选择。

7. 按 Copilot 提示选择 **Continue** 以运行测试。

   ![Copilot Chat 面板请求用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会出现暂停，也可能有测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![包含完整 Copilot 智能体模式会话的 Chat 截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解到如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 在将更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 拓展练习 — 实现分页

随着游戏列表不断增加，需要启用分页。使用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 确定分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示词时，为 Copilot 提供实现所需的指导。
- 你可能需要与 GitHub Copilot 迭代，通过补充上下文来要求修改。这是与 Copilot 协作的正常流程！

## 资源

- [Copilot 的询问、编辑和智能体模式：用途及适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
