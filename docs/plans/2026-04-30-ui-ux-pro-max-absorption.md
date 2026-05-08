# UI/UX Pro Max Skills Absorption — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.

**Goal:** Absorb 7 skills from `nextlevelbuilder/ui-ux-pro-max-skill` into `claude-superskills`, following all project architecture rules, bumping the library from 56 to 63 skills at v1.24.0.

**Architecture:** Clone the source repo to `/tmp/ui-ux-src`, copy each skill's files from `.claude/skills/<name>` into `skills/<name>`. For `ui-ux-pro-max`, resolve the symlinks by copying the real files from `src/ui-ux-pro-max/data/` and `src/ui-ux-pro-max/scripts/` instead of the symlink targets. Fix SKILL.md frontmatter (strip extra fields, fix descriptions), create READMEs and evals for each skill, update metadata, bump version, and push.

**Tech Stack:** Bash, Python (CSV data files), Git. No node/npm needed for this plan.

---

## Source Repo Reference

| Item | Value |
|------|-------|
| Source | `https://github.com/nextlevelbuilder/ui-ux-pro-max-skill` |
| License | MIT |
| Skills location | `.claude/skills/` |
| Data files | `src/ui-ux-pro-max/data/` (CSVs, ~1.4MB) |
| Scripts | `src/ui-ux-pro-max/scripts/` (3 Python files) |
| Stacks | `src/ui-ux-pro-max/data/stacks/` (16 stack CSVs, ~232KB) |

## 7 Skills to Absorb

| Target Name | Source Name | Notes |
|-------------|-------------|-------|
| `ui-ux-pro-max` | `ui-ux-pro-max` | Has data/ + scripts/ via symlinks → resolve from src/ |
| `design` | `ckm:design` | Strip `ckm:` namespace prefix |
| `design-system` | `design-system` | Strip extra frontmatter fields |
| `brand` | `ckm:brand` | Strip `ckm:` prefix + `argument-hint` + `metadata:` block |
| `ui-styling` | `ui-styling` | Strip extra frontmatter fields |
| `slides` | `slides` | Strip extra frontmatter fields |
| `banner-design` | `banner-design` | Strip extra frontmatter fields |

## Version Impact

- **Before:** v1.23.0, 56 skills
- **After:** v1.24.0, 63 skills

---

### Task 1: Clone Source Repository

**Files:**
- Creates: `/tmp/ui-ux-src/` (temporary, not committed)

**Step 1: Clone**

```bash
git clone https://github.com/nextlevelbuilder/ui-ux-pro-max-skill.git /tmp/ui-ux-src
```

Expected: `Cloning into '/tmp/ui-ux-src'...` and success message.

**Step 2: Verify structure**

```bash
ls /tmp/ui-ux-src/.claude/skills/
ls /tmp/ui-ux-src/src/ui-ux-pro-max/
```

Expected:
```
.claude/skills/: banner-design  brand  design  design-system  slides  ui-styling  ui-ux-pro-max
src/ui-ux-pro-max/: data  scripts
```

---

### Task 2: Copy 6 Simple Skills

Skills without special data directories: `design`, `design-system`, `brand`, `ui-styling`, `slides`, `banner-design`.

**Files:**
- Creates: `skills/design/`, `skills/design-system/`, `skills/brand/`, `skills/ui-styling/`, `skills/slides/`, `skills/banner-design/`

**Step 1: Copy each skill directory**

```bash
cd /Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/claude-superskills

for skill in design design-system brand ui-styling slides banner-design; do
  cp -r /tmp/ui-ux-src/.claude/skills/$skill skills/$skill
  echo "Copied: $skill"
done
```

**Step 2: Verify**

```bash
ls skills/design/ skills/brand/ skills/slides/
```

Expected: Each directory has at least `SKILL.md`.

---

### Task 3: Copy `ui-ux-pro-max` (Resolving Symlinks)

