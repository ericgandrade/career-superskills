# Skills Carve-Out Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use executing-plans to implement this plan task-by-task.

**Goal:** Carve out 46 skills from `claude-superskills` into 5 focused repos: `obsidian-superskills` (6), `career-superskills` (20), `product-superskills` (8), `design-superskills` (9), and `avanade-superskills` (3, private). Delete 3 low-value skills. Bump `claude-superskills` to v2.0.0 with 17 skills remaining — a focused core of orchestration, planning, research, and content.

**Architecture:** Each new repo is a full clone of the `claude-superskills` structure — complete with `cli-installer`, `scripts`, `docs`, `.github/workflows`, `.claude-plugin`, `CLAUDE.md`, `VERSIONING.md`, `CHANGELOG.md`, and `bundles.json`. The "copy everything, then adapt" strategy avoids missing files. The `avanade-superskills` repo is private, has no npm package, and uses a self-contained shell installer (`install.sh`) with full functionality: install for all 8 platforms, uninstall, update, and list.

**Tech Stack:** Node.js, npm, GitHub CLI (`gh`), Claude Code plugin model, GitHub Actions.

---

## Decisions Log

| Skill | Decision | Reason |
|-------|----------|--------|
| `code-method` | ❌ Delete | Nome genérico, sem bundle, baixo valor |
| `ai-native-product` | ❌ Delete | Overlap com `product-strategy` |
| `docling-converter` | ❌ Delete | `document-converter` cobre o caso |
| `storytelling-expert` | ✅ Fica em claude-superskills | Content/storytelling genérico |
| `slides` | → `design-superskills` | HTML + Chart.js — coeso com design/UI |
| `mckinsey-strategist` | ✅ Fica em claude-superskills | Estratégia consultiva cross-domínio |

---

## Skill Distribution After Carve-Out

### obsidian-superskills (6 skills) — novo repo público
`obsidian-markdown`, `obsidian-links`, `obsidian-frontmatter`, `obsidian-automation`, `obsidian-note-builder`, `obsidian-canvas`

### career-superskills (20 skills) — novo repo público
`academic-cv-builder`, `career-changer-translator`, `cover-letter-generator`, `creative-portfolio-resume`, `executive-resume-writer`, `interview-prep-generator`, `job-description-analyzer`, `linkedin-profile-optimizer`, `offer-comparison-analyzer`, `portfolio-case-study-writer`, `reference-list-builder`, `resume-ats-optimizer`, `resume-bullet-writer`, `resume-formatter`, `resume-quantifier`, `resume-section-builder`, `resume-tailor`, `resume-version-manager`, `salary-negotiation-prep`, `tech-resume-optimizer`

### product-superskills (8 skills) — novo repo público
`product-strategy`, `product-discovery`, `product-delivery`, `product-leadership`, `product-architecture`, `product-operating-model`, `abx-strategy`, `startup-growth-strategist`

### design-superskills (9 skills) — novo repo público
`ui-ux-pro-max`, `design`, `design-system`, `brand`, `ui-styling`, `slides`, `banner-design`, `excalidraw-diagram`, `mermaid-diagram`

### avanade-superskills (3 skills) — novo repo PRIVADO, sem npm
`avanade-pptx`, `avanade-web`, `avanade-pdf` (novo skill — criado neste plano)

### claude-superskills (17 skills) — bumpa para v2.0.0
Core focado: meta/orquestração, planejamento, pesquisa, conteúdo, e `mckinsey-strategist`.

| Grupo | Skills |
|-------|--------|
| Meta/Orquestração | `skill-creator`, `agent-skill-discovery`, `agent-skill-orchestrator` |
| Planejamento | `brainstorming`, `writing-plans`, `executing-plans`, `prompt-engineer` |
| Pesquisa | `deep-research`, `us-program-research`, `senior-solution-architect`, `webpage-reader` |
| Conteúdo/Mídia | `youtube-summarizer`, `audio-transcriber`, `pptx-translator`, `document-converter`, `storytelling-expert` |
| Estratégia | `mckinsey-strategist` |

---

## Task 1: Criar obsidian-superskills

### 1.1 — Clonar estrutura base

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects

# Copia tudo do repo principal como base (exceto git history e artefatos)
cp -r claude-superskills obsidian-superskills
cd obsidian-superskills

# Remove git history — será um repo novo
rm -rf .git

# Remove artefatos gerados e específicos do repo original
rm -rf output/ proposta-media/ plugin-output/
rm -rf .codex/ .opencode/ .adal/ .agent/ .cursor/ .gemini/
rm -f .claude/settings.local.json
rm -f .DS_Store

# Remove planos internos do repo original
rm -rf docs/plans/ docs/plan/
```

### 1.2 — Manter apenas os 6 skills Obsidian

```bash
# Dentro de obsidian-superskills/

# Remove TODOS os skills
rm -rf skills/

# Recria a pasta e copia apenas os 6 obsidian
mkdir skills
for skill in obsidian-markdown obsidian-links obsidian-frontmatter obsidian-automation obsidian-note-builder obsidian-canvas; do
  cp -r ../claude-superskills/skills/$skill skills/
