# Usage

## How to Trigger

Describe what you want in natural language:

- "Illustrate this article: path/to/article.md"
- "Add images to my article, use a blueprint style"
- "Generate illustrations for this article with infographic type"
- "为文章配图" (paste content or provide file path)

## Options

| Option | Description |
|--------|-------------|
| `--type <name>` | Illustration type (see Type Gallery in SKILL.md) |
| `--style <name>` | Visual style (see references/styles.md) |
| `--preset <name>` | Shorthand for type + style combo (see [references/style-presets.md](references/style-presets.md)) |
| `--density <level>` | Image count: minimal / balanced / per-section / rich |

## Input Modes

| Mode | Trigger | Output Directory |
|------|---------|-----------------|
| File path | Provide path to article file | `{article-dir}/imgs/` (default) |
| Paste content | Paste article text directly | `illustrations/{topic-slug}/` |

## Output Directory Options

| Value | Path |
|-------|------|
| `imgs-subdir` (default) | `{article-dir}/imgs/` |
| `same-dir` | `{article-dir}/` |
| `illustrations-subdir` | `{article-dir}/illustrations/` |
| `independent` | `illustrations/{topic-slug}/` |

## Examples

**Technical article with data**:
> Illustrate api-design.md with infographic type and blueprint style

**Same thing with preset**:
> Illustrate api-design.md using the tech-explainer preset

**Personal story**:
> Add illustrations to journey.md using the storytelling preset

**Tutorial with steps**:
> Generate rich illustrations for how-to-deploy.md, tutorial preset

**Opinion article with poster style**:
> Illustrate opinion.md using opinion-piece preset

**Preset with style override**:
> Illustrate article.md with tech-explainer preset but notion style

**Paste mode**:
> 为这篇文章配图:
> [paste article content]
