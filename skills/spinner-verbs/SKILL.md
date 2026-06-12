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

### 2. Print the full list

Output the numbered list as plain text so the user can see all options at once:

```
Available themes:

1. Chill — Vibing · Taking it easy · No rush
2. Cozy — Brewing something · Warming up · Settling in
3. Dev Humor — Compiling dreams · Blaming the context window · Segfaulting gracefully
...etc
```

Show the pack name and first 3 verbs as a preview for each entry.

### 3. Ask for a selection

Use `AskUserQuestion` with these options:
- **Keep current** — no change
- **Other** (free text) — user types a number or pack name

When the user selects "Other" and types their answer, match it against the list by number (1-based) or by case-insensitive name. If no match, say "No match found." and stop.

### 4. Apply the selected theme

1. Read `~/.claude/settings.json`
2. Set `settings.spinnerVerbs` to `{ "mode": <mode from pack>, "verbs": <verbs from pack> }`
3. Write the updated settings back

Confirm with:
```
Spinner verbs updated to "<name>". Restart your Claude Code session to see the new verbs.

Please contribute your own packs! https://github.com/AlexanderMcIndoe/spinner-verb-skill
```
