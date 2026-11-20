---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支中。现在需要一起审查这些内容并创建拉取请求。你已手动合并星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，筛选和质量工作流都保留在同一个对话和分支中，并通过此 PR 提交。

本课将：

- 了解 Agent Merge 以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时让开发人员保留合并授权权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复持续集成（CI）检查失败、回复审查评论，以及在需要时进行变基；之后，如果存储库允许，还会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为执行，但启用前仍应先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部完成后，一起审查代码、创建 PR，并让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监控 PR 的持续集成过程。一切成功后，它会执行合并。
7. 确认看到 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或缺失的权限。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成审查后的合并。

## 总结和下一步

你已自动化开发流程中的多个部分，包括代码生成、代码测试和验证，以及拉取请求处理。本课中，你：

- 了解了 Agent Merge 以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来[探索 Copilot CLI 中其他实用的斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
