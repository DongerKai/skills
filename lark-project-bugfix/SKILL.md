---
name: lark-project-bugfix
description: Use when the user provides a Lark/Meego/Jira bug or defect work item link and asks an AI coding agent to analyze the report, inspect details/comments/documents/images, compare the report with the current repository, and pause for human confirmation before modifying code. Also use when explicitly invoked as lark-project-bugfix or $lark-project-bugfix.
---

# Lark Project Bugfix

## Purpose

Turn a Lark/Meego/Jira bug work item link into a verified bug analysis and proposed code fix. Implement only after the user confirms or adjusts the plan.

This skill is for defect reports. It is designed to work across AI coding agents and tool stacks. Preserve a human checkpoint between analysis and code changes.

## Agent Compatibility

- Use the best available project-management integration first: MCP tools, platform APIs, browser automation, CLI tools, exported JSON, copied text, screenshots, or user-provided files.
- Use any available repository tools for code inspection and verification. Prefer fast search (`rg`/`ripgrep` if available), version-control diffs, build tools, and test runners.
- If a named tool is unavailable, do not stop just because the tool is missing. State what could not be accessed, use the accessible evidence, and ask only for information that materially affects the fix.
- Keep source notes separate: work item fields, comments, documents, images/logs, and repository findings.

## Workflow

### 1. Identify the bug report

- Accept a Lark/Meego/Jira project URL, or any text containing one.
- Prefer structured project tools when available because they can parse URLs and return work item data reliably.
- If structured tools are insufficient, use available alternatives such as browser access, platform CLI tools, downloaded/exported files, screenshots, copied descriptions, or user-provided context.
- If authentication or permissions are missing, state exactly what cannot be read and continue with accessible context.

Collect, when available:

- Work item ID, title, type, status, project key, priority/severity, reporter, owner/roles, affected version, environment, planned time, and linked nodes or subtasks.
- Bug description, reproduction steps, expected result, actual result, frequency, error messages, logs, stack traces, comments, operation history when useful, and related work items.
- Attached or linked documents, wiki pages, sheets, files, screenshots, screen recordings, diagrams, and image comments.

### 2. Inspect documents, images, and evidence

- For Lark docs/wiki/sheets: extract meaningful bug facts, tables, field mappings, test data, links, and embedded references.
- For attachments: download or open only files needed to understand or reproduce the bug.
- For images: inspect screenshots, prototypes, diagrams, browser/network captures, logs, exception dialogs, UI states, field values, timestamps, and annotations. Use local image viewing or OCR/vision-capable tools when available.
- Keep source notes: distinguish facts from the work item, comments, documents, images, logs, and repository inspection.
- Do not invent missing product rules. If ambiguity materially affects the fix, include it in the confirmation questions.

### 3. Inspect the repository before proposing a fix

- Read repository instructions such as `AGENTS.md`, `CLAUDE.md`, `.cursor/rules`, `.github/copilot-instructions.md`, or nested project guidance when present.
- Use fast project search first when available.
- Read relevant build files, API contracts, DTO/request/response classes, services, data access code, configuration, migrations, and nearby tests.
- Identify module boundaries and respect existing layering.
- Check whether the issue touches domain boundaries such as permissions, tenants, auth, tokens, partitions, queues, database ownership, cross-service calls, or real-time state.
- Compare reported behavior with current code to identify the likely root cause. If the report cannot be reproduced from code and evidence, say so and give the best-supported hypothesis.

### 4. Produce the human checkpoint

After bug and code analysis, stop and output a concise plan. Do not edit code yet.

The checkpoint should include:

- Bug summary: what fails, who is affected, and under what conditions.
- Evidence: key facts from the work item, comments, documents, images, logs, and code.
- Current behavior: what the repository appears to do today.
- Likely root cause: the specific code path, data condition, or integration behavior that explains the bug.
- Proposed fix: modules/files likely involved and intended behavior after the change.
- Compatibility and risk: API/data/config/migration impact, deployment boundary concerns, permission/token risks, and known side effects.
- Open questions: only questions that materially affect implementation.
- Suggested verification: minimal unit, integration, manual, build, lint, or type-check commands to run.

End with a clear confirmation request, for example:

`请确认以上分析和修改方案是否正确；确认后我再开始修改代码。`

### 5. Implement only after confirmation

After the user confirms or adjusts the checkpoint:

- Apply the confirmed fix exactly, using repository conventions.
- Keep changes focused. Avoid unrelated refactors, broad formatting churn, and import noise.
- Prefer backward-compatible changes for released APIs unless the user explicitly confirms a breaking change.
- Add or update comments only for new public fields or non-obvious business rules when that matches the repository style.
- Avoid inefficient data access patterns such as queries inside loops; batch query and map in memory where practical.
- Add schema migrations only when the fix truly needs schema changes and the correct database ownership is clear.
- Add or update tests when practical, especially for root-cause logic and regressions.
- Run the smallest meaningful verification target for the repository, such as targeted unit tests, package/build checks, lint/type checks, or framework-specific test commands.

### 6. Final response

Report:

- What changed and why it fixes the bug.
- Important files touched.
- Verification run and any environment limitations.
- Remaining product, data, deployment, or Lark/Jira follow-up if any.

## Tool Preferences

- Project/work-item tools: Lark/Meego/Jira APIs, MCP servers, browser automation, CLI tools, exported files, or copied content.
- Document tools: Lark docs, wiki, files, sheets, attachments, OCR, and image viewers when available.
- Repository tools: fast search, file readers, version-control diff, build systems, package managers, test runners, and linters.
- Image tools: inspect downloaded screenshots, diagrams, recordings, browser/network captures, and annotated images.

## Safety Rules

- Never modify code before the analysis checkpoint is confirmed.
- Do not invent inaccessible document, image, log, or work item details.
- Do not use service tokens to bypass a bug scenario that depends on real user permissions.
- Do not update Lark/Jira status, comments, owners, fields, or attachments unless the user explicitly asks.
- If the implementation boundary is unclear, especially cross-center, cross-partition, or database ownership behavior, ask before coding.
