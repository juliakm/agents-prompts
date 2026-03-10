---
description: Add an AI-Enhanced section with MCP Server prompts to an existing article
agent: Doc-Kit Author
---

# Add AI Assistance Section

Add the AI-Enhanced section pattern to the following article, enabling readers to use the Azure DevOps MCP Server for the article's topic area.

## Target Article

${file}

## Instructions

Perform all four of the following changes to the target article:

### 1. Update frontmatter metadata

Add or update these fields in the YAML frontmatter (after existing `ms.*` fields):

```yaml
ai-usage: ai-assisted
ms.date: 2026-03-10
ms.custom: copilot-scenario-highlight
```

If `ms.custom` already has a value, append `copilot-scenario-highlight` as a comma-separated entry (e.g., `ms.custom: existing-value, copilot-scenario-highlight`).

### 2. Add the AI include tip before Prerequisites

Insert the following line immediately **before** the **Prerequisites** section (or before the first H2 section if no Prerequisites section exists):

```markdown
[!INCLUDE [ai-assistance](../../includes/ai-assistance-mcp-server-tip.md)]
```

Adjust the relative path based on the article's folder depth under `docs/`:
- `docs/service/article.md` → `../includes/...`
- `docs/service/subfolder/article.md` → `../../includes/...`
- `docs/service/subfolder/deeper/article.md` → `../../../includes/...`

### 3. Add anchor before Related content

Insert this anchor immediately before the **Related content** section (or before the last section in the article if no Related content exists). If a **Next steps** section is present, place the anchor after it:

```html
<a id="use-ai-assistance"></a>
```

### 4. Add the AI section with topic-specific prompts

After the anchor and before **Related content**, add a new section following this template:

```markdown
## Use AI to [task description]

If you configure the [Azure DevOps MCP Server](../../mcp-server/mcp-server-overview.md), you can [brief description of what users can do with natural language for this topic].

| Task | Example prompt |
|------|----------------|
| [Task 1] | `[Natural language prompt with <placeholders> for user-specific values]` |
| [Task 2] | `[Natural language prompt with <placeholders> for user-specific values]` |
| ... | ... |

> [!TIP]
> If you're using Visual Studio Code, [agent mode](/visualstudio/ide/copilot-chat-context#agent-mode) is especially helpful for [brief description of the AI-assisted scenario].
```

Adjust the MCP Server link relative path to match the article's folder depth.

**Prompt guidelines:**
- Write 5-10 example prompts specific to the article's topic
- Use `<angle-bracket placeholders>` for all user-specific values (e.g., `<project-name>`, `<organization-name>`, `<me>`). Use `<Contoso>` as an example project name.
- Cover a range of tasks from simple (view or list) to more complex (create, batch, or triage)
- Make prompts realistic and actionable — avoid vague or overly generic prompts
- Use specific field names, states, and concepts from the article's domain
- Ensure prompts are realistic and supported by MCP Server tools

## Reference examples

See these articles for the established pattern:
- `docs/boards/backlogs/define-features-epics.md` — features and epics prompts
- `docs/boards/backlogs/manage-bugs.md` — bug management prompts
- `docs/boards/backlogs/manage-work-items.md` — work item prompts
- `docs/pipelines/get-started/what-is-azure-pipelines.md` — pipeline management prompts
- `docs/boards/work-items/view-add-work-items.md` — Boards pattern
- `docs/organizations/accounts/resolve-orphaned-organization.md` — Organization settings pattern