done
```

**Verificar:**
```bash
ls skills/ | wc -l   # deve retornar 6
```

### 1.3 — Atualizar cli-installer/package.json

Editar `cli-installer/package.json` — substituir todos os campos com referências ao repo original:

```json
{
  "name": "obsidian-superskills",
  "version": "1.0.0",
  "description": "6 AI skills for Obsidian knowledge management — notes, wikilinks, frontmatter, automation, canvas. Works with Claude Code, GitHub Copilot, and 6 more AI platforms.",
  "main": "lib/index.js",
  "bin": {
    "obsidian-superskills": "bin/cli.js"
  },
  "scripts": {
    "test": "echo '✅ obsidian-superskills - test passed'",
    "build": "cd .. && ./scripts/build-skills.sh",
    "prebuild": "cd .. && ./scripts/build-skills.sh",
    "link": "npm link",
    "unlink": "npm unlink -g obsidian-superskills",
    "generate-index": "cd .. && python3 scripts/generate-skills-index.py",
    "generate-catalog": "cd .. && python3 scripts/generate-catalog.py",
    "generate-all": "npm run build && npm run generate-index && npm run generate-catalog",
    "prepublishOnly": "npm run build && npm test",
    "version": "git add -A"
  },
  "keywords": ["obsidian", "claude", "copilot", "ai", "skills", "cli", "knowledge-management", "note-taking", "pkm"],
  "author": "Eric Andrade",
  "license": "MIT",
  "engines": { "node": ">=14.0.0" },
  "files": ["bin/", "lib/", "README.md"],
  "dependencies": {
    "adm-zip": "^0.5.16",
    "axios": "^1.6.5",
    "chalk": "^4.1.2",
    "commander": "^11.1.0",
    "fs-extra": "^11.2.0",
    "inquirer": "^8.2.5",
    "js-yaml": "^4.1.0",
    "ora": "^5.4.1",
    "semver": "^7.5.4"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ericgandrade/obsidian-superskills.git"
  },
  "bugs": { "url": "https://github.com/ericgandrade/obsidian-superskills/issues" },
  "homepage": "https://github.com/ericgandrade/obsidian-superskills#readme"
}
```

Deletar `cli-installer/package-lock.json` — será regenerado.

### 1.4 — Atualizar cli-installer/bin/cli.js

Fazer busca-e-substituição em `cli-installer/bin/cli.js`:
- `claude-superskills` → `obsidian-superskills`
- `ericgandrade/claude-superskills` → `ericgandrade/obsidian-superskills`
- Qualquer referência ao skill count (`64 skills`, `55 skills`) → `6 skills`

### 1.5 — Atualizar cli-installer/lib/core/downloader.js

Fazer busca-e-substituição:
- `claude-superskills` → `obsidian-superskills`
- `ericgandrade/claude-superskills` → `ericgandrade/obsidian-superskills`
- `~/.claude-superskills/` → `~/.obsidian-superskills/`

O cache path deve ser único por pacote para evitar colisões quando múltiplos repos estão instalados na mesma máquina.

### 1.6 — Atualizar .claude-plugin/plugin.json

```json
{
  "name": "obsidian-superskills",
  "version": "1.0.0",
  "description": "6 AI skills for Obsidian knowledge management — notes, wikilinks, frontmatter, automation, and canvas.",
  "author": "Eric Andrade",
  "license": "MIT",
  "skills": "skills/"
}
```

### 1.7 — Atualizar .claude-plugin/marketplace.json

```json
{
  "plugins": [
    {
      "name": "obsidian-superskills",
      "description": "6 skills for Obsidian users: note building, wikilink management, frontmatter standardization, vault automation, and canvas workspaces.",
      "source": "github",
      "repo": "ericgandrade/obsidian-superskills"
    }
  ]
}
```

### 1.8 — Atualizar bundles.json (raiz)

Substituir o conteúdo completo por:

```json
{
  "version": "1.0.0",
  "generated": "2026-05-07T00:00:00Z",
  "bundles": {
    "all": {
      "name": "All Obsidian Skills",
      "description": "Complete toolkit for Obsidian knowledge management.",
      "skills": [
        "obsidian-markdown",
        "obsidian-links",
        "obsidian-frontmatter",
        "obsidian-automation",
        "obsidian-note-builder",
        "obsidian-canvas"
      ],
      "use_cases": [
        "Creating well-structured Obsidian notes",
        "Managing wikilinks and knowledge graphs",
        "Standardizing frontmatter properties",
        "Automating vault tasks with CLI scripts",
        "Building visual workspaces with Canvas"
      ],
      "target": "Obsidian users, knowledge workers, researchers"
    }
  }
}
```

### 1.9 — Atualizar scripts/release.js

Fazer busca-e-substituição:
- `Claude Superskills` → `Obsidian Superskills`
- `claude-superskills` → `obsidian-superskills`

### 1.10 — Atualizar README.md

Substituir conteúdo do README.md:
- Título: `# 🧠 Obsidian Superskills v1.0.0`
- Badge de versão: `version-1.0.0`
- Skill count badge: `skills-6`
- Descrição: foco em Obsidian knowledge management
- Tabela de skills: apenas os 6 obsidian
- Instruções de install: `npx obsidian-superskills`
- Links do repo: `github.com/ericgandrade/obsidian-superskills`
- Footer: `*Version 1.0.0 | May 2026*`
- Adicionar seção: `## Part of the Superskills Family` com links para `claude-superskills` e `career-superskills`

### 1.11 — Atualizar CLAUDE.md

Fazer busca-e-substituição:
- `claude-superskills` → `obsidian-superskills`
- `v1.25.0` → `v1.0.0`
- skill count: `64` → `6`
- Skill list na architecture tree: manter apenas os 6 obsidian
- npm package name: `obsidian-superskills`

### 1.12 — Atualizar CHANGELOG.md

Substituir com conteúdo inicial:
```markdown
# Changelog

## [1.0.0] - 2026-05-07

### Added
- Initial release — 6 Obsidian knowledge management skills carved out from claude-superskills v1.25.0
- obsidian-markdown: Creates well-structured Obsidian notes
- obsidian-links: Manages wikilinks and knowledge graphs
- obsidian-frontmatter: Standardizes frontmatter properties
- obsidian-automation: Automates vault tasks via CLI scripts
- obsidian-note-builder: Builds structured notes from templates
- obsidian-canvas: Creates visual workspaces with Obsidian Canvas
```

### 1.13 — Atualizar .github/workflows/publish-npm.yml

```bash
sed -i '' 's/claude-superskills/obsidian-superskills/g' .github/workflows/publish-npm.yml
```

Remover manualmente o step `Deprecate old cli-ai-skills package` (não aplicável).

Verificar que não sobrou nenhuma referência:
```bash
grep "claude-superskills\|cli-ai-skills" .github/workflows/publish-npm.yml
# deve retornar vazio
```

### 1.14 — Atualizar docs/

- `docs/INSTALLATION.md` — substituir nome do pacote e URLs
- `docs/guides/getting-started.md` — substituir nome e skill list
- `docs/guides/skill-anatomy.md` — substituir referências ao repo
- `docs/bundles/bundles.md` — substituir com bundles do obsidian

### 1.15 — Reinstalar dependências do cli-installer

```bash
cd cli-installer
npm install
cd ..
```

Verificar que nenhum bundle referencia skills fora dos 6 do repo:
```bash
node -e "
const b = require('./bundles.json');
const skills = require('fs').readdirSync('./skills');
Object.entries(b.bundles).forEach(([name, bundle]) => {
  bundle.skills.forEach(s => {
    if (!skills.includes(s)) console.error('MISSING in bundle', name, ':', s);
  });
});
console.log('bundles ok');
"
# deve imprimir apenas "bundles ok"
```

### 1.16 — Inicializar git e criar repo no GitHub

```bash
git init
git add .
git commit -m "feat: initial release v1.0.0 — 6 Obsidian skills carved out from claude-superskills"

gh repo create ericgandrade/obsidian-superskills \
  --public \
  --description "6 AI skills for Obsidian knowledge management — notes, wikilinks, frontmatter, automation, and canvas"

git remote add origin https://github.com/ericgandrade/obsidian-superskills.git
git branch -M main
git push -u origin main

# OBRIGATÓRIO antes do tag push — sem isso o publish no npm vai falhar silenciosamente
gh secret set NPM_TOKEN --repo ericgandrade/obsidian-superskills --body "$NPM_TOKEN"

git tag v1.0.0
git push origin v1.0.0
```

Tag push aciona o GitHub Actions workflow → publica no npm automaticamente.

**Verificar:**
```bash
# Aguardar ~2min para o Actions completar, depois:
npx obsidian-superskills --version   # deve retornar 1.0.0
claude --plugin-dir ./obsidian-superskills   # deve carregar 6 skills
```

