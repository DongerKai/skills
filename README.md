# AI Agent Skills

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
