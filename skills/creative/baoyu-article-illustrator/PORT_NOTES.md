# Port Notes — baoyu-article-illustrator

Ported from [JimLiu/baoyu-skills](https://github.com/JimLiu/baoyu-skills) v1.57.0.

## Changes from upstream

`SKILL.md`, `references/workflow.md`, and `references/usage.md` were modified. All other reference files are verbatim copies.

### SKILL.md adaptations

| Change | Upstream | Hermes |
|--------|----------|--------|
| Metadata namespace | `openclaw` | `hermes` |
| Trigger | Natural language description with slash command examples | Natural language skill matching only |
| User config (EXTEND.md) | Steps 1.1–1.5: project/user/XDG config, first-time setup | Removed — not part of Hermes infra |
| User prompts | `AskUserQuestion` (batched, fallback-aware) | `clarify` tool (one question at a time) |
| Image generation | `baoyu-imagine`, `imagegen`, `image_generate` (any available) | `image_generate` tool (primary) |
| Platform support | Linux/macOS/Windows/WSL/PowerShell | Linux/macOS only |
| File operations | Bash commands for file checks | Hermes file tools (write_file, read_file) |
| Output directory default | Configured via EXTEND.md first-time setup | `imgs-subdir` default; ask via `clarify` if needed |

### workflow.md adaptations

- Removed Steps 1.2–1.5 (EXTEND.md loading, preference checks, first-time setup blocking)
- Simplified Step 1.1 to direct input-type determination with `clarify` for output directory
- Replaced all `AskUserQuestion` calls with `clarify` (one question per call)
- Removed PowerShell/Windows command blocks
- Removed EXTEND.md preference lookups in Step 3 (Q3 style selection)
- Step 5.2 updated to reference `image_generate` directly

### usage.md adaptations

- Removed slash command syntax (`/baoyu-article-illustrator`)
- Rewrote examples as natural language descriptions

### What was preserved

- Three Dimensions model (Type × Style × Palette)
- All 6 types and type descriptions
- Full style gallery reference
- Workflow structure (Steps 1–6) and all step logic
- Outline format and prompt-file-first requirement
- Reference image handling (Step 1.0, Step 5.3)
- Output directory options table
- Modification table (Edit / Add / Delete)
- All 23 style definition files
- All 4 palette definition files
- `references/prompt-construction.md`
- `references/style-presets.md`
- `references/styles.md`
- `prompts/system.md`
- Author, version, homepage attribution

## Syncing with upstream

To pull upstream updates:
```bash
# Compare versions
curl -sL https://raw.githubusercontent.com/JimLiu/baoyu-skills/main/skills/baoyu-article-illustrator/SKILL.md | head -5
# Look for version: line

# Diff reference files
diff <(curl -sL https://raw.githubusercontent.com/JimLiu/baoyu-skills/main/skills/baoyu-article-illustrator/references/styles/blueprint.md) references/styles/blueprint.md
```

Reference files (styles, palettes, prompt-construction, style-presets, styles.md) can be overwritten directly — they are unchanged from upstream. `SKILL.md`, `references/workflow.md`, and `references/usage.md` must be manually merged since they contain Hermes-specific adaptations.