---

## Task 2: Criar career-superskills

### 2.1 — Clonar estrutura base

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects

cp -r claude-superskills career-superskills
cd career-superskills
rm -rf .git output/ proposta-media/ plugin-output/
rm -rf .codex/ .opencode/ .adal/ .agent/ .cursor/ .gemini/
rm -f .claude/settings.local.json .DS_Store
rm -rf docs/plans/ docs/plan/
```

### 2.2 — Manter apenas os 20 skills de carreira

```bash
rm -rf skills/
mkdir skills
for skill in \
  academic-cv-builder career-changer-translator cover-letter-generator \
  creative-portfolio-resume executive-resume-writer interview-prep-generator \
  job-description-analyzer linkedin-profile-optimizer offer-comparison-analyzer \
  portfolio-case-study-writer reference-list-builder resume-ats-optimizer \
  resume-bullet-writer resume-formatter resume-quantifier resume-section-builder \
  resume-tailor resume-version-manager salary-negotiation-prep tech-resume-optimizer; do
  cp -r ../claude-superskills/skills/$skill skills/
done
```

**Verificar:**
```bash
ls skills/ | wc -l   # deve retornar 20
```

### 2.3 — Atualizar cli-installer/package.json

```json
{
  "name": "career-superskills",
  "version": "1.0.0",
  "description": "20 AI skills for job search, resume optimization, and career development. Works with Claude Code, GitHub Copilot, and 6 more AI platforms.",
  "bin": { "career-superskills": "bin/cli.js" },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ericgandrade/career-superskills.git"
  },
  "bugs": { "url": "https://github.com/ericgandrade/career-superskills/issues" },
  "homepage": "https://github.com/ericgandrade/career-superskills#readme"
}
```
(manter todas as outras seções do package.json iguais ao obsidian-superskills)

Deletar `cli-installer/package-lock.json`.

### 2.4 — Atualizar cli-installer/bin/cli.js e downloader.js

Busca-e-substituição:
- `claude-superskills` → `career-superskills`
- `ericgandrade/claude-superskills` → `ericgandrade/career-superskills`
- `~/.claude-superskills/` → `~/.career-superskills/`
- Skill count → `20 skills`

### 2.5 — Atualizar .claude-plugin/plugin.json

```json
{
  "name": "career-superskills",
  "version": "1.0.0",
  "description": "20 AI skills for job search, resume optimization, career transitions, and professional development.",
  "author": "Eric Andrade",
  "license": "MIT",
  "skills": "skills/"
}
```

### 2.6 — Atualizar .claude-plugin/marketplace.json

```json
{
  "plugins": [
    {
      "name": "career-superskills",
      "description": "20 skills for career development: resume optimization, ATS tuning, cover letters, LinkedIn, interview prep, salary negotiation, job search, and portfolio building.",
      "source": "github",
      "repo": "ericgandrade/career-superskills"
    }
  ]
}
```

### 2.7 — Atualizar bundles.json (raiz)

```json
{
  "version": "1.0.0",
  "generated": "2026-05-07T00:00:00Z",
  "bundles": {
    "resume": {
      "name": "Resume & CV",
      "description": "Skills for building, optimizing, and tailoring resumes and CVs.",
      "skills": ["resume-tailor","resume-formatter","resume-quantifier","resume-bullet-writer",
                 "resume-section-builder","resume-ats-optimizer","resume-version-manager",
                 "executive-resume-writer","tech-resume-optimizer","academic-cv-builder",
                 "creative-portfolio-resume"],
      "target": "Job seekers, career changers"
    },
    "job-search": {
      "name": "Job Search",
      "description": "Skills for the full job search cycle: applications, interviews, and offers.",
      "skills": ["job-description-analyzer","offer-comparison-analyzer","interview-prep-generator",
                 "salary-negotiation-prep","cover-letter-generator","linkedin-profile-optimizer",
                 "career-changer-translator"],
      "target": "Active job seekers"
    },
    "portfolio": {
      "name": "Portfolio & Presence",
      "description": "Build your professional brand and online presence.",
      "skills": ["portfolio-case-study-writer","reference-list-builder",
                 "linkedin-profile-optimizer","creative-portfolio-resume"],
      "target": "Creatives, consultants, executives"
    },
    "all": {
      "name": "All Career Skills",
      "description": "Complete career development toolkit — all 20 skills.",
      "skills": ["academic-cv-builder","career-changer-translator","cover-letter-generator",
                 "creative-portfolio-resume","executive-resume-writer","interview-prep-generator",
                 "job-description-analyzer","linkedin-profile-optimizer","offer-comparison-analyzer",
                 "portfolio-case-study-writer","reference-list-builder","resume-ats-optimizer",
                 "resume-bullet-writer","resume-formatter","resume-quantifier","resume-section-builder",
                 "resume-tailor","resume-version-manager","salary-negotiation-prep","tech-resume-optimizer"],
      "target": "All career stages"
    }
  }
}
```

### 2.8 — Atualizar scripts/release.js, README.md, CLAUDE.md, CHANGELOG.md, docs/

Mesmas substituições que Task 1.9–1.14, com:
- `Obsidian Superskills` → `Career Superskills`
- `obsidian-superskills` → `career-superskills`
- Skill count: `20 skills`
- Título: `# 💼 Career Superskills v1.0.0`

Incluindo o step 1.13 para o workflow:
```bash
sed -i '' 's/claude-superskills/career-superskills/g' .github/workflows/publish-npm.yml
# Remover step "Deprecate old cli-ai-skills package"
grep "claude-superskills\|cli-ai-skills" .github/workflows/publish-npm.yml  # deve retornar vazio
```

### 2.9 — Reinstalar dependências e publicar

```bash
cd cli-installer && npm install && cd ..
```

Verificar bundles:
```bash
node -e "
const b = require('./bundles.json');
const skills = require('fs').readdirSync('./skills');
Object.entries(b.bundles).forEach(([name, bundle]) => {
  bundle.skills.forEach(s => {
    if (!skills.includes(s)) console.error('MISSING in bundle', name, ':', s);
  });
});
console.log('bundles ok');
"
```

```bash
git init
git add .
git commit -m "feat: initial release v1.0.0 — 20 career skills carved out from claude-superskills"

gh repo create ericgandrade/career-superskills \
  --public \
  --description "20 AI skills for job search, resume optimization, and career development"

git remote add origin https://github.com/ericgandrade/career-superskills.git
git branch -M main
git push -u origin main

# OBRIGATÓRIO antes do tag push — sem isso o publish no npm vai falhar silenciosamente
gh secret set NPM_TOKEN --repo ericgandrade/career-superskills --body "$NPM_TOKEN"

git tag v1.0.0 && git push origin v1.0.0
```

**Verificar:**
```bash
npx career-superskills --version    # deve retornar 1.0.0
claude --plugin-dir ./career-superskills   # deve carregar 20 skills
```

---

## Task 3: Criar product-superskills

