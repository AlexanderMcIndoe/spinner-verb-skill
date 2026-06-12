---
name: spinner-verbs
description: This skill should be used when the user asks to "change spinner verbs", "change spinning verbs", "change thinking verbs", "update my spinner", "pick a verb theme", or wants to customize the Claude Code loading verbs.
---

# Spinner Verbs Skill

Interactively pick a verb theme from the GitHub repo and apply it to `~/.claude/settings.json`.

## Step-by-step

### 1. Sync packs

Check when packs were last synced:
```bash
cat ~/.claude/plugins/spinner-verbs/.last_sync 2>/dev/null
```

The file contains a Unix timestamp.

- **If it doesn't exist**: write the current time and skip the sync — the local packs are already fresh from install.
```bash
date +%s > ~/.claude/plugins/spinner-verbs/.last_sync
```

- **If it exists and is more than 3 days old** (current `date +%s` minus timestamp > 259200): fetch fresh packs from GitHub:
  1. Use WebFetch to browse `https://github.com/AlexanderMcIndoe/spinner-verb-skill/tree/main/packs` and get the list of `.json` filenames.
  2. For each file, fetch its raw content from `https://raw.githubusercontent.com/AlexanderMcIndoe/spinner-verb-skill/main/packs/<filename>` and save it to `~/.claude/plugins/spinner-verbs/<filename>` using the Write tool.
  3. Update the timestamp: `date +%s > ~/.claude/plugins/spinner-verbs/.last_sync`

- **If it exists and is less than 3 days old**: skip the sync, use local packs as-is.

If a sync fails (no network), continue with whatever is already in `~/.claude/plugins/spinner-verbs/`.

If there are no local packs at all, tell the user:
> "No packs found and GitHub is unreachable."
Then stop.

### 2. Load packs

Read all `.json` files from `~/.claude/plugins/spinner-verbs/` (excluding `.last_sync`). Parse the `name` and `verbs` array from each.

### 3. Present the list interactively (paginated)

`AskUserQuestion` has a hard limit of 4 options. Show packs 3 at a time, with a "More themes →" option to advance pages and a "Keep current" option on the last page.

**Page structure (3 packs + 1 nav slot per page):**
- Pages except the last: 3 pack options + "More themes →"
- Last page: remaining packs + "Keep current"

Each pack option:
- **label**: the theme `name`
- **description**: all verbs joined with ` · `
- **preview**: verbs listed one per line

If the user picks "More themes →", show the next page.
If the user picks "Keep current", say "No changes made." and stop.

### 4. Apply the selected theme

1. Read `~/.claude/settings.json`
2. Set `settings.spinnerVerbs` to `{ "mode": <mode from file>, "verbs": <verbs from file> }` — strip the `name` field
3. Write the updated settings back

Confirm with one line: `Spinner verbs updated to "<name>".`

## Pack format

```json
{
  "name": "Theme display name",
  "mode": "replace",
  "verbs": ["First verb", "Second verb", "Third verb"]
}
```

- `mode`: `"replace"` (only your verbs) or `"append"` (mixed with Claude's defaults)
- `verbs`: present-participle strings — Claude adds the trailing `...`
