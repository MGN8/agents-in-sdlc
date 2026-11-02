---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理工作。只需在 GitHub 上分配任务，云智能体便会在后台接手工作：探索存储库、进行更改并创建拉取请求，让你可以同时处理其他事项。

本系列练习将先添加云智能体会遵循的自定义说明，然后分配一个 GitHub 议题，让它实现相应工作。接着审查并使用自定义智能体来调整其工作方式，在智能体仪表板中监控并引导会话，最后审查它创建的拉取请求并继续迭代。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体会遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 GitHub Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为账户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过验证的学生可以使用 [GitHub Education][student-plan] 免费获得 GitHub Copilot。**Copilot Student** 计划包含本研讨会所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成本系列的所有课程。

[student-plan]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，所有计划（Copilot Student、Pro、Pro+、Business 和 Enterprise）均提供云智能体。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本系列不涉及 MCP。要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 系列][cli]或 [VS Code 系列][vscode]。

## 开始学习

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