Segue exatamente o mesmo padrão do Task 1 (obsidian) e Task 2 (career): clone completo, adaptar, npm, GitHub Actions.

### 3.1 — Clonar estrutura base

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects
cp -r claude-superskills product-superskills
cd product-superskills
rm -rf .git output/ proposta-media/ plugin-output/
rm -rf .codex/ .opencode/ .adal/ .agent/ .cursor/ .gemini/
rm -f .claude/settings.local.json .DS_Store
rm -rf docs/plans/ docs/plan/
```

### 3.2 — Manter apenas os 8 skills de produto

```bash
rm -rf skills/
mkdir skills
for skill in \
  product-strategy product-discovery product-delivery product-leadership \
  product-architecture product-operating-model abx-strategy startup-growth-strategist; do
  cp -r ../claude-superskills/skills/$skill skills/
done
```

**Verificar:** `ls skills/ | wc -l` → 8

### 3.3 — Atualizar cli-installer/package.json

```json
{
  "name": "product-superskills",
  "version": "1.0.0",
  "description": "8 AI skills for product management, strategy, and GTM — product discovery, delivery, architecture, ABX, and startup growth. Works with Claude Code, GitHub Copilot, and 6 more AI platforms.",
  "bin": { "product-superskills": "bin/cli.js" },
  "keywords": ["product-management", "strategy", "gtm", "abx", "startup", "claude", "copilot", "ai", "skills"],
  "repository": { "type": "git", "url": "git+https://github.com/ericgandrade/product-superskills.git" },
  "bugs": { "url": "https://github.com/ericgandrade/product-superskills/issues" },
  "homepage": "https://github.com/ericgandrade/product-superskills#readme"
}
```

Deletar `cli-installer/package-lock.json`.

### 3.4 — Atualizar cli-installer/bin/cli.js e downloader.js

Busca-e-substituição:
- `claude-superskills` → `product-superskills`
- `ericgandrade/claude-superskills` → `ericgandrade/product-superskills`
- `~/.claude-superskills/` → `~/.product-superskills/`
- Skill count → `8 skills`

### 3.5 — Atualizar .claude-plugin/plugin.json

```json
{
  "name": "product-superskills",
  "version": "1.0.0",
  "description": "8 AI skills for product management, product strategy, GTM, ABX, and startup growth.",
  "author": "Eric Andrade",
  "license": "MIT",
  "skills": "skills/"
}
```

### 3.6 — Atualizar .claude-plugin/marketplace.json

```json
{
  "plugins": [
    {
      "name": "product-superskills",
      "description": "8 skills for product managers and strategists: product strategy, discovery, delivery, leadership, architecture, operating model, ABX strategy, and startup growth.",
      "source": "github",
      "repo": "ericgandrade/product-superskills"
    }
  ]
}
```

### 3.7 — Atualizar bundles.json

```json
{
  "version": "1.0.0",
  "generated": "2026-05-08T00:00:00Z",
  "bundles": {
    "core-product": {
      "name": "Core Product Management",
      "description": "End-to-end product management: strategy, discovery, delivery, leadership, and operating model.",
      "skills": ["product-strategy","product-discovery","product-delivery","product-leadership","product-operating-model"],
      "target": "Product Managers, CPOs"
    },
    "architecture": {
      "name": "Product Architecture",
      "description": "Technical product and systems design.",
      "skills": ["product-architecture"],
      "target": "Technical PMs, Architects"
    },
    "growth": {
      "name": "Growth & GTM",
      "description": "Go-to-market strategy and startup growth frameworks.",
      "skills": ["abx-strategy","startup-growth-strategist"],
      "target": "Founders, GTM leads, Strategists"
    },
    "all": {
      "name": "All Product Skills",
      "description": "Complete product management and strategy toolkit — all 8 skills.",
      "skills": ["product-strategy","product-discovery","product-delivery","product-leadership",
                 "product-architecture","product-operating-model","abx-strategy","startup-growth-strategist"],
      "target": "PMs, Founders, Strategists"
    }
  }
}
```

### 3.8 — Atualizar scripts/release.js, README.md, CLAUDE.md, CHANGELOG.md, docs/

- `Claude Superskills` → `Product Superskills`
- `claude-superskills` → `product-superskills`
- Skill count: `8 skills`
- Título README: `# 📦 Product Superskills v1.0.0`
- CHANGELOG: `## [1.0.0] - 2026-05-08 — Initial release, 8 product skills carved out from claude-superskills v1.25.0`

Workflow:
```bash
sed -i '' 's/claude-superskills/product-superskills/g' .github/workflows/publish-npm.yml
# Remover step "Deprecate old cli-ai-skills package"
grep "claude-superskills\|cli-ai-skills" .github/workflows/publish-npm.yml  # deve retornar vazio
```

### 3.9 — Reinstalar dependências e publicar

```bash
cd cli-installer && npm install && cd ..
```

Verificar bundles:
```bash
node -e "
const b = require('./bundles.json');
const skills = require('fs').readdirSync('./skills');
Object.entries(b.bundles).forEach(([name, bundle]) => {
  bundle.skills.forEach(s => {
    if (!skills.includes(s)) console.error('MISSING in bundle', name, ':', s);
  });
});
console.log('bundles ok');
"
```

```bash
git init
git add .
git commit -m "feat: initial release v1.0.0 — 8 product skills carved out from claude-superskills"
gh repo create ericgandrade/product-superskills \
  --public \
  --description "8 AI skills for product management, strategy, GTM, and startup growth"
git remote add origin https://github.com/ericgandrade/product-superskills.git
git branch -M main
git push -u origin main

# OBRIGATÓRIO antes do tag push — sem isso o publish no npm vai falhar silenciosamente
gh secret set NPM_TOKEN --repo ericgandrade/product-superskills --body "$NPM_TOKEN"

git tag v1.0.0 && git push origin v1.0.0
```

**Verificar:**
```bash
npx product-superskills --version           # → 1.0.0
ls skills/ | wc -l                          # → 8
claude --plugin-dir ./product-superskills   # carrega 8 skills sem erro
gh repo view ericgandrade/product-superskills --json visibility -q .visibility  # → PUBLIC
```

---

## Task 4: Criar design-superskills

Segue exatamente o mesmo padrão de Task 1, 2 e 3.

### 4.1 — Clonar estrutura base

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects
cp -r claude-superskills design-superskills
cd design-superskills
rm -rf .git output/ proposta-media/ plugin-output/
rm -rf .codex/ .opencode/ .adal/ .agent/ .cursor/ .gemini/
rm -f .claude/settings.local.json .DS_Store
rm -rf docs/plans/ docs/plan/
```

### 4.2 — Manter apenas os 9 skills de design

```bash
rm -rf skills/
mkdir skills
for skill in \
  ui-ux-pro-max design design-system brand ui-styling slides banner-design \
  excalidraw-diagram mermaid-diagram; do
  cp -r ../claude-superskills/skills/$skill skills/
