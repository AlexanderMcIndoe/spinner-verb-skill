# spinner-verbs

A Claude Code plugin that lets you interactively pick themed spinner verb packs — the verbs Claude shows while it's thinking.

## Install

```
/plugin install spinner-verbs
```

Then copy some packs from this repo's `packs/` folder into `~/.claude/plugins/spinner-verbs/`:

```bash
mkdir -p ~/.claude/plugins/spinner-verbs
cp packs/*.json ~/.claude/plugins/spinner-verbs/
```

## Usage

```
/spinner-verbs
```

Browse the available themes, pick one, and it's applied instantly to `~/.claude/settings.json`.

## Included packs

| Pack | Preview |
|---|---|
| **Apologetic** | Trying my best · Sorry, hurrying · Almost there, I promise |
| **Bro** | Brooooooooooooo · Brooo · Broooooooooooooooo |
| **Chill** | Vibing · Taking it easy · No rush |
| **Cozy** | Brewing something good · Warming up · Settling in |
| **Scary Dev Humor** | Deleting git history · Wiping the codebase · Deploying to prod |
| **Existential** | Questioning everything · Staring into the void · Contemplating my weights |
| **Dry & Sarcastic** | Hallucinating · Pretending to work · Pretending to think |
| **Simple** | Thinking |

## Add your own

Drop any `.json` file into `~/.claude/plugins/spinner-verbs/`:

```json
{
  "name": "My Theme",
  "mode": "replace",
  "verbs": [
    "Cooking something up",
    "Almost there",
    "Worth the wait"
  ]
}
```

Set `"mode"` to `"append"` to mix your verbs with Claude's built-in defaults.

## Contributing packs

PRs welcome — add a new `.json` file to `packs/` and open a pull request.

## License

MIT
