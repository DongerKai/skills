# AI Agent Skills

语言：中文 | [English](#english)

这个仓库包含可复用的 AI coding agent skills。每个 skill 都是一个独立目录，包含必需的 `SKILL.md`，以及可选的 `agents/` 元数据。

## 可用 Skills

### `feishu-project-bugfix`

当用户提供 Feishu/Lark/Meego/Jira 的 bug 或缺陷工单链接，并希望 agent 在修改代码前先分析工单、评论、文档、截图和仓库代码时，使用这个 skill。

这个 skill 要求在修改代码前保留人工确认节点。

### `feishu-project-requirement`

当用户提供 Feishu/Lark/Meego/Jira 的需求、故事、任务或项目工单链接，并希望 agent 分析需求、查看关联文档和图片、对比当前仓库并产出实现方案时，使用这个 skill。

这个 skill 要求在开始实现前保留人工确认节点。

## 兼容性

这些 skills 按 agent-neutral 的方式编写，可用于 Codex，也可用于其他支持 skill 风格指令、项目工具、浏览器自动化、CLI 访问或仓库检查能力的 AI coding agents。

当某个特定集成不可用时，agent 应使用当前可用的最佳替代方式，例如平台 API、MCP 工具、浏览器访问、CLI 工具、导出文件、截图、复制的工单文本或用户提供的上下文。

## 仓库结构

```text
.
├── README.md
├── feishu-project-bugfix/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
└── feishu-project-requirement/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## 发布

推送前建议检查是否包含密钥或私有凭据：

```bash
rg "api_key|secret|token|password|AKIA|Bearer" .
```

然后提交并推送：

```bash
git add feishu-project-bugfix feishu-project-requirement README.md
git commit -m "Add Feishu project skills"
git push
```

---

## English

Language: [中文](#ai-agent-skills) | English

This repository contains reusable skills for AI coding agents. Each skill is a
self-contained directory with a required `SKILL.md` and optional agent metadata
under `agents/`.

## Available Skills

### `feishu-project-bugfix`

Use this skill when a user provides a Feishu/Lark/Meego/Jira bug or defect work
item link and wants the agent to inspect the report, comments, documents,
screenshots, and repository code before proposing a fix.

The skill requires a human confirmation checkpoint before code changes.

### `feishu-project-requirement`

Use this skill when a user provides a Feishu/Lark/Meego/Jira requirement, story,
task, or project work item link and wants the agent to analyze the requirement,
inspect linked documents and images, compare it with the repository, and produce
an implementation plan.

The skill requires a human confirmation checkpoint before implementation.

## Compatibility

These skills are written to be agent-neutral. They can be used by Codex or by
other AI coding agents that support skill-style instructions, project tools,
browser automation, CLI access, or repository inspection.

When a specific integration is unavailable, the agent should use the best
available alternative, such as platform APIs, MCP tools, browser access, CLI
tools, exported files, screenshots, copied work item text, or user-provided
context.

## Repository Layout

```text
.
├── README.md
├── feishu-project-bugfix/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
└── feishu-project-requirement/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Publishing

Before pushing changes, check for secrets or private credentials:

```bash
rg "api_key|secret|token|password|AKIA|Bearer" .
```

Then commit and push:

```bash
git add feishu-project-bugfix feishu-project-requirement README.md
git commit -m "Add Feishu project skills"
git push
```