done
```

**Verificar:** `ls skills/ | wc -l` → 9

### 4.3 — Atualizar cli-installer/package.json

```json
{
  "name": "design-superskills",
  "version": "1.0.0",
  "description": "9 AI skills for UI/UX design, brand identity, design systems, component styling, diagrams, presentations, and banner design. Works with Claude Code, GitHub Copilot, and 6 more AI platforms.",
  "bin": { "design-superskills": "bin/cli.js" },
  "keywords": ["design", "ui-ux", "brand", "design-system", "figma", "diagrams", "claude", "copilot", "ai", "skills"],
  "repository": { "type": "git", "url": "git+https://github.com/ericgandrade/design-superskills.git" },
  "bugs": { "url": "https://github.com/ericgandrade/design-superskills/issues" },
  "homepage": "https://github.com/ericgandrade/design-superskills#readme"
}
```

Deletar `cli-installer/package-lock.json`.

### 4.4 — Atualizar cli-installer/bin/cli.js e downloader.js

Busca-e-substituição:
- `claude-superskills` → `design-superskills`
- `ericgandrade/claude-superskills` → `ericgandrade/design-superskills`
- `~/.claude-superskills/` → `~/.design-superskills/`
- Skill count → `9 skills`

### 4.5 — Atualizar .claude-plugin/plugin.json

```json
{
  "name": "design-superskills",
  "version": "1.0.0",
  "description": "9 AI skills for UI/UX design, brand identity, design systems, styling, diagrams, presentations, and banners.",
  "author": "Eric Andrade",
  "license": "MIT",
  "skills": "skills/"
}
```

### 4.6 — Atualizar .claude-plugin/marketplace.json

```json
{
  "plugins": [
    {
      "name": "design-superskills",
      "description": "9 skills for designers and front-end teams: UI/UX intelligence, brand identity, design systems, component styling, Mermaid/Excalidraw diagrams, HTML presentations, and banner design.",
      "source": "github",
      "repo": "ericgandrade/design-superskills"
    }
  ]
}
```

### 4.7 — Atualizar bundles.json

```json
{
  "version": "1.0.0",
  "generated": "2026-05-08T00:00:00Z",
  "bundles": {
    "ui-ux": {
      "name": "UI/UX Design",
      "description": "Comprehensive UI/UX design intelligence and component styling.",
      "skills": ["ui-ux-pro-max","ui-styling","design"],
      "target": "Front-end developers, product designers"
    },
    "brand-identity": {
      "name": "Brand & Identity",
      "description": "Brand identity, design systems, and token architecture.",
      "skills": ["brand","design-system"],
      "target": "Brand designers, design leads"
    },
    "visual": {
      "name": "Visual & Diagrams",
      "description": "Diagrams, presentations, and banner design.",
      "skills": ["mermaid-diagram","excalidraw-diagram","slides","banner-design"],
      "target": "Developers, content creators, marketers"
    },
    "all": {
      "name": "All Design Skills",
      "description": "Complete design toolkit — all 9 skills.",
      "skills": ["ui-ux-pro-max","design","design-system","brand","ui-styling",
                 "slides","banner-design","excalidraw-diagram","mermaid-diagram"],
      "target": "Designers, front-end developers, product teams"
    }
  }
}
```

### 4.8 — Atualizar scripts/release.js, README.md, CLAUDE.md, CHANGELOG.md, docs/

- `Claude Superskills` → `Design Superskills`
- `claude-superskills` → `design-superskills`
- Skill count: `9 skills`
- Título README: `# 🎨 Design Superskills v1.0.0`
- CHANGELOG: `## [1.0.0] - 2026-05-08 — Initial release, 9 design skills carved out from claude-superskills v1.25.0`

Workflow:
```bash
sed -i '' 's/claude-superskills/design-superskills/g' .github/workflows/publish-npm.yml
# Remover step "Deprecate old cli-ai-skills package"
grep "claude-superskills\|cli-ai-skills" .github/workflows/publish-npm.yml  # deve retornar vazio
```

### 4.9 — Reinstalar dependências e publicar

```bash
cd cli-installer && npm install && cd ..
```

Verificar bundles:
```bash
node -e "
const b = require('./bundles.json');
const skills = require('fs').readdirSync('./skills');
Object.entries(b.bundles).forEach(([name, bundle]) => {
  bundle.skills.forEach(s => {
    if (!skills.includes(s)) console.error('MISSING in bundle', name, ':', s);
  });
});
console.log('bundles ok');
"
```

```bash
git init
git add .
git commit -m "feat: initial release v1.0.0 — 9 design/UI skills carved out from claude-superskills"
gh repo create ericgandrade/design-superskills \
  --public \
  --description "9 AI skills for UI/UX design, brand identity, design systems, diagrams, and presentations"
git remote add origin https://github.com/ericgandrade/design-superskills.git
git branch -M main
git push -u origin main

# OBRIGATÓRIO antes do tag push — sem isso o publish no npm vai falhar silenciosamente
gh secret set NPM_TOKEN --repo ericgandrade/design-superskills --body "$NPM_TOKEN"

git tag v1.0.0 && git push origin v1.0.0
```

**Verificar:**
```bash
npx design-superskills --version           # → 1.0.0
ls skills/ | wc -l                         # → 9
claude --plugin-dir ./design-superskills   # carrega 9 skills sem erro
gh repo view ericgandrade/design-superskills --json visibility -q .visibility  # → PUBLIC
```

---

## Task 5: Criar avanade-superskills (privado, sem npm, com shell installer)

O `avanade-superskills` é privado e não vai para npm. Em vez do cli-installer Node.js, usa um shell installer completo (`install.sh`) com as mesmas funcionalidades: install para todas as 8 plataformas, uninstall, update, e list.

### 3.1 — Criar estrutura do repo

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects
mkdir avanade-superskills && cd avanade-superskills
git init

mkdir -p skills .claude-plugin docs scripts
```

### 3.2 — Copiar e renomear os 2 skills existentes

Os skills existem como `ava-pptx` e `ava-web` em `claude-superskills`. Precisam ser copiados **e renomeados**.

```bash
cp -r ../claude-superskills/skills/ava-pptx skills/avanade-pptx
cp -r ../claude-superskills/skills/ava-web  skills/avanade-web
```

Atualizar o campo `name` no frontmatter de cada SKILL.md:

`skills/avanade-pptx/SKILL.md` — linha 2: `name: avanade-pptx`
`skills/avanade-web/SKILL.md` — linha 2: `name: avanade-web`

Verificar referências internas ao nome antigo:
```bash
grep -r "ava-pptx\|ava-web" skills/
# Substituir qualquer ocorrência por avanade-pptx / avanade-web
```

### 3.3 — Criar o novo skill avanade-pdf

Criar `skills/avanade-pdf/SKILL.md`:

```markdown
---
name: avanade-pdf
description: Use this skill when the user needs to generate any document (report, proposal, brief, executive summary, technical document) as a branded PDF following official Avanade visual identity guidelines — colors, fonts, logo placement, header, and footer.
license: MIT
---

