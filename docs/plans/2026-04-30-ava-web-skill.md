# ava-web Skill — Plano de Criação v1.25.0

## Goal

Criar o skill `ava-web` para gerar websites, landing pages e componentes web com identidade visual Avanade.
O skill usa o mesmo sistema de cores/tipografia do `ava-pptx` (brand-guidelines.md), adaptado para web.
Suporta HTML puro, HTML+Tailwind, React+Tailwind e Next.js.

---

## Análise de Referências

### Brand Avanade (extraído de ava-pptx/references/brand-guidelines.md)

| Token | Valor | Uso web |
|-------|-------|---------|
| Primary Orange | `#FF5800` | Botões, links ativos, acentos, bordas de card |
| Dark Orange | `#DC4600` | Hover states, Pantone 159 |
| Yellow Accent | `#FFD700` | Wave accent line, gradiente decorativo |
| Gradient Start | `#FF5800` | Hero backgrounds |
| Gradient Mid | `#B43C14` | Hero backgrounds |
| Gradient End | `#870032` | Hero backgrounds |
| Body Text | `#333333` | Parágrafos, títulos em fundos claros |
| Secondary Text | `#666666` | Subtítulos, captions |
| Tertiary Text | `#999999` | Labels, footers |
| Background Tint | `#FFF0E8` | Callout boxes, seções de destaque suave |
| White | `#FFFFFF` | Texto sobre gradiente, fundos primários |
| Font | Segoe UI | Web: `'Segoe UI', system-ui, -apple-system, sans-serif` |
| Tagline | "Do what matters" | Footer e nav |
| Wave | SVG orgânico | Decoração de seções (reaproveitado de brand-guidelines.md) |

### Padrões visuais observados em avanade.com

- **Hero**: gradiente laranja→roxo full-width, texto branco, onda SVG inferior
- **Nav**: logo esquerda, links no centro/direita, botão CTA laranja
- **Seções de conteúdo**: fundo branco com acentos laranja, grid 2-3 colunas
- **Stats bar**: números grandes em laranja (ex: "50,000+ employees")
- **Cards**: sombra suave, borda superior laranja ou ícone laranja
- **CTA sections**: fundo gradiente ou laranja sólido, texto branco
- **Footer**: escuro com links brancos e logo branca
- **Separadores**: onda SVG entre seções (elemento-assinatura da marca)

---

## Architecture

```
skills/ava-web/
├── SKILL.md                          # Spec principal do skill
├── README.md                         # Documentação usuário
├── references/
│   ├── web-brand-guidelines.md       # CSS vars, tipografia, spacing, componentes
│   ├── components.md                 # Catálogo de componentes c/ código HTML+Tailwind+React
│   └── tailwind-config.md            # Tailwind config com tokens Avanade
└── evals/
    ├── evals.json                    # 5 cenários de teste
    └── trigger-eval.json             # 15 queries trigger/no-trigger
```

---

## Tech Stack

- **HTML puro**: tags semânticas + CSS custom properties + SVG inline
- **HTML + Tailwind**: Tailwind config com tokens Avanade + classes utilitárias
- **React + Tailwind**: componentes JSX, mesma config Tailwind
- **Next.js**: estrutura de páginas Next.js App Router, Tailwind, componentes reutilizáveis

O skill detecta o stack do projeto ou pergunta ao usuário.

---

## Componentes / Seções a Documentar

| Componente | Variantes |
|------------|-----------|
| **Navbar** | Light (branca + laranja), Dark (laranja sólido), Sticky |
| **Hero** | Gradient full-width + onda, Image bg overlay, Solid orange |
| **Features Grid** | 2 cols, 3 cols, 4 cols — cards com ícone laranja |
| **Stats Bar** | Números grandes laranja, fundo claro ou gradiente |
| **Content + Image** | Image left, Image right, alternating |
| **Callout / Quote** | Borda laranja, fundo FFF0E8, pull-quote |
| **CTA Section** | Gradiente (como cover PPTX), sólido laranja |
| **Testimonials** | Card branco, borda laranja, avatar |
| **Footer** | Escuro, logo branca, links, tagline "Do what matters" |
| **Wave Divider** | SVG onda reutilizável entre seções |
| **Button** | Primary (laranja), Secondary (outline laranja), Ghost |
| **Card** | Default shadow, orange top-border, gradient header |

---

## Tipos de Página Suportados

| Tipo | Descrição |
|------|-----------|
| **Landing Page** | Hero + Features + Stats + CTA + Footer |
| **Service Page** | Hero + Content-Image alternating + Features + CTA |
| **About Page** | Hero + Stats + Two-col + Team grid + CTA |
| **Case Study** | Hero + Challenge/Solution/Result + Stats + Quote + CTA |
| **Contact Page** | Hero curto + Form + Map/Office info + Footer |

