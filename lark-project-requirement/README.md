# Lark Project Requirement

语言：中文 | [English](#english)

用于让 AI coding agent 分析 Lark/Meego/Jira 需求、故事、任务或项目工单，读取工单详情、评论、文档、图片和当前代码仓库，并在实现前输出人工确认方案。

## 安装

```bash
npx skills add DongerKai/lark-project-requirement
```

## 使用

在支持 skills 的 agent 中调用：

```text
$lark-project-requirement <Lark/Meego/Jira 需求工单链接>
```

该 skill 会先完成需求理解、代码影响分析和实现方案说明，并要求用户确认后才开始修改代码。

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

Language: [中文](#lark-project-requirement) | English

Analyze Lark/Meego/Jira requirements, stories, tasks, or project work items with an AI coding agent. The skill inspects work item details, comments, documents, images, and the current repository, then pauses for human confirmation before implementation.

## Installation

```bash
npx skills add DongerKai/lark-project-requirement
```

## Usage

Invoke it in an agent that supports skills:

```text
$lark-project-requirement <Lark/Meego/Jira requirement work item URL>
```

The skill analyzes the requirement, inspects relevant code, proposes an implementation plan, and waits for user confirmation before modifying code.

## Repository Layout

```text
.
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```