# Avanade PDF Generator

## Role

You are an Avanade document designer. Generate well-structured, on-brand PDF documents using HTML + CSS rendered to PDF via WeasyPrint or wkhtmltopdf.

## Avanade Brand Guidelines

**Colors:**
- Primary purple: `#A100FF`
- Dark background: `#1A1A2E`
- Light gray: `#F5F5F5`
- Text dark: `#1A1A1A`
- Text light: `#FFFFFF`
- Accent: `#7700CC`

**Typography:**
- Headings: Arial Bold or GT Walsheim (fallback: Arial)
- Body: Arial or Calibri, 11pt, line-height 1.6
- Code/mono: Courier New

**Layout:**
- Header: Avanade logo top-left, document title top-right, purple top border (4px)
- Footer: page number center, "Confidential — Avanade" right, purple bottom border (2px)
- Margins: 2.5cm top/bottom, 2cm left/right
- Cover page: full purple left stripe (40%), white right content area

## When to Use

Trigger when the user asks to:
- Generate a PDF in Avanade format
- Create an Avanade-branded document (report, proposal, brief, executive summary)
- Export content as a branded Avanade PDF

## Workflow

### Step 1 — Gather Content

Ask the user for:
1. Document type (report / proposal / brief / executive summary / other)
2. Title and subtitle
3. Author name and date
4. Content sections (or ask user to paste raw content to be formatted)
5. Confidentiality level (Confidential / Internal / Public)

### Step 2 — Generate HTML Template

Produce a complete, self-contained HTML file with embedded CSS following the brand guidelines above. Structure:
- Cover page
- Table of contents (if >3 sections)
- Content sections with Avanade-styled headings, tables, and callout boxes
- Back page with Avanade contact/legal footer

### Step 3 — Convert to PDF

```bash
# Option A: WeasyPrint (preferred — pure Python, no X11 dependency)
pip install weasyprint
weasyprint document.html document.pdf

# Option B: wkhtmltopdf (fallback)
wkhtmltopdf --page-size A4 --margin-top 25mm --margin-bottom 25mm \
            --margin-left 20mm --margin-right 20mm \
            document.html document.pdf
```

### Step 4 — Output

Save the PDF to the current directory as `{document-title}-avanade.pdf`.
Report: file path, page count, file size.

## Critical Rules

- NEVER use colors outside the Avanade palette
- ALWAYS include the Avanade logo placeholder (`[AVANADE LOGO]`) — user replaces with actual SVG/PNG
- ALWAYS include page numbers in the footer
- Match the document type to an appropriate structure (proposal ≠ report ≠ brief)
- If WeasyPrint is not installed, instruct the user to install it before proceeding

## Example Usage

1. "Generate an Avanade PDF proposal for a digital transformation engagement"
2. "Create a branded executive summary PDF for the Q2 results"
3. "Make an Avanade technical report on AI infrastructure recommendations"
```

Criar `skills/avanade-pdf/README.md`:

```markdown
## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-05-07 |
| Updated | 2026-05-07 |
| Platforms | All 8 |
| Category | Document Generation |
| Tags | pdf, avanade, branding, document, report, proposal |
| Risk | Low |
```

### 3.4 — Criar .claude-plugin/plugin.json

```json
{
  "name": "avanade-superskills",
  "version": "1.0.0",
  "description": "Avanade-branded AI skills for PowerPoint, web, and PDF generation following official Avanade visual identity guidelines.",
  "author": "Eric Andrade",
  "license": "MIT",
  "skills": "skills/"
}
```

### 3.5 — Criar o shell installer (scripts/install.sh)

Este script substitui o cli-installer Node.js. Deve ter as mesmas funcionalidades do `npx claude-superskills`.

Criar `scripts/install.sh`:

```bash
#!/usr/bin/env bash
set -e

REPO="ericgandrade/avanade-superskills"
CACHE_DIR="$HOME/.avanade-superskills/cache"
VERSION="1.0.0"

# Platform install paths
CLAUDE_DIR="$HOME/.claude/skills"
COPILOT_DIR="$HOME/.github/skills"
CODEX_DIR="$HOME/.codex/skills"
OPENCODE_DIR="$HOME/.agent/skills"
GEMINI_DIR="$HOME/.gemini/skills"
ANTIGRAVITY_DIR="$HOME/.gemini/antigravity/skills"
CURSOR_DIR="$HOME/.cursor/skills"
ADAL_DIR="$HOME/.adal/skills"

SKILLS_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)/skills"

usage() {
  echo "Usage: $0 [command] [options]"
  echo ""
  echo "Commands:"
  echo "  install    Install skills to all detected platforms (default)"
  echo "  uninstall  Remove skills from all platforms"
  echo "  update     Pull latest and reinstall"
  echo "  list       List installed skills"
  echo ""
  echo "Options:"
  echo "  --all      Install to all platforms without prompting"
  echo "  --quiet    Suppress non-essential output"
}

detect_platforms() {
  DETECTED=()
  command -v claude    &>/dev/null && DETECTED+=("claude")
  command -v gh        &>/dev/null && DETECTED+=("copilot")
  command -v codex     &>/dev/null && DETECTED+=("codex")
  command -v opencode  &>/dev/null && DETECTED+=("opencode")
  command -v gemini    &>/dev/null && DETECTED+=("gemini")
  command -v cursor    &>/dev/null && DETECTED+=("cursor")
  command -v adal      &>/dev/null && DETECTED+=("adal")
}

install_to_platform() {
  local platform="$1"
  local target_dir=""
  case "$platform" in
    claude)      target_dir="$CLAUDE_DIR" ;;
    copilot)     target_dir="$COPILOT_DIR" ;;
    codex)       target_dir="$CODEX_DIR" ;;
    opencode)    target_dir="$OPENCODE_DIR" ;;
    gemini)      target_dir="$GEMINI_DIR" ;;
    antigravity) target_dir="$ANTIGRAVITY_DIR" ;;
    cursor)      target_dir="$CURSOR_DIR" ;;
    adal)        target_dir="$ADAL_DIR" ;;
    *) echo "Unknown platform: $platform"; return 1 ;;
  esac
  mkdir -p "$target_dir"
  cp -r "$SKILLS_DIR"/. "$target_dir/"
  echo "✅ Installed to $target_dir"
}

