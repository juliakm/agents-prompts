---
description: "Sync active Azure DevOps user stories to Apple Reminders with #work tag"
mode: agent
tools: ["mcp_azure-devops_wit_my_work_items", "mcp_azure-devops_wit_get_work_items_batch_by_ids", "run_in_terminal", "create_file"]
---

# Sync Active Work Items to Apple Reminders

Fetch my active (non-closed) User Story work items from Azure DevOps and add them to Apple Reminders.

## Step 1: Get assigned work items

Use the Azure DevOps MCP server to get my assigned work items:

- Organization: `my-devops-org` (replace with your DevOps organization)
- Project: `my-project` (replace with project)
- Use `mcp_azure-devops_wit_my_work_items` with `type: assignedtome`, `includeCompleted: true`, `top: 200`, `project: my-project`

## Step 2: Get work item details

Take all the work item IDs returned and fetch their details in batches of 50 using `mcp_azure-devops_wit_get_work_items_batch_by_ids`.

## Step 3: Filter to active User Stories

From the results, keep only items where:
- `System.WorkItemType` equals `User Story`
- `System.State` is NOT `Closed` and NOT `Resolved`

Extract these fields for each matching item:
- `System.Id` (the work item ID)
- `System.Title`
- `System.State`

## Step 4: Generate and run AppleScript

Create a temporary AppleScript file at `/tmp/add_reminders.scpt` that adds each work item as a reminder in Apple Reminders.

Each reminder should have:
- **Name**: `{ID} - {Title} #work`
- **Body/Notes**: `State: {State} | https://dev.azure.com/my-devops-org/my-project/_workitems/edit/{ID}`

Important AppleScript rules:
- Use arrays (lists) for names and notes to avoid special character issues in inline strings
- Use a `repeat` loop to create reminders from the arrays
- Add to the default reminders list
- Sanitize titles: remove brackets like `[` and `]` from titles to avoid AppleScript syntax issues

Run the script with `osascript /tmp/add_reminders.scpt`.

## Step 5: Report results

Tell me how many reminders were added and list them in a table with columns: ID, State, Title.
