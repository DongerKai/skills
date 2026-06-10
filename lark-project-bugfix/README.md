# Lark Project Bugfix

语言：中文 | [English](#english)

用于让 AI coding agent 分析 Lark/Meego/Jira bug 或缺陷工单，读取工单详情、评论、文档、截图和当前代码仓库，并在修改代码前输出人工确认方案。

## 安装

```bash
npx skills add DongerKai/lark-project-bugfix
```

## 使用

在支持 skills 的 agent 中调用：

```text
$lark-project-bugfix <Lark/Meego/Jira bug 工单链接>
```

该 skill 会先完成问题分析、代码排查和修复方案说明，并要求用户确认后才开始修改代码。

## 仓库结构

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

---

## English

Language: [中文](#lark-project-bugfix) | English

Analyze Lark/Meego/Jira bug or defect work items with an AI coding agent. The skill inspects work item details, comments, documents, screenshots, and the current repository, then pauses for human confirmation before code changes.

## Installation

```bash
npx skills add DongerKai/lark-project-bugfix
```

## Usage

Invoke it in an agent that supports skills:

```text
$lark-project-bugfix <Lark/Meego/Jira bug work item URL>
```

The skill analyzes the bug, inspects relevant code, proposes a fix, and waits for user confirmation before modifying code.

## Repository Layout

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```