uninstall_from_platform() {
  local platform="$1"
  local target_dir=""
  case "$platform" in
    claude)      target_dir="$CLAUDE_DIR" ;;
    copilot)     target_dir="$COPILOT_DIR" ;;
    codex)       target_dir="$CODEX_DIR" ;;
    opencode)    target_dir="$OPENCODE_DIR" ;;
    gemini)      target_dir="$GEMINI_DIR" ;;
    antigravity) target_dir="$ANTIGRAVITY_DIR" ;;
    cursor)      target_dir="$CURSOR_DIR" ;;
    adal)        target_dir="$ADAL_DIR" ;;
  esac
  for skill in "$SKILLS_DIR"/*/; do
    skill_name=$(basename "$skill")
    rm -rf "${target_dir:?}/$skill_name"
  done
  echo "🗑️  Uninstalled from $target_dir"
}

cmd_install() {
  detect_platforms
  echo "🔍 Detected platforms: ${DETECTED[*]}"
  for platform in "${DETECTED[@]}"; do
    install_to_platform "$platform"
  done
  echo "✅ avanade-superskills v$VERSION installed."
}

cmd_uninstall() {
  detect_platforms
  for platform in "${DETECTED[@]}"; do
    uninstall_from_platform "$platform"
  done
  echo "✅ avanade-superskills uninstalled."
}

cmd_update() {
  echo "⬇️  Pulling latest from GitHub..."
  git -C "$(dirname "$SKILLS_DIR")" pull origin main
  cmd_install
}

cmd_list() {
  echo "📦 Skills in avanade-superskills v$VERSION:"
  for skill in "$SKILLS_DIR"/*/; do
    echo "  - $(basename "$skill")"
  done
}

COMMAND="${1:-install}"
case "$COMMAND" in
  install)   cmd_install ;;
  uninstall) cmd_uninstall ;;
  update)    cmd_update ;;
  list)      cmd_list ;;
  --help|-h) usage ;;
  *) echo "Unknown command: $COMMAND"; usage; exit 1 ;;
esac
```

```bash
chmod +x scripts/install.sh
```

Criar `scripts/uninstall.sh` como wrapper:
```bash
#!/usr/bin/env bash
"$(dirname "${BASH_SOURCE[0]}")/install.sh" uninstall
```
```bash
chmod +x scripts/uninstall.sh
```

### 3.6 — Criar CLAUDE.md

```markdown
# avanade-superskills

Avanade-branded AI skills — private repository, not published to npm.

## Skills (3)
- `avanade-pptx` — PowerPoint presentations following Avanade brand guidelines
- `avanade-web` — Web page generation following Avanade visual identity
- `avanade-pdf` — PDF document generation (any type) in Avanade format

## Prerequisites

- Must be added as a collaborator on this private repo (contact @ericgandrade)
- SSH key configured for GitHub — `ssh -T git@github.com` must succeed before cloning

## Installation

### Shell installer (all 8 platforms)
```bash
git clone git@github.com:ericgandrade/avanade-superskills.git
cd avanade-superskills
./scripts/install.sh           # install to all detected platforms
./scripts/install.sh list      # list installed skills
./scripts/install.sh update    # pull latest and reinstall
./scripts/install.sh uninstall # remove from all platforms
```

### Claude Code plugin (direct, no install)
```bash
claude --plugin-dir ./avanade-superskills
```

## Version
v1.0.0

## Version Management
To bump version: update `version` in `.claude-plugin/plugin.json` and `scripts/install.sh`, then update `CHANGELOG.md`, commit, and tag.
```

### 3.7 — Criar README.md

README com:
- Título: `# Avanade Superskills v1.0.0`
- Descrição: 3 skills para Avanade-branded content generation
- **Seção de pré-requisitos** (antes do install):
  ```markdown
  ## Prerequisites
  - You must be added as a collaborator on the private GitHub repo.
    Contact @ericgandrade to request access.
  - SSH key configured for GitHub (`ssh -T git@github.com` must succeed).
    HTTPS clone will fail on private repos without a PAT.
  ```
- Seção de install com SSH (não HTTPS):
  ```bash
  git clone git@github.com:ericgandrade/avanade-superskills.git
  cd avanade-superskills
  ./scripts/install.sh
  ```
- Dois métodos: shell installer + `claude --plugin-dir`
- Tabela dos 3 skills
- Nota: repo privado, uso interno Avanade

### 3.8 — Criar CHANGELOG.md, .gitignore, LICENSE

`.gitignore` — copiar de `claude-superskills`.

`CHANGELOG.md`:
```markdown
# Changelog

## [1.0.0] - 2026-05-07

### Added
- avanade-pptx: PowerPoint generation with Avanade brand (renamed from ava-pptx)
- avanade-web: Web page generation with Avanade brand (renamed from ava-web)
- avanade-pdf: New skill — generate any document type as branded Avanade PDF
- scripts/install.sh: Full shell installer — install/uninstall/update/list for all 8 AI platforms
```

`LICENSE` — copiar de `claude-superskills`.

### 3.9 — Criar repo privado e push

```bash
git add .
git commit -m "feat: initial release v1.0.0 — 3 Avanade-branded skills + shell installer"

gh repo create ericgandrade/avanade-superskills \
  --private \
  --description "Avanade-branded AI skills — internal use only (avanade-pptx, avanade-web, avanade-pdf)"

git remote add origin git@github.com:ericgandrade/avanade-superskills.git
git branch -M main
git push -u origin main
git tag v1.0.0 && git push origin v1.0.0
```

**Verificar:**
```bash
ls skills/ | wc -l                          # → 3
./scripts/install.sh list                   # lista os 3 skills
claude --plugin-dir ./avanade-superskills   # carrega 3 skills sem erro
gh repo view ericgandrade/avanade-superskills --json visibility -q .visibility  # → PRIVATE
```

---

## Task 6: Limpar claude-superskills e bumpar para v2.0.0

> ⛔ **PRÉ-REQUISITO:** Tasks 1–5 devem estar 100% completas e todos os pacotes npm visíveis no registry antes de iniciar esta task.
> Verificar:
> ```bash
> npm view obsidian-superskills version   # → 1.0.0
> npm view career-superskills version     # → 1.0.0
> npm view product-superskills version    # → 1.0.0
> npm view design-superskills version     # → 1.0.0
> gh repo view ericgandrade/avanade-superskills --json visibility -q .visibility  # → PRIVATE
> ```
> Só prosseguir após todos os 5 checks passarem.

### 6.1 — Deletar skills removidos

```bash
cd ~/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/claude-superskills

# Remove obsidian skills (agora em obsidian-superskills)
git rm -r skills/obsidian-markdown skills/obsidian-links skills/obsidian-frontmatter \
           skills/obsidian-automation skills/obsidian-note-builder skills/obsidian-canvas

# Remove career skills (agora em career-superskills)
git rm -r skills/academic-cv-builder skills/career-changer-translator skills/cover-letter-generator \
           skills/creative-portfolio-resume skills/executive-resume-writer skills/interview-prep-generator \
           skills/job-description-analyzer skills/linkedin-profile-optimizer skills/offer-comparison-analyzer \
           skills/portfolio-case-study-writer skills/reference-list-builder skills/resume-ats-optimizer \
           skills/resume-bullet-writer skills/resume-formatter skills/resume-quantifier \
           skills/resume-section-builder skills/resume-tailor skills/resume-version-manager \
           skills/salary-negotiation-prep skills/tech-resume-optimizer

# Remove product skills (agora em product-superskills)
git rm -r skills/product-strategy skills/product-discovery skills/product-delivery \
           skills/product-leadership skills/product-architecture skills/product-operating-model \
           skills/abx-strategy skills/startup-growth-strategist

# Remove design skills (agora em design-superskills)
git rm -r skills/ui-ux-pro-max skills/design skills/design-system skills/brand \
           skills/ui-styling skills/slides skills/banner-design \
           skills/excalidraw-diagram skills/mermaid-diagram

# Remove ava skills (agora em avanade-superskills como avanade-pptx / avanade-web)
git rm -r skills/ava-pptx skills/ava-web

# Deleta skills de baixo valor (decisão 2026-05-07)
git rm -r skills/code-method skills/ai-native-product skills/docling-converter
```