The `ui-ux-pro-max` skill has `data/` and `scripts/` as symlinks pointing to `../../src/ui-ux-pro-max/data` and `../../src/ui-ux-pro-max/scripts`. Resolve them to real files.

**Files:**
- Creates: `skills/ui-ux-pro-max/SKILL.md`
- Creates: `skills/ui-ux-pro-max/data/` (15 CSVs + stacks/ subdir)
- Creates: `skills/ui-ux-pro-max/scripts/` (3 Python files: core.py, design_system.py, search.py)

**Step 1: Create skill directory and copy SKILL.md**

```bash
mkdir -p skills/ui-ux-pro-max
cp /tmp/ui-ux-src/.claude/skills/ui-ux-pro-max/SKILL.md skills/ui-ux-pro-max/
```

**Step 2: Copy data directory (real files, not symlinks)**

```bash
cp -rL /tmp/ui-ux-src/src/ui-ux-pro-max/data/ skills/ui-ux-pro-max/data/
```

Note: `-L` flag dereferences symlinks in the source. Exclude `_sync_all.py` (it's a repo maintenance script):

```bash
rm -f skills/ui-ux-pro-max/data/_sync_all.py
```

**Step 3: Copy scripts directory**

```bash
cp -rL /tmp/ui-ux-src/src/ui-ux-pro-max/scripts/ skills/ui-ux-pro-max/scripts/
```

**Step 4: Verify**

```bash
ls skills/ui-ux-pro-max/
ls skills/ui-ux-pro-max/data/ | head -20
ls skills/ui-ux-pro-max/data/stacks/
ls skills/ui-ux-pro-max/scripts/
```

Expected:
```
skills/ui-ux-pro-max/: SKILL.md  data/  scripts/
data/: app-interface.csv charts.csv colors.csv design.csv draft.csv google-fonts.csv icons.csv landing.csv products.csv react-performance.csv stacks/ styles.csv typography.csv ux-guidelines.csv ui-reasoning.csv
stacks/: angular.csv astro.csv flutter.csv html-tailwind.csv jetpack-compose.csv laravel.csv nextjs.csv nuxt-ui.csv nuxtjs.csv react.csv react-native.csv shadcn.csv svelte.csv swiftui.csv threejs.csv vue.csv
scripts/: core.py design_system.py search.py
```

---

### Task 4: Fix SKILL.md Frontmatter — All 7 Skills

**Rules:**
- Keep ONLY: `name`, `description`, `license: MIT`
- Remove: `version`, `author`, `platforms`, `category`, `tags`, `risk`, `created`, `updated`, `argument-hint`, `metadata:` block
- Strip `ckm:` namespace prefix from names (`ckm:design` → `design`, `ckm:brand` → `brand`)
- Description must be single-line, starting with `"This skill should be used when..."`
- No bare date values in frontmatter

**Files:** Modify `SKILL.md` in each of the 7 skill directories.

**Step 1: Fix `skills/design/SKILL.md`**

Current frontmatter starts with `name: ckm:design`. Replace entire frontmatter:

```yaml
---
name: design
description: This skill should be used when the user needs comprehensive design assistance including brand identity, design tokens, UI styling, logo generation (55 styles), corporate identity program (50 deliverables), HTML presentations, banner design (22 styles), icon design (15 styles, SVG), and social photo generation.
license: MIT
---
```

**Step 2: Fix `skills/brand/SKILL.md`**

Current frontmatter has `name: ckm:brand`, `argument-hint:`, and `metadata:` block. Replace entire frontmatter:

```yaml
---
name: brand
description: This skill should be used when the user needs brand identity guidance, voice and messaging frameworks, visual identity standards, asset management, brand consistency review, style guides, color palette management, and typography specifications.
license: MIT
---
```

Also remove `metadata:` block from the body (lines after `---` close) if it leaked out of frontmatter.

**Step 3: Fix `skills/design-system/SKILL.md`**

Replace frontmatter, keeping only `name`, `description`, `license`:

```yaml
---
name: design-system
description: This skill should be used when the user needs to create, maintain, or migrate a design system including design tokens, component libraries, CSS variables, theme management, and cross-platform style consistency.
license: MIT
---
```

**Step 4: Fix `skills/ui-styling/SKILL.md`**

```yaml
---
name: ui-styling
description: This skill should be used when the user needs UI styling guidance including CSS patterns, visual design rules, component styling, responsive design, dark mode, and design-to-code translation for web and mobile interfaces.
license: MIT
---
```

**Step 5: Fix `skills/slides/SKILL.md`**

```yaml
---
name: slides
description: This skill should be used when the user needs to create presentation slides, HTML-based slideshows, or slide deck content with structured layouts, data visualizations, and speaker notes.
license: MIT
---
```

**Step 6: Fix `skills/banner-design/SKILL.md`**

```yaml
---
name: banner-design
description: This skill should be used when the user needs to design banners in any of 22 styles for social media, advertising, web, or print formats including HTML/CSS output and Gemini AI image generation.
license: MIT
---
```

**Step 7: Fix `skills/ui-ux-pro-max/SKILL.md`**

The current SKILL.md has only `name: ui-ux-pro-max` in frontmatter (minimal). Add description and license:

```yaml
---
name: ui-ux-pro-max
description: This skill should be used when the user needs comprehensive UI/UX design intelligence for web and mobile projects, including 50+ styles, 161 color palettes, 57 font pairings, UX guidelines, and stack-specific best practices across React, Next.js, Vue, Svelte, Angular, Astro, Flutter, SwiftUI, React Native, and HTML/Tailwind.
license: MIT
---
```

**Step 8: Validate all frontmatter**

```bash
cd /Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/claude-superskills

for skill in ui-ux-pro-max design design-system brand ui-styling slides banner-design; do
  echo "=== $skill ==="
  head -6 skills/$skill/SKILL.md
done
```

Expected: Each shows only `---`, `name:`, `description:`, `license: MIT`, `---` with no extra fields.

---

### Task 5: Validate YAML with Script

**Step 1: Run validation for all 7 skills**

```bash
for skill in ui-ux-pro-max design design-system brand ui-styling slides banner-design; do
  ./scripts/validate-skill-yaml.sh skills/$skill
done
```

Expected: `✅ YAML frontmatter valid!` for each. Warnings about word count (>5000) for `ui-ux-pro-max` and `design` are acceptable.

---

### Task 6: Create README.md for Each Skill

Create `README.md` in each skill directory with Metadata table. Dates follow Metadata table convention (NOT in SKILL.md frontmatter).

**Files:**
- Creates: `skills/{skill-name}/README.md` × 7

**Template to apply for each skill:**

```markdown
# {Skill Display Name}

{One-paragraph description of what the skill does and key capabilities.}

## Installation

```bash
npx claude-superskills install {skill-name}
```

## Usage

Activate with any of these phrases:
- {trigger phrase 1}
- {trigger phrase 2}
- {trigger phrase 3}

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | nextlevelbuilder |
| Created | 2026-04-30 |
| Updated | 2026-04-30 |
| Platforms | All (GitHub Copilot, Claude Code, Codex, OpenCode, Gemini CLI, Antigravity, Cursor, AdaL) |
| Category | {category} |
| Tags | {tag1}, {tag2}, {tag3} |
| Risk | safe |
```

**Step 1: Create README for `ui-ux-pro-max`** — Category: design, Tags: ui-ux, design-system, web, mobile
**Step 2: Create README for `design`** — Category: design, Tags: logo, branding, identity, banner
**Step 3: Create README for `design-system`** — Category: design, Tags: tokens, components, theming
**Step 4: Create README for `brand`** — Category: design, Tags: brand, voice, identity, messaging
**Step 5: Create README for `ui-styling`** — Category: design, Tags: css, styling, components, responsive
**Step 6: Create README for `slides`** — Category: content, Tags: slides, presentation, html
**Step 7: Create README for `banner-design`** — Category: design, Tags: banner, ads, social-media, svg

---

### Task 7: Create Evals for Each Skill

**Pattern:** `evals/evals.json` (5 test scenarios) + `evals/trigger-eval.json` (15 trigger queries).

**Files:**
- Creates: `skills/{skill-name}/evals/evals.json` × 7
- Creates: `skills/{skill-name}/evals/trigger-eval.json` × 7

**`evals.json` structure (use `ava-pptx` as reference):**

```json
[
  {
    "scenario": "...",
    "input": "...",
    "assertions": [
      { "text": "...", "weight": 0.3 },
      { "text": "...", "weight": 0.2 }
    ]
  }
]
```

**`trigger-eval.json` structure:**

```json
[
  { "id": "t1", "query": "...", "should_trigger": true },
  { "id": "t2", "query": "...", "should_trigger": false }
]
```

Create evals for each skill with relevant scenarios:
- `ui-ux-pro-max`: design system generation, style selection, color palette, stack best practices, accessibility review
- `design`: logo generation, brand identity, CIP creation, banner design, icon design
- `design-system`: token creation, component theming, CSS variables, dark mode, migration
- `brand`: brand voice, style guide, asset management, tone of voice, brand audit
- `ui-styling`: CSS patterns, responsive design, dark mode, animation, component styling
- `slides`: HTML slideshow, presentation creation, data visualization slides, speaker notes
- `banner-design`: social media banner, ad banner, web banner, print banner, style selection

---

### Task 8: Update Repository Metadata (56 → 63)

**Files to modify:**

| File | Change |
|------|--------|
| `README.md` | Badge `56` → `63`; add UI/UX category table with 7 new skills |
| `CLAUDE.md` | Version 1.23.0 → 1.24.0; count 56 → 63; add 7 skills to tree; update bundles |
| `CHANGELOG.md` | Add `[1.24.0]` entry |
| `bundles.json` | Add new `ui-ux` bundle; add all 7 skills to `all` bundle |
| `cli-installer/README.md` | `56` → `63` |
| `.claude-plugin/marketplace.json` | Update description with new count |

**Step 1: Update `bundles.json` — add `ui-ux` bundle**

```json
"ui-ux": {
  "name": "UI/UX Design Intelligence",
  "description": "Complete UI/UX design system with 50+ styles, 161 color palettes, brand identity, design tokens, CSS styling, slides, and banner design",
  "skills": ["ui-ux-pro-max", "design", "design-system", "brand", "ui-styling", "slides", "banner-design"],
  "use_cases": [
    "Design complete web/mobile UI with proper styles and color palettes",
    "Generate brand identity, logos, and corporate identity programs",
    "Create design tokens and component libraries",
    "Design social media banners, ads, and web banners",
    "Build HTML presentation slides"
  ],
  "target": "Designers, frontend developers, and product teams"
}
```

Also add all 7 skills to the `all` bundle's `skills` array.

**Step 2: Update `README.md`**

- Change `skills-56` badge → `skills-63`
- Add new table row / section for UI/UX skills in the catalog
- Update footer version: `v1.23.0` → `v1.24.0`

**Step 3: Update `CLAUDE.md`**

- `v1.23.0` → `v1.24.0` in Project Overview
- `56 skills` → `63 skills`
- Add 7 skill entries to the `skills/` tree in Repository Architecture
- Add `ui-ux` to bundles section
- Add `UI/UX Design` to Skill Types section

**Step 4: Update `CHANGELOG.md`**

```markdown
## [1.24.0] - 2026-04-30

### Added
- 7 UI/UX design intelligence skills absorbed from `nextlevelbuilder/ui-ux-pro-max-skill` (MIT):
  - `ui-ux-pro-max`: comprehensive design intelligence (50+ styles, 161 palettes, 10 stacks)
  - `design`: brand identity, logos, CIP, banners, icons (55 styles, Gemini AI)
  - `design-system`: design tokens, component libraries, theming
  - `brand`: brand voice, messaging, style guides, asset management
  - `ui-styling`: CSS patterns, responsive design, dark mode
  - `slides`: HTML presentation generation
  - `banner-design`: 22-style banner creation for social/ads/web/print
- New bundle `ui-ux` with all 7 design skills
```

**Step 5: Update `cli-installer/README.md`** — replace `56` with `63` in skill count references.

**Step 6: Update `.claude-plugin/marketplace.json`** — update description to reference 63 skills.

---

### Task 9: Bump Version to 1.24.0

**Files:**
- Modify: `cli-installer/package.json` (version field)
- Modify: `cli-installer/package-lock.json` (version field)
- Modify: `.claude-plugin/plugin.json` (version field — NOT auto-updated by script)

**Step 1: Run bump script**

```bash
cd /Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/claude-superskills
./scripts/bump-version.sh minor
```

Expected output: `Bumped version to 1.24.0` and updated `cli-installer/package.json`.

**Step 2: Manually update `.claude-plugin/plugin.json`**

```bash
# Verify script did NOT update plugin.json
grep '"version"' .claude-plugin/plugin.json
```

If still shows `1.23.0`, update it:

```bash
sed -i '' 's/"version": "1.23.0"/"version": "1.24.0"/' .claude-plugin/plugin.json
grep '"version"' .claude-plugin/plugin.json
```

Expected: `"version": "1.24.0"`.

**Step 3: Verify version sync**

```bash
./scripts/verify-version-sync.sh
```

Expected: All version files show `1.24.0` with no mismatches.

---

### Task 10: Commit and Push

**Step 1: Stage all new and modified files**

```bash
cd /Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/claude-superskills

git add skills/ui-ux-pro-max/ skills/design/ skills/design-system/ skills/brand/ \
        skills/ui-styling/ skills/slides/ skills/banner-design/ \
        README.md CLAUDE.md CHANGELOG.md bundles.json \
        cli-installer/package.json cli-installer/package-lock.json \
        .claude-plugin/plugin.json cli-installer/README.md \
        .claude-plugin/marketplace.json \
        docs/plans/2026-04-30-ui-ux-pro-max-absorption.md
```

**Step 2: Verify staged files**

```bash
git status --short
```

Expected: All 7 skill directories + metadata files shown as `A` (new) or `M` (modified).

**Step 3: Commit**

```bash
git commit -m "feat: add 7 ui-ux-pro-max skills v1.0.0 and bump to v1.24.0

- Absorb ui-ux-pro-max, design, design-system, brand, ui-styling, slides, banner-design
- Source: nextlevelbuilder/ui-ux-pro-max-skill (MIT license)
- Includes data/ (1.4MB CSVs) and scripts/ for ui-ux-pro-max
- Add ui-ux bundle in bundles.json
- 56 → 63 skills

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

**Step 4: Create version tag**

```bash
git tag v1.24.0
```

**Step 5: Push commit and tag (triggers npm publish via CI)**

```bash
git push origin main && git push origin v1.24.0
```

Expected: CI/CD workflow triggers and publishes `claude-superskills@1.24.0` to npm automatically.

**Step 6: Verify tag was pushed**

```bash
git log --oneline -3
git tag | tail -3
```

Expected: `v1.24.0` appears in tag list.

---

## Validation Checklist (run before commit)

```bash
# 1. Validate all 7 skill YAML frontmatter
for skill in ui-ux-pro-max design design-system brand ui-styling slides banner-design; do
  ./scripts/validate-skill-yaml.sh skills/$skill
done

# 2. Validate skill content (warnings for >5000 words are acceptable)
for skill in ui-ux-pro-max design design-system brand ui-styling slides banner-design; do
  ./scripts/validate-skill-content.sh skills/$skill
done

# 3. Verify version sync across all files
./scripts/verify-version-sync.sh

# 4. Confirm skill count
ls skills/ | wc -l
# Expected: 63
```

## Cleanup

```bash
# Remove temp clone after commit
rm -rf /tmp/ui-ux-src
```