---

## Tasks

### Task 1: Criar `references/web-brand-guidelines.md`

Documento completo com:
- CSS custom properties (`:root { --ava-orange: #FF5800; ... }`)
- Tailwind config completo com tokens Avanade
- Escala tipográfica para web (px/rem equivalentes às dimensões PPTX)
- Spacing system
- SVG das ondas (reaproveitado de brand-guidelines.md, adaptado para HTML)
- Dark/light section guidance (quando usar gradiente vs branco)
- Regras de acessibilidade (contraste mínimo 4.5:1 — laranja sobre branco FALHA, usar apenas em texto grande ou sobre fundos escuros)

**Atenção crítica — acessibilidade**: `#FF5800` sobre `#FFFFFF` tem contraste **2.8:1** (WCAG AA fail para texto normal). Regra: laranja apenas para texto ≥24px bold (large text — ratio OK ≥3:1) ou como elemento decorativo/CTA. Texto de leitura sempre em `#333333`.

### Task 2: Criar `references/components.md`

Catálogo de todos os 12 componentes com:
- HTML semântico completo
- Versão Tailwind com classes utilitárias
- Versão React (JSX + props)
- Notas de uso e variantes

### Task 3: Criar `references/tailwind-config.md`

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

### Task 4: Criar `SKILL.md`

Workflow completo:
- **Step 0: Discovery** — detectar stack (HTML/Tailwind/React/Next.js), tipo de página, conteúdo
- **Step 1: Load brand** — `Read references/web-brand-guidelines.md` antes de qualquer código
- **Step 2: Select components** — mapear seções necessárias ao tipo de página pedido
- **Step 3: Generate** — código pronto para uso, comentado por seção
- **Step 4: QA** — checklist de brand compliance antes de entregar

Frontmatter mínimo (apenas name, description, license):
```yaml
---
name: ava-web
description: This skill should be used when the user needs to create a website, landing page, web component, or web UI following official Ava brand guidelines. Trigger for "create a landing page", "build a website", "design a web page", "create a hero section", "Avanade-branded website", "create a React component Avanade style", or any request to generate HTML/CSS/React/Next.js code in Avanade visual identity.
license: MIT
---
```

### Task 5: Criar `README.md`

Com tabela Metadata (Version 1.0.0, Author Eric Andrade, Category design, Tags ui, web, avanade, branding, tailwind, react).

### Task 6: Criar evals

`evals/evals.json` — 5 cenários:
1. Landing page para um serviço de AI
2. Hero section React+Tailwind
3. Página de Case Study
4. Componente de Stats bar
5. Footer com tagline "Do what matters"

`evals/trigger-eval.json` — 15 queries (10 true, 5 false como Python scripts, áudio, Excel).

### Task 7: Atualizar metadata do repositório

- `README.md`: 63→64 skills, add `ava-web` na tabela UI/UX Design
- `CLAUDE.md`: contagem + skills tree + bundles
- `CHANGELOG.md`: entrada v1.25.0
- `bundles.json`: add `ava-web` ao bundle `content` (conteúdo branded) e ao `all`
- `cli-installer/README.md`: 63→64
- `.claude-plugin/marketplace.json`: 63→64

### Task 8: Bump version + Commit + Push

```bash
./scripts/bump-version.sh patch
# Editar .claude-plugin/plugin.json: "version": "1.25.0"
git add skills/ava-web/ bundles.json README.md CLAUDE.md CHANGELOG.md \
        cli-installer/package.json cli-installer/package-lock.json cli-installer/README.md \
        .claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "feat: add ava-web skill v1.0.0 and bump to v1.25.0"
git tag v1.25.0
git push origin main && git push origin v1.25.0
```

---

## Critério de Qualidade

- [ ] Frontmatter SKILL.md tem APENAS `name`, `description`, `license`
- [ ] Nenhuma data em SKILL.md (apenas em README.md Metadata)
- [ ] Descrição começa com "This skill should be used when..."
- [ ] Acessibilidade documentada: laranja apenas em texto ≥24px bold ou decorativo
- [ ] Pelo menos 4 stacks documentados (HTML, Tailwind, React, Next.js)
- [ ] Wave SVG reutilizável incluído nas referências
- [ ] Todos os componentes têm versão HTML + Tailwind + React
- [ ] evals.json tem 5 cenários realistas
- [ ] trigger-eval.json tem 15 queries (10 true, 5 false)
- [ ] YAML validation passa: `./scripts/validate-skill-yaml.sh skills/ava-web`
