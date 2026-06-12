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
| **Chill** | Vibing · Taking it easy · Going with the flow |
| **Cozy** | Brewing something · Settling in · Knitting thoughts together |
| **Dev Humor** | Compiling dreams · Segfaulting gracefully · Hallucinating confidently |
| **Existential** | Staring into the void · Dissolving into tokens · Being and nothingness |
| **Grind Mode** | Locking in · Shipping · Not stopping |
| **Startup Brain** | Disrupting · Pivoting · Finding product-market fit |

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
