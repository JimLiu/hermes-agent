# Detailed Workflow Procedures

## Step 1: Pre-check

### 1.0 Detect & Save Reference Images ⚠️ REQUIRED if images provided

Check if user provided reference images. Handle based on input type:

| Input Type | Action |
|------------|--------|
| Image file path provided | Copy to `references/` subdirectory → can use as `--ref` |
| Image in conversation (no path) | **ASK user for file path** with `clarify` |
| User can't provide path | Extract style/palette verbally → append to prompts (NO frontmatter references) |

**CRITICAL**: Only add `references` to prompt frontmatter if files are ACTUALLY SAVED to `references/` directory.

**If user provides file path**:
1. Copy to `references/NN-ref-{slug}.png`
2. Create description: `references/NN-ref-{slug}.md`
3. Verify files exist before proceeding

**If user can't provide path** (extracted verbally):
1. Analyze image visually, extract: colors, style, composition
2. Create `references/extracted-style.md` with extracted info
3. DO NOT add `references` to prompt frontmatter
4. Instead, append extracted style/colors directly to prompt text

**Description File Format** (only when file saved):
```yaml
---
ref_id: NN
filename: NN-ref-{slug}.png
---
[User's description or auto-generated description]
```

**Verification** (only for saved files):
```
Reference Images Saved:
- 01-ref-{slug}.png ✓ (can use --ref)
- 02-ref-{slug}.png ✓ (can use --ref)
```

**Or for extracted style**:
```
Reference Style Extracted (no file):
- Colors: #E8756D coral, #7ECFC0 mint...
- Style: minimal flat vector, clean lines...
→ Will append to prompt text (not --ref)
```

---

### 1.1 Determine Input Type

| Input | Output Directory | Next |
|-------|-----------------|------|
| File path | `{article-dir}/imgs/` (default) — use `clarify` to ask if a different location is preferred | → continue |
| Pasted content | `illustrations/{topic-slug}/` | → continue |

**Backup rule for pasted content**: If `source.md` exists in target directory, rename to `source-backup-YYYYMMDD-HHMMSS.md` before saving.

**Output directory options** (ask via `clarify` if needed):

| Value | Path |
|-------|------|
| `imgs-subdir` (default) | `{article-dir}/imgs/` |
| `same-dir` | `{article-dir}/` |
| `illustrations-subdir` | `{article-dir}/illustrations/` |
| `independent` | `illustrations/{topic-slug}/` |

---

## Step 2: Setup & Analyze

### 2.1 Analyze Content

| Analysis | Description |
|----------|-------------|
| Content type | Technical / Tutorial / Methodology / Narrative |
| Illustration purpose | information / visualization / imagination |
| Core arguments | 2-5 main points to visualize |
| Visual opportunities | Positions where illustrations add value |
| Recommended type | Based on content signals and purpose |
| Recommended density | Based on length and complexity |

### 2.2 Extract Core Arguments

- Main thesis
- Key concepts reader needs
- Comparisons/contrasts
- Framework/model proposed

**CRITICAL**: If article uses metaphors (e.g., "电锯切西瓜"), do NOT illustrate literally. Visualize the **underlying concept**.

### 2.3 Identify Positions

**Illustrate**:
- Core arguments (REQUIRED)
- Abstract concepts
- Data comparisons
- Processes, workflows

**Do NOT Illustrate**:
- Metaphors literally
- Decorative scenes
- Generic illustrations

### 2.4 Analyze Reference Images (if provided in Step 1.0)

For each reference image:

| Analysis | Description |
|----------|-------------|
| Visual characteristics | Style, colors, composition |
| Content/subject | What the reference depicts |
| Suitable positions | Which sections match this reference |
| Style match | Which illustration types/styles align |
| Usage recommendation | `direct` / `style` / `palette` |

| Usage | When to Use |
|-------|-------------|
| `direct` | Reference matches desired output closely |
| `style` | Extract visual style characteristics only |
| `palette` | Extract color scheme only |

---

## Step 3: Confirm Settings ⚠️

**Do NOT skip.** Use the `clarify` tool, one question per call. Ask Q1 first, then Q2, etc.

### Q1: Preset or Type ⚠️ REQUIRED

