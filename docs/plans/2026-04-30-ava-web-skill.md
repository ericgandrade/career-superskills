# ava-web Skill — Creation Plan v1.25.0

## Goal

Create the `ava-web` skill to generate websites, landing pages, and web components following Avanade visual identity.
The skill uses the same color/typography system as `ava-pptx` (brand-guidelines.md), adapted for the web.
Supports plain HTML, HTML+Tailwind, React+Tailwind, and Next.js.

---

## Reference Analysis

### Avanade Brand (extracted from ava-pptx/references/brand-guidelines.md)

| Token | Value | Web usage |
|-------|-------|-----------|
| Primary Orange | `#FF5800` | Buttons, active links, accents, card borders |
| Dark Orange | `#DC4600` | Hover states, Pantone 159 |
| Yellow Accent | `#FFD700` | Wave accent line, decorative gradient |
| Gradient Start | `#FF5800` | Hero backgrounds |
| Gradient Mid | `#B43C14` | Hero backgrounds |
| Gradient End | `#870032` | Hero backgrounds |
| Body Text | `#333333` | Paragraphs, headings on light backgrounds |
| Secondary Text | `#666666` | Subtitles, captions |
| Tertiary Text | `#999999` | Labels, footers |
| Background Tint | `#FFF0E8` | Callout boxes, soft highlight sections |
| White | `#FFFFFF` | Text on gradient, primary backgrounds |
| Font | Segoe UI | Web: `'Segoe UI', system-ui, -apple-system, sans-serif` |
| Tagline | "Do what matters" | Footer and nav |
| Wave | Organic SVG | Section decoration (reused from brand-guidelines.md) |

### Visual patterns observed on avanade.com

- **Hero**: full-width orange→purple gradient, white text, SVG wave at the bottom
- **Nav**: logo left, links center/right, orange CTA button
- **Content sections**: white background with orange accents, 2–3 column grid
- **Stats bar**: large numbers in orange (e.g. "50,000+ employees")
- **Cards**: soft shadow, orange top border or orange icon
- **CTA sections**: gradient or solid orange background, white text
- **Footer**: dark with white links and white logo
- **Dividers**: SVG wave between sections (brand signature element)

---

## Architecture

```
skills/ava-web/
├── SKILL.md                          # Main skill spec
├── README.md                         # User documentation
├── references/
│   ├── web-brand-guidelines.md       # CSS vars, typography, spacing, components
│   ├── components.md                 # Component catalog with HTML+Tailwind+React code
│   └── tailwind-config.md            # Tailwind config with Avanade tokens
└── evals/
    ├── evals.json                    # 5 test scenarios
    └── trigger-eval.json             # 15 trigger/no-trigger queries
```

---

## Tech Stack

- **Plain HTML**: semantic tags + CSS custom properties + inline SVG
- **HTML + Tailwind**: Tailwind config with Avanade tokens + utility classes
- **React + Tailwind**: JSX components, same Tailwind config
- **Next.js**: Next.js App Router page structure, Tailwind, reusable components

The skill detects the project stack or asks the user.

---

## Components / Sections to Document

| Component | Variants |
|-----------|----------|
| **Navbar** | Light (white + orange), Dark (solid orange), Sticky |
| **Hero** | Gradient full-width + wave, Image bg overlay, Solid orange |
| **Features Grid** | 2 cols, 3 cols, 4 cols — cards with orange icon |
| **Stats Bar** | Large orange numbers, light or gradient background |
| **Content + Image** | Image left, Image right, alternating |
| **Callout / Quote** | Orange border, FFF0E8 background, pull-quote |
| **CTA Section** | Gradient (like PPTX cover), solid orange |
| **Testimonials** | White card, orange border, avatar |
| **Footer** | Dark, white logo, links, tagline "Do what matters" |
| **Wave Divider** | Reusable SVG wave between sections |
| **Button** | Primary (orange), Secondary (orange outline), Ghost |
| **Card** | Default shadow, orange top-border, gradient header |

---

## Supported Page Types

| Type | Description |
|------|-------------|
| **Landing Page** | Hero + Features + Stats + CTA + Footer |
| **Service Page** | Hero + Content-Image alternating + Features + CTA |
| **About Page** | Hero + Stats + Two-col + Team grid + CTA |
| **Case Study** | Hero + Challenge/Solution/Result + Stats + Quote + CTA |
| **Contact Page** | Short hero + Form + Map/Office info + Footer |

---

## Tasks

### Task 1: Create `references/web-brand-guidelines.md`

