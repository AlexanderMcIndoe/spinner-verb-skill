---
name: spinner-verbs
description: This skill should be used when the user asks to "change spinner verbs", "change spinning verbs", "change thinking verbs", "update my spinner", "pick a verb theme", or wants to customize the Claude Code loading verbs.
---

# Spinner Verbs Skill

Interactively pick a verb theme and apply it to `~/.claude/settings.json`.

## Step-by-step

### 1. Load packs

Read the single index file:
```
~/.claude/plugins/spinner-verbs/packs/index.json
```

This is a JSON array of pack objects, each with `name`, `mode`, and `verbs`.

If the file is missing, tell the user:
> "No packs found. Run `/plugin update` to get the latest packs."
Then stop.

### 2. Present the list interactively (paginated)

`AskUserQuestion` has a hard limit of 4 options. Show packs 3 at a time with a "More themes →" option to advance pages, and a "Keep current" option on the last page.

- Pages except the last: 3 pack options + "More themes →"
- Last page: remaining packs + "Keep current"

Each pack option:
- **label**: the theme `name`
- **description**: all verbs joined with ` · `
- **preview**: verbs listed one per line

If the user picks "More themes →", show the next page.
If the user picks "Keep current", say "No changes made." and stop.

### 3. Apply the selected theme

1. Read `~/.claude/settings.json`
2. Set `settings.spinnerVerbs` to `{ "mode": <mode from pack>, "verbs": <verbs from pack> }`
3. Write the updated settings back

Confirm with: `Spinner verbs updated to "<name>". Restart your Claude Code session to see the new verbs.`