Based on Step 2 content analysis, recommend a preset first (sets both type & style). Look up [style-presets.md](style-presets.md) "Content Type → Preset Recommendations" table.

- [Recommended preset] — [brief: type + style + why] (Recommended)
- [Alternative preset] — [brief]
- Or choose type manually: infographic / scene / flowchart / comparison / framework / timeline / mixed

**If user picks a preset → skip Q3** (type & style both resolved).
**If user picks a type → Q3 is REQUIRED.**

### Q2: Density ⚠️ REQUIRED - DO NOT SKIP
- minimal (1-2) - Core concepts only
- balanced (3-5) - Major sections
- per-section - At least 1 per section/chapter (Recommended)
- rich (6+) - Comprehensive coverage

### Q3: Style ⚠️ REQUIRED (skip if preset chosen in Q1)

Present Core Styles first:
- [Best compatible core style] (Recommended)
- [Other compatible core style 1]
- [Other compatible core style 2]
- Other (see full Style Gallery)

**Core Styles** (simplified selection):

| Core Style | Maps To | Best For |
|------------|---------|----------|
| `minimal-flat` | notion | General, knowledge sharing, SaaS |
| `sci-fi` | blueprint | AI, frontier tech, system design |
| `hand-drawn` | sketch/warm | Relaxed, reflective, casual |
| `editorial` | editorial | Processes, data, journalism |
| `scene` | warm/watercolor | Narratives, emotional, lifestyle |
| `poster` | screen-print | Opinion, editorial, cultural, cinematic |

Style selection based on Type × Style compatibility matrix (styles.md).
**In Step 5.1**, read `styles/<style>.md` for visual elements and rendering rules.

### Q4: Palette (optional)

Offer available palettes if the user may benefit from a palette override:

- Default (use style's built-in colors) (Recommended)
- `macaron` — soft pastel blocks on warm cream
- `warm` — warm earth tones, no cool colors
- `neon` — vibrant neon on dark backgrounds

See Palette Gallery in [styles.md](styles.md#palette-gallery) and full specs in `palettes/<palette>.md`.

### Q5: Image Text Language ⚠️ REQUIRED when article language ≠ expected output language

Detect article language from content. If different from expected output language, MUST ask:
- Article language (match article content) (Recommended)
- User's general preference language

### Display Reference Usage (if references detected in Step 1.0)

When presenting outline preview to user, show reference assignments:

```
Reference Images:
| Ref | Filename | Recommended Usage |
|-----|----------|-------------------|
| 01 | 01-ref-diagram.png | direct → Illustration 1, 3 |
| 02 | 02-ref-chart.png | palette → Illustration 2 |
```

---

## Step 4: Generate Outline

Save as `{output-dir}/outline.md` (all paths below are relative to the output directory determined in Step 1.1):

```yaml
---
type: infographic
density: balanced
style: blueprint
image_count: 4
references:                    # Only if references provided
  - ref_id: 01
    filename: 01-ref-diagram.png
    description: "Technical diagram showing system architecture"
  - ref_id: 02
    filename: 02-ref-chart.png
    description: "Color chart with brand palette"
---

## Illustration 1

**Position**: [section] / [paragraph]
**Purpose**: [why this helps]
**Visual Content**: [what to show]
**Type Application**: [how type applies]
**References**: [01]                    # Optional: list ref_ids used
**Reference Usage**: direct             # direct | style | palette
**Filename**: 01-infographic-concept-name.png

## Illustration 2
...
```

**Requirements**:
- Each position justified by content needs
- Type applied consistently
- Style reflected in descriptions
- Count matches density
- References assigned based on Step 2.4 analysis

---

## Step 5: Generate Images

### 5.1 Create Prompts ⛔ BLOCKING

**Every illustration MUST have a saved prompt file before generation begins. DO NOT skip this step.**

For each illustration in the outline:

1. **Create prompt file**: `{output-dir}/prompts/NN-{type}-{slug}.md`
2. **Include YAML frontmatter**:
   ```yaml
   ---
   illustration_id: 01
   type: infographic
   style: custom-flat-vector
   ---
   ```
3. **Load style specs**: Read `styles/<style>.md` for visual elements, style rules, and rendering instructions
4. **Load palette specs** (if palette specified): Read `palettes/<palette>.md` for colors and background. Palette colors **replace** the style's default Color Palette. If no palette specified, use the style's built-in colors.
5. **Follow type-specific template** from [prompt-construction.md](prompt-construction.md), using rendering from style + colors from palette (or style default)
6. **Prompt quality requirements** (all REQUIRED):
   - `Layout`: Describe overall composition (grid / radial / hierarchical / left-right / top-down)
   - `ZONES`: Describe each visual area with specific content, not vague descriptions
   - `LABELS`: Use **actual numbers, terms, metrics, quotes from the article** — NOT generic placeholders
   - `COLORS`: Specify hex codes from palette (or style default) with semantic meaning
   - `STYLE`: Describe line treatment, texture, mood, character rendering per style rules
   - `ASPECT`: Specify ratio (e.g., `16:9`)
7. **Apply defaults**: composition requirements, character rendering, text guidelines
8. **Backup rule**: If prompt file exists, rename to `prompts/NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.md`

**Verification** ⛔: Before proceeding to 5.2, confirm ALL prompt files exist:
```
Prompt Files:
- prompts/01-infographic-overview.md ✓
- prompts/02-infographic-distillation.md ✓
...
```

**DO NOT** pass ad-hoc inline text to `image_generate` without first saving prompt files.

**Execution choice**:
- If multiple illustrations already have saved prompt files and the task is now plain generation, generate sequentially
- Use subagents only when each illustration still needs separate prompt rewriting, style exploration, or other per-image reasoning before generation

**CRITICAL - References in Frontmatter**:
- Only add `references` field if files ACTUALLY EXIST in `references/` directory
- If style/palette was extracted verbally (no file), append info to prompt BODY instead
- Before writing frontmatter, verify the file exists using `read_file` or checking the path

### 5.2 Generate Images

Use `image_generate` with the content of each saved prompt file.

- Map aspect ratio to image_generate's format: `16:9` → `landscape`, `9:16` → `portrait`, `1:1` → `square`
- For custom ratios, pick the closest named aspect

### 5.3 Process References ⚠️ REQUIRED if references saved in Step 1.0

**DO NOT SKIP if user provided reference images.** For each illustration with references:

1. **VERIFY files exist first** using `read_file` — if file MISSING but in frontmatter → ERROR, fix frontmatter or remove references field
2. Read prompt frontmatter for reference info
3. Process based on usage type:

| Usage | Action | Example |
|-------|--------|---------|
| `direct` | Pass reference path to `image_generate` | via `references` parameter if supported |
| `style` | Analyze reference, append style traits to prompt | "Style: clean lines, gradient backgrounds..." |
| `palette` | Extract colors from reference, append to prompt | "Colors: #E8756D coral, #7ECFC0 mint..." |

**Verification**: Before generating, confirm reference processing:
```
Reference Processing:
- Illustration 1: using 01-ref-brand.png (direct) ✓
- Illustration 2: extracted palette from 02-ref-style.png ✓
```

### 5.4 Generate

1. For each illustration:
   - **Backup rule**: If image file exists, rename to `NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.png`
   - Generate image using `image_generate`
2. After each: "Generated X/N"
3. On failure: retry once, then log and continue

---

## Step 6: Finalize

### 6.1 Update Article

Insert after corresponding paragraph, using path relative to article file:

| Output Directory | Insert Path |
|-----------------|-------------|
| `imgs-subdir` | `![description](imgs/NN-{type}-{slug}.png)` |
| `same-dir` | `![description](NN-{type}-{slug}.png)` |
| `illustrations-subdir` | `![description](illustrations/NN-{type}-{slug}.png)` |
| `independent` | `![description](illustrations/{topic-slug}/NN-{type}-{slug}.png)` (relative to cwd) |

Alt text: concise description in article's language.

### 6.2 Output Summary

```
Article Illustration Complete!

Article: [path]
Type: [type] | Density: [level] | Style: [style]
Location: [directory]
Images: X/N generated

Positions:
- 01-xxx.png → After "[Section]"
- 02-yyy.png → After "[Section]"

[If failures]
Failed:
- NN-zzz.png: [reason]
```