Complete document with:
- CSS custom properties (`:root { --ava-orange: #FF5800; ... }`)
- Full Tailwind config with Avanade tokens
- Web typography scale (px/rem equivalents of PPTX dimensions)
- Spacing system
- Wave SVGs (reused from brand-guidelines.md, adapted for HTML)
- Dark/light section guidance (when to use gradient vs white)
- Accessibility rules (minimum contrast 4.5:1 — orange on white FAILS, use only in large text or on dark backgrounds)

**Critical accessibility note**: `#FF5800` on `#FFFFFF` has contrast **2.8:1** (WCAG AA fail for normal text). Rule: orange only for text ≥24px bold (large text — ratio OK ≥3:1) or as decorative/CTA element. Reading text always in `#333333`.

### Task 2: Create `references/components.md`

Catalog of all 12 components with:
- Complete semantic HTML
- Tailwind version with utility classes
- React version (JSX + props)
- Usage notes and variants

### Task 3: Create `references/tailwind-config.md`

```js
// tailwind.config.js — Avanade brand tokens
module.exports = {
  theme: {
    extend: {
      colors: {
        ava: {
          orange:       '#FF5800',
          'orange-dark':'#DC4600',
          yellow:       '#FFD700',
          flame:        '#B43C14',
          purple:       '#870032',
          'gray-dark':  '#333333',
          'gray-mid':   '#666666',
          'gray-light': '#999999',
          tint:         '#FFF0E8',
        }
      },
      fontFamily: {
        ava: ["'Segoe UI'", 'system-ui', '-apple-system', 'sans-serif'],
      },
      backgroundImage: {
        'ava-gradient': 'linear-gradient(135deg, #FF5800 0%, #B43C14 55%, #870032 100%)',
        'ava-wave':     'linear-gradient(90deg, #FF5800 0%, #DC4600 60%, #B43C14 100%)',
      }
    }
  }
}
```

### Task 4: Create `SKILL.md`

Full workflow:
- **Step 0: Discovery** — detect stack (HTML/Tailwind/React/Next.js), page type, content
- **Step 1: Load brand** — `Read references/web-brand-guidelines.md` before any code
- **Step 2: Select components** — map required sections to the requested page type
- **Step 3: Generate** — ready-to-use code, commented by section
- **Step 4: QA** — brand compliance checklist before delivering

Minimal frontmatter (name, description, license only):
```yaml
---
name: ava-web
description: This skill should be used when the user needs to create a website, landing page, web component, or web UI following official Ava brand guidelines. Trigger for "create a landing page", "build a website", "design a web page", "create a hero section", "Avanade-branded website", "create a React component Avanade style", or any request to generate HTML/CSS/React/Next.js code in Avanade visual identity.
license: MIT
---
```

### Task 5: Create `README.md`

With Metadata table (Version 1.0.0, Author Eric Andrade, Category design, Tags ui, web, avanade, branding, tailwind, react).

### Task 6: Create evals

`evals/evals.json` — 5 scenarios:
1. Landing page for an AI service
2. Hero section React+Tailwind
3. Case Study page
4. Stats bar component
5. Footer with tagline "Do what matters"

`evals/trigger-eval.json` — 15 queries (10 true, 5 false such as Python scripts, audio, Excel).

### Task 7: Update repository metadata

- `README.md`: 63→64 skills, add `ava-web` to the UI/UX Design table
- `CLAUDE.md`: count + skills tree + bundles
- `CHANGELOG.md`: v1.25.0 entry
- `bundles.json`: add `ava-web` to the `content` bundle (branded content) and to `all`
- `cli-installer/README.md`: 63→64
- `.claude-plugin/marketplace.json`: 63→64

### Task 8: Bump version + Commit + Push

```bash
./scripts/bump-version.sh patch
# Edit .claude-plugin/plugin.json: "version": "1.25.0"
git add skills/ava-web/ bundles.json README.md CLAUDE.md CHANGELOG.md \
        cli-installer/package.json cli-installer/package-lock.json cli-installer/README.md \
        .claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "feat: add ava-web skill v1.0.0 and bump to v1.25.0"
git tag v1.25.0
git push origin main && git push origin v1.25.0
```

---

## Quality Criteria

- [ ] SKILL.md frontmatter has ONLY `name`, `description`, `license`
- [ ] No date in SKILL.md (only in README.md Metadata)
- [ ] Description starts with "This skill should be used when..."
- [ ] Accessibility documented: orange only in text ≥24px bold or decorative
- [ ] At least 4 stacks documented (HTML, Tailwind, React, Next.js)
- [ ] Reusable wave SVG included in references
- [ ] All components have HTML + Tailwind + React versions
- [ ] evals.json has 5 realistic scenarios
- [ ] trigger-eval.json has 15 queries (10 true, 5 false)
- [ ] YAML validation passes: `./scripts/validate-skill-yaml.sh skills/ava-web`
