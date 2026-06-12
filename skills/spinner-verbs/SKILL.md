---
name: spinner-verbs
description: This skill should be used when the user asks to "change spinner verbs", "change spinning verbs", "change thinking verbs", "update my spinner", "pick a verb theme", or wants to customize the Claude Code loading verbs.
version: 1.0.0
---

# Spinner Verbs Skill

Interactively pick a verb theme from the local pack library and apply it to `~/.claude/settings.json`.

## Step-by-step

### 1. Discover available themes

Run:
```bash
ls ~/.claude/plugins/spinner-verbs/*.json 2>/dev/null
```

If the directory doesn't exist or is empty, tell the user:
> "No verb packs found at `~/.claude/plugins/spinner-verbs/`. Copy some `.json` files from the `packs/` directory in this repo there to get started."

Then stop.

### 2. Read each config file

For every `.json` file found, `cat` it and parse out the `name` field and full `verbs` array.

### 3. Present the list interactively

Use `AskUserQuestion` with a single-select question. Each option should show:
- **label**: the theme `name` from the JSON (or the filename if `name` is missing)
- **description**: all verbs joined with ` · `
- **preview**: verbs listed one per line

Include a "Keep current" option as the last choice.

### 4. Apply the selected theme

If the user picks "Keep current", say "No changes made." and stop.

Otherwise:
1. Read `~/.claude/settings.json`
2. Set `settings.spinnerVerbs` to `{ "mode": <mode from file>, "verbs": <verbs from file> }` — strip the `name` field, it's metadata only
3. Write the updated settings back

Confirm with one line: `Spinner verbs updated to "<name>".`

## Pack format

Each file at `~/.claude/plugins/spinner-verbs/<theme>.json`:

```json
{
  "name": "Theme display name",
  "mode": "replace",
  "verbs": [
    "First verb",
    "Second verb",
    "Third verb"
  ]
}
```

- `mode`: `"replace"` (only your verbs) or `"append"` (mixed with Claude's defaults)
- `name`: human-readable label shown in the picker
- `verbs`: present-participle strings — Claude adds the trailing `...`