**Verificar:**
```bash
ls skills/ | wc -l   # deve retornar 17
```

### 6.2 — Atualizar bundles.json

Remover dos bundles todos os skills movidos para outros repos:
- Todos os obsidian skills
- Todos os career skills
- Todos os product skills (`product-strategy`, `product-discovery`, `product-delivery`, `product-leadership`, `product-architecture`, `product-operating-model`, `abx-strategy`, `startup-growth-strategist`)
- Todos os design skills (`ui-ux-pro-max`, `design`, `design-system`, `brand`, `ui-styling`, `slides`, `banner-design`, `excalidraw-diagram`, `mermaid-diagram`)
- `ava-pptx`, `ava-web`
- `code-method`, `ai-native-product`, `docling-converter`

Remover completamente os bundles `career`, `obsidian`, `product`, e `design` (agora são repos separados). Manter apenas: `essential`, `planning`, `research`, `content`, `orchestration`, `all`.

### 6.3 — Bumpar para v2.0.0

```bash
node scripts/release.js major
```

Editar `CHANGELOG.md` — substituir o placeholder gerado pelo release.js com:
```markdown
## [2.0.0] - 2026-05-08

### Breaking Changes
- Removed 6 Obsidian skills → now available at github.com/ericgandrade/obsidian-superskills (`npx obsidian-superskills`)
- Removed 20 career/resume skills → now available at github.com/ericgandrade/career-superskills (`npx career-superskills`)
- Removed 8 product/strategy skills → now available at github.com/ericgandrade/product-superskills (`npx product-superskills`)
- Removed 9 design/UI skills → now available at github.com/ericgandrade/design-superskills (`npx design-superskills`)
- Removed 2 Avanade-branded skills (`ava-pptx` → renamed to `avanade-pptx`, `ava-web` → renamed to `avanade-web`) → now in private repo ericgandrade/avanade-superskills

### Removed
- Deleted low-value skills: `code-method`, `ai-native-product`, `docling-converter`

### Changed
- claude-superskills is now a focused core: 17 skills across meta/orchestration, planning, research, and content
```

### 6.4 — Atualizar README.md

- Título: `# 🤖 Claude Superskills v2.0.0`
- Skill count badge: `skills-17`
- Remover tabelas de skills removidos
- Adicionar seção `## Related Packages`:
  ```markdown
  ## Related Packages
  | Package | Skills | Focus |
  |---------|--------|-------|
  | [obsidian-superskills](https://github.com/ericgandrade/obsidian-superskills) | 6 | Obsidian knowledge management |
  | [career-superskills](https://github.com/ericgandrade/career-superskills) | 20 | Job search & career development |
  | [product-superskills](https://github.com/ericgandrade/product-superskills) | 8 | Product management & GTM strategy |
  | [design-superskills](https://github.com/ericgandrade/design-superskills) | 9 | UI/UX design, brand & diagrams |
  ```

### 6.5 — Atualizar CLAUDE.md

- Skill count: `64` → `17`
- Remover skills da architecture tree (obsidian, career, product, design, ava-*)
- Adicionar referências aos novos repos em "Related Packages"
- Atualizar versão: `v1.25.0` → `v2.0.0`
- Atualizar npm description: `"17 core AI skills for Claude Code, GitHub Copilot & 6 more platforms"`

### 6.6 — Atualizar GitHub About

```bash
gh repo edit ericgandrade/claude-superskills \
  --description "17 Universal AI Skills for Claude Code, GitHub Copilot & 6 more platforms. Meta/orchestration, planning, research, and content — the focused core of the Superskills family."
```

### 6.7 — Commit, tag, push

```bash
git add .
git commit -m "feat!: carve out obsidian/career/product/design/ava skills into dedicated repos — bump to v2.0.0"
git tag v2.0.0
git push origin main && git push origin v2.0.0
```

---

## Validation Checklist Final

Execute cada check antes de considerar o plano concluído:

```bash
# obsidian-superskills
npx obsidian-superskills --version              # → 1.0.0
ls ~/Library/CloudStorage/.../obsidian-superskills/skills/ | wc -l   # → 6
claude --plugin-dir ./obsidian-superskills      # carrega 6 skills sem erro
gh repo view ericgandrade/obsidian-superskills --json visibility -q .visibility  # → PUBLIC

# career-superskills
npx career-superskills --version               # → 1.0.0
ls ~/Library/CloudStorage/.../career-superskills/skills/ | wc -l     # → 20
claude --plugin-dir ./career-superskills       # carrega 20 skills sem erro
gh repo view ericgandrade/career-superskills --json visibility -q .visibility    # → PUBLIC

# product-superskills
npx product-superskills --version              # → 1.0.0
ls ~/Library/CloudStorage/.../product-superskills/skills/ | wc -l    # → 8
claude --plugin-dir ./product-superskills      # carrega 8 skills sem erro
gh repo view ericgandrade/product-superskills --json visibility -q .visibility   # → PUBLIC

# design-superskills
npx design-superskills --version               # → 1.0.0
ls ~/Library/CloudStorage/.../design-superskills/skills/ | wc -l     # → 9
claude --plugin-dir ./design-superskills       # carrega 9 skills sem erro
gh repo view ericgandrade/design-superskills --json visibility -q .visibility    # → PUBLIC

# avanade-superskills
ls ~/Library/CloudStorage/.../avanade-superskills/skills/ | wc -l        # → 3
./scripts/install.sh list                                                  # lista avanade-pptx, avanade-web, avanade-pdf
claude --plugin-dir ./avanade-superskills          # carrega 3 skills sem erro
gh repo view ericgandrade/avanade-superskills --json visibility -q .visibility       # → PRIVATE

# claude-superskills
npx claude-superskills --version               # → 2.0.0
ls ~/Library/CloudStorage/.../claude-superskills/skills/ | wc -l     # → 17
claude --plugin-dir ./claude-superskills       # carrega 17 skills sem erro

# npm packages visíveis
npm view obsidian-superskills version          # → 1.0.0
npm view career-superskills version            # → 1.0.0
npm view product-superskills version           # → 1.0.0
npm view design-superskills version            # → 1.0.0
npm view avanade-superskills 2>/dev/null || echo "NOT PUBLISHED (correct)"  # → NOT PUBLISHED
```
