---
name: feishu-project-requirement
description: Use when the user provides a Feishu/Lark/Meego/Jira requirement, story, task, or project work item link and asks an AI coding agent to analyze the requirement, inspect details/comments/documents/images, compare it with the current repository, and pause for human confirmation before implementation. Also use when explicitly invoked as feishu-project-requirement or $feishu-project-requirement.
---

# Feishu Project Requirement

## Purpose

Turn a Feishu/Lark/Meego/Jira project work item link into a confirmed implementation plan, then complete the code change in the current repository after the user approves or adjusts the analysis.

This skill is for requirement work, not ordinary bug fixing. It is designed to work across AI coding agents and tool stacks. Preserve a human checkpoint between requirement analysis and code changes.

## Agent Compatibility

- Use the best available project-management integration first: MCP tools, platform APIs, browser automation, CLI tools, exported JSON, copied text, screenshots, or user-provided files.
- Use any available repository tools for code inspection and verification. Prefer fast search (`rg`/`ripgrep` if available), version-control diffs, build tools, and test runners.
- If a named tool is unavailable, do not stop just because the tool is missing. State what could not be accessed, use the accessible evidence, and ask only for information that materially affects implementation.
- Keep source notes separate: work item fields, comments, documents, images/prototypes, and repository findings.

## Workflow

### 1. Identify the work item

- Accept a Feishu/Lark/Meego/Jira project URL, or any text containing one.
- Prefer structured project tools when available because they can parse URLs and return work item data reliably.
- If structured tools are insufficient, use available alternatives such as browser access, platform CLI tools, downloaded/exported files, screenshots, copied descriptions, or user-provided context.
- If authentication or permissions are missing, tell the user exactly what cannot be read and continue with the accessible context.

Collect, when available:

- Work item ID, title, type, status, project key, owner/roles, priority, planned time, and linked nodes or subtasks.
- Description, custom fields, acceptance criteria, comments, operation history when useful, and related work items.
- Attached or linked documents, wiki pages, sheets, and files.
- Attached images, screenshots, diagrams, and image comments.

### 2. Read requirement documents and images

- For Feishu docs/wiki/sheets: extract the meaningful requirement text, tables, lists, links, and embedded references.
- For attachments: download or open only the files needed for requirement understanding.
- For images: inspect screenshots, prototypes, diagrams, error captures, field mappings, and annotations. Use local image viewing or OCR/vision-capable tools when available.
- Keep source notes: record which details came from the work item, comments, document text, image content, or repository inspection.

Do not assume missing product rules. If a requirement is ambiguous and materially affects behavior, include it in the confirmation questions.

### 3. Inspect the repository

Before proposing implementation, read local project guidance and relevant code:

- Check repository instructions such as `AGENTS.md`, `CLAUDE.md`, `.cursor/rules`, `.github/copilot-instructions.md`, or nested project guidance when present.
- Use fast project search first when available.
- Read relevant build files, API contracts, DTO/request/response classes, services, data access code, configuration, migrations, and nearby tests.
- Identify module boundaries and respect existing layering.
- Watch for domain boundaries such as cross-service calls, tenants, tokens, permissions, partitions, queues, real-time state, and database ownership.

### 4. Produce the human checkpoint

After requirement and code analysis, stop and output a concise analysis. Do not edit code yet.

The checkpoint should include:

- Requirement summary: what the user/product appears to want.
- Evidence: key points from the work item, documents, images, comments, and code.
- Current behavior: what the repository appears to do today.
- Proposed change: modules/files likely involved and the intended behavior after change.
- Data/API impact: new or changed API fields, database/migration needs, config changes, compatibility concerns, and deployment boundary concerns.
- Open questions: only questions that materially affect implementation.
- Suggested verification: minimal test, build, lint, or type-check commands to run.

End with a clear request for confirmation or adjustments, for example:

`请确认以上理解是否正确；确认后我再开始修改代码。`

### 5. Implement after confirmation

Only after the user confirms or adjusts the checkpoint:

- Apply the confirmed requirement exactly, using repository conventions.
- Keep changes focused. Avoid unrelated refactors, formatting churn, or broad rewrites.
- Add or update comments only for new public fields or non-obvious business rules when that matches the repository style.
- Avoid inefficient data access patterns such as queries inside loops; batch query and map in memory where practical.
- Preserve compatibility for existing APIs unless the user explicitly confirms a breaking change.
- Add schema migrations only when the requirement truly needs schema changes and the correct database ownership is clear.
- Run the smallest meaningful build/test target for the repository, such as targeted unit tests, package/build checks, lint/type checks, or framework-specific test commands.

### 6. Final response

Report:

- What changed.
- Where the important files are.
- What verification ran and any environment limitations.
- Any follow-up the user must handle outside the repository, such as product confirmation, database deployment, or Feishu work item updates.

## Tool Preferences

- Project/work-item tools: Feishu/Lark/Meego/Jira APIs, MCP servers, browser automation, CLI tools, exported files, or copied content.
- Document tools: Feishu/Lark docs, wiki, files, sheets, attachments, OCR, and image viewers when available.
- Repository tools: fast search, file readers, version-control diff, build systems, package managers, test runners, and linters.
- Image tools: inspect downloaded screenshots, diagrams, prototypes, recordings, and annotated images.

## Safety Rules

- Never change code before the analysis checkpoint is confirmed.
- Do not invent details for inaccessible documents, images, or work item fields.
- Do not use service tokens to bypass a requirement that depends on the real user's permissions.
- Do not update the Feishu work item status, comments, owners, or fields unless the user explicitly asks.
- If the implementation boundary is unclear, especially for cross-center or cross-partition behavior, ask before coding.
