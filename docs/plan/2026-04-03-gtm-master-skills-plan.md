# GTM Master Skills Plan — Campeão

> **Para Claude:** Use `executing-plans` para implementar este plano task-by-task. Respeite os Decision Gates.

**Objetivo:** Elevar o nível dos 3 skills GTM existentes e criar 19 skills novos cobrindo GTM de Produto + GTM de Consultoria Microsoft, com convenção de nomes uniforme, posicionamento como pilar central, e dois skills de workflow que orquestram cada track completo.

**Versão atual:** v1.22.0 (55 skills)
**Versão alvo:** v1.27.0 (74 skills)

---

## Abordagem de Criação de Skills

> **CRÍTICO para execução:** Todo skill novo neste plano deve ser criado usando o skill `skill-creator`.
> Não escrever SKILL.md manualmente. O skill-creator cuida de triggers, description optimization e evals.

**Para cada skill novo, o executor deve:**
1. Invocar `skill-creator` com o brief do skill (purpose, trigger phrases, frameworks, output format)
2. Deixar o skill-creator conduzir a entrevista, escrever o SKILL.md e criar os evals
3. Rodar os test cases e iterar até aprovação
4. Rodar description optimization para maximizar triggering accuracy
5. Validar com `./scripts/validate-skill-yaml.sh` e `./scripts/validate-skill-content.sh`

**Evals obrigatórios por skill (mínimo 3 test cases):**
- 1 caso de uso principal (trigger phrase canônica)
- 1 caso edge (input incompleto, contexto ambíguo)
- 1 caso negativo (should-not-trigger — prompt similar mas que não deve ativar este skill)

---

## Convenção de Nomes Aprovada

| Prefixo | Quando usar | Exemplo |
|---------|-------------|---------|
| `gtm-` | Skill compartilhado pelos dois tracks | `gtm-market-sizing` |
| `gtm-product-` | Exclusivo do GTM de Produto SaaS/PLG | `gtm-product-launch` |
| `gtm-consulting-` | Exclusivo do GTM de Consultoria Microsoft | `gtm-consulting-icp` |

## Tabela de Renaming Completa

| Nome antigo (plano anterior) | Nome novo (este plano) | Track | P do GTM |
|------------------------------|------------------------|-------|----------|
| `market-sizing` | **`gtm-market-sizing`** | Shared | Praça (segmentação) |
| `icp-designer` | **`gtm-icp-designer`** | Shared | Praça (targeting) |
| *(novo — gap crítico)* | **`gtm-positioning`** | Shared | Promoção (mensagem) |
| `startup-growth-strategist` *(existente — rebuild slim)* | **`gtm-product-growth`** | Product | Produto (viabilidade) |
| `gtm-launch-strategy` | **`gtm-product-launch`** | Product | Praça + Promoção |
| `pricing-strategy` | **`gtm-product-pricing`** | Product | Preço |
| `signal-based-outreach` | **`gtm-product-outreach`** | Product | Praça (canal outbound) |
| `gtm-demand-generation` | **`gtm-product-demand-gen`** | Product | Praça (canal inbound) |
| `gtm-account-expansion` | **`gtm-product-expansion`** | Product | Preço + Produto |
| *(novo)* | **`gtm-product-workflow`** | Product | Orquestrador |
| `microsoft-consulting-icp` | **`gtm-consulting-icp`** | Consulting | Praça (targeting) |
| *(novo — gap crítico)* | **`gtm-consulting-offer-design`** | Consulting | Produto (oferta) |
| *(novo — gap crítico)* | **`gtm-consulting-pricing`** | Consulting | Preço (T&M/fixed/outcome) |
| `consulting-gtm-designer` | **`gtm-consulting-designer`** | Consulting | Praça + Promoção |
| `microsoft-cosell-strategy` | **`gtm-consulting-cosell`** | Consulting | Praça (canal parceiro) |
| `presales-qualifier` | **`gtm-consulting-qualifier`** | Consulting | Praça (qualificação) |
| `executive-account-briefing` | **`gtm-consulting-briefing`** | Consulting | Promoção (engajamento) |
| `consulting-pursuit-governance` | **`gtm-consulting-pursuit`** | Consulting | Processo (execução) |
| `consulting-account-expansion` | **`gtm-consulting-expansion`** | Consulting | Preço + Produto |
| *(novo)* | **`gtm-consulting-workflow`** | Consulting | Orquestrador |

**Skills existentes que ficam com nome atual** (enriquecidos, não renomeados):
- `product-strategy` — escopo maior que GTM (posicionamento + estratégia)
- `abx-strategy` — escopo B2B genérico (signal scoring, pipeline)

**Fontes de referência usadas neste plano:**
- `GTM-Strategist/gtm-strategist-skills` — metodologia Maja Voje, 12 fases, testada em 1000+ empresas. Skills em `.claude/skills/` no branch `master`.
- `iamachilles/gtm-skills-os` — 151 skills, 28.500+ linhas, operacionalmente o mais profundo. Skills em `skills/<name>/SKILL.md` (flat).
- `kenny589/gtm-flywheel` — 15 skills de uma agência $7M ARR. Skills em diretórios raiz.

---

## Os Dois Workflows GTM

> Invocar `gtm-consulting-workflow` ou `gtm-product-workflow` para executar o workflow completo.
> Invocar skills individuais para etapas específicas.

---

### Workflow A — GTM de Consultoria Microsoft
**Invocar:** `/gtm-consulting-workflow`

```
╔══════════════════════════════════════════════════════════════════╗
║  CADÊNCIA TRIMESTRAL — "Praça, Produto, Preço e Mensagem"        ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  PRAÇA ─────────────────────────────────────────────────────     ║
║  gtm-market-sizing         → TAM/SAM/SOM do segmento alvo        ║
║  gtm-consulting-icp        → Quem perseguir? (tier × oferta)     ║
║                                                                  ║
║  PRODUTO ───────────────────────────────────────────────────     ║
║  gtm-consulting-offer-design → O QUÊ vender (escopo, entregáveis,║
║                                 metodologia, Hero Offers)        ║
║                                                                  ║
║  PREÇO ─────────────────────────────────────────────────────     ║
║  gtm-consulting-pricing    → T&M vs fixed-fee vs outcome-based    ║
║                               rate justification, pricing psychology║
║                                                                  ║
║  PROMOÇÃO ──────────────────────────────────────────────────     ║
║  gtm-positioning           → Posicionamento + Messaging House     ║
║                               + battle cards (SHARED)            ║
║  gtm-consulting-designer   → GTM Canvas + motion da oferta       ║
║  gtm-consulting-cosell     → Canal Microsoft + funding            ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  POR OPORTUNIDADE — "Vale e como governar este deal?"            ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  gtm-icp-designer          → Fit score desta conta (5 dimensões) ║
║         ↓                                                        ║
║  gtm-consulting-qualifier  → Vale investir pre-sales?            ║
║         ↓                                                        ║
║  gtm-consulting-briefing   → Preparar reunião C-level            ║
║         ↓                                                        ║
║  gtm-consulting-pursuit    → Governar pursuit ativo              ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  TRIMESTRAL — "Como crescer em contas existentes?"               ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  gtm-consulting-expansion  → Expansão de revenue na conta        ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝

Suporte (qualquer etapa):
  abx-strategy [ENRICHED]  → Signal scoring, PURE, multi-thread
  mckinsey-strategist       → Estrutura de problema complexo
```

---

### Workflow B — GTM de Produto SaaS/PLG
**Invocar:** `/gtm-product-workflow`

```
╔══════════════════════════════════════════════════════════════════╗
║  FOUNDATION — "Praça, Produto, Preço, Promoção — base certa"     ║
║  Cadência: Anual / ao lançar novo produto                        ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  PRAÇA (quem e onde) ───────────────────────────────────────     ║
║  gtm-market-sizing    → TAM/SAM/SOM com fontes e sensibilidade   ║
║  gtm-icp-designer     → ICP 5D, Anti-ICP, triggers, committee   ║
║                                                                  ║
║  PRODUTO (viabilidade) ────────────────────────────────────      ║
║  gtm-product-growth   → Unit economics, Bet Board, roadmap       ║
║  product-strategy [ENR.] → Beachhead, GTM motion, moat          ║
║                                                                  ║
║  PROMOÇÃO (mensagem) ──────────────────────────────────────      ║
║  gtm-positioning      → April Dunford, Messaging House,          ║
║                          battle cards, message-market fit (SHARED)║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  LAUNCH — "Como levar ao mercado?"                               ║
║  Cadência: Por produto ou feature major (trimestral)             ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  PREÇO ──────────────────────────────────────────────────────    ║
║  gtm-product-pricing  → Value metric, WTP Van Westendorp,        ║
║                          Gabor-Granger, 3 tiers, feature gates   ║
║                                                                  ║
║  PRAÇA + PROMOÇÃO ──────────────────────────────────────────     ║
║  gtm-product-launch   → 9 fases OPE Canvas → validação →         ║
║                          assets → channels → go-live             ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  GROWTH — "Como gerar e converter demanda?"                      ║
║  Cadência: Por canal (mensal)                                    ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  gtm-product-outreach  → Signal scoring, SPARK, 6-layer diag.   ║
║  gtm-product-demand-gen → Paid, organic, growth loops,           ║
║                            trial-to-paid, funnel optimization    ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║  EXPANSION — "Como crescer em contas existentes?"                ║
║  Cadência: Trimestral                                            ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  gtm-product-expansion → NRR, health scoring, upsell triggers,  ║
║                           churn prevention                       ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝

Suporte (qualquer etapa):
  abx-strategy [ENR.]   → Enterprise deals >$10K ACV, ciclo longo
  product-discovery      → Validação contínua de problema/solução
  product-delivery       → Staged rollout + métricas de launch
  product-leadership     → Portfolio-level GTM + board
```

---

### Skills Compartilhados (prefixo `gtm-`)

```
  gtm-market-sizing   →  Usado em AMBOS os workflows
  gtm-icp-designer    →  Usado em AMBOS os workflows
  abx-strategy        →  Enterprise deals em qualquer contexto
```

---

## Decision Gates

1. ✅ Plano aprovado (com decomposição em market-sizing + icp-designer + startup-growth-strategist slim)
2. ⏳ User aprova Track A após Batch 1-3 (3 skills fixados + 2 novos: market-sizing, icp-designer)
3. ⏳ User aprova Track B Batch 1 (gtm-launch-strategy + pricing-strategy) antes de continuar
4. ⏳ User aprova Track B Batch 2 (signal-based-outreach + demand + expansion) antes de continuar
5. ⏳ User aprova Track C Batch 1 (microsoft-consulting-icp + consulting-gtm-designer)
6. ⏳ User aprova Track C Batch 2 (5 skills restantes)
7. ⏳ Version bump final (v1.25.0, 69 skills)

---

## Frontmatter Rules (CRÍTICO — não errar)

```yaml
---
name: kebab-case-name
description: This skill should be used when... (single line, sem quotes desnecessárias)
license: MIT
---
```

**PROIBIDO em SKILL.md:** `version`, `author`, `platforms`, `category`, `tags`, `risk`, `created`, `updated`
**Esses campos vão em README.md** na tabela `## Metadata`.

---

## TRACK A — Corrigir Skills Existentes + Criar Novos Skills de Foundation

### Batch 1: Decompor `startup-growth-strategist` em 3 skills focados

**Diagnóstico original:** Stub disfarçado de skill. Phase 5 vazia. Sem ICP. Sem templates. Escrito como role description ("You are..."). Cobria 5 fases que pertencem a 3 skills distintos.

**Decisão de design:**
- `market-sizing` [NOVO] — TAM/SAM/SOM reutilizável por ambos os workflows
- `icp-designer` [NOVO] — ICP scoring matrix, buying triggers, Anti-ICP, buying committee — o skill mais reutilizável de todo o GTM
- `startup-growth-strategist` [SLIM] — refocado em unit economics + Bet Board + growth roadmap

**O que foi REMOVIDO do startup-growth-strategist e movido:**
- Phase 1 (TAM/SAM/SOM) → `market-sizing`
- Phase 2 (ICP Definition) → `icp-designer`
- Phase 4 GTM motion selection → já existe em `product-strategy` (enriquecido no Batch 2)

---

#### Task A1.1 — Criar `skills/market-sizing/SKILL.md`

**Arquivo:** `skills/market-sizing/SKILL.md`
**Ação:** Criar novo skill.

```markdown
---
name: market-sizing
description: This skill should be used when the user needs to calculate or validate market size for a product, service, or business opportunity. Use when building TAM/SAM/SOM analysis, validating market assumptions before investing in GTM, sizing a new segment or geography, or preparing market opportunity data for fundraising or strategic planning.
license: MIT
---

# Market Sizing

Build a rigorous, sourced market size model. Every number must trace to a calculation or a cited source — no unsourced estimates.

## When to Use

- Validating whether a market opportunity justifies investment
- Building TAM/SAM/SOM for a pitch deck or strategic plan
- Sizing a new segment before launching GTM motion
- Triangulating whether a market is big enough to build a company
- Preparing market opportunity data for board or investor review

## Progress Tracking

```
[████████░░░░░░░░░░░░] 40% — Phase 1/3: TAM (Total Addressable Market)
[████████████░░░░░░░░] 67% — Phase 2/3: SAM (Serviceable Addressable Market)
[████████████████████] 100% — Phase 3/3: SOM + Sensitivity + Output
```

## Workflow

### Phase 1: TAM — Total Addressable Market

**Step 1 — Load context.** Identify: product description, target customer type, geography, and price point hypothesis. If any are missing, ask before proceeding.

**Step 2 — Research industry data.** Run at least 2 independent searches:
- "[market name] total addressable market [year]"
- "[vertical] market size analyst report"
- "[segment] spending estimate [source]"

Use named sources: Gartner, IDC, Forrester, Grand View Research, Statista, or primary research from industry associations.

**Step 3 — Segment the market.** Break into 3-5 sub-segments by industry, company size, geography, or use case. Document boundary rationale for each segment (why it's in, why adjacent segments are out).

**Step 4 — Calculate TAM two ways and triangulate:**

| Method | Formula | Your Calculation |
|--------|---------|-----------------|
| **Bottom-up** | Total companies matching profile × ACV hypothesis | = $X |
| **Top-down** | Industry market size × relevant segment % | = $Y |
| **Triangulated TAM** | Average or weighted average of both | = $Z |

Document source and confidence level for each input.

**Step 5 — "Why now?" argument.** What market timing factor creates urgency?
- Regulatory change creating demand
- Technology shift making the solution possible
- Macro trend (AI, remote work, compliance)
- Competitive gap opening

---

### Phase 2: SAM — Serviceable Addressable Market

Apply realistic filters to TAM based on your actual ability to reach and serve the market.

**SAM filters — apply each and document the reduction:**

| Filter | What It Removes | SAM After Filter |
|--------|----------------|-----------------|
| **Geography** | Segments you can't serve today (language, regulation, no team) | |
| **Channel capacity** | Segments requiring a channel you don't have | |
| **Compliance / certification** | Segments requiring credentials you don't hold | |
| **Minimum deal size** | Accounts too small to be economical | |
| **Technology requirement** | Accounts without required tech stack | |

SAM = TAM × (% remaining after all filters)

**Validation check:** SAM should be 10-30% of TAM for most startups. If SAM > 50% of TAM, your filters are too loose. If SAM < 5% of TAM, your go-to-market is too narrow to build a business.

---

### Phase 3: SOM + Sensitivity + Output Table

**SOM — Serviceable Obtainable Market (12-month realistic capture):**

| Stage | Capture Rate | Rationale |
|-------|-------------|-----------|
| Pre-PMF / 0→1 | 0.5–2% of SAM | Building initial distribution |
| Post-PMF / 1→10 | 2–5% of SAM | Repeatable motion established |
| Scaling / 10→100 | 5–15% of SAM | Market leadership established |

Choose the capture rate that matches your current stage. Document the assumption.

**Sensitivity check — what if inputs are wrong?**

| Scenario | ACV Change | Segment Size Change | SOM Result |
|----------|-----------|--------------------|-----------| 
| Bear | -30% | -40% | |
| Base | — | — | |
| Bull | +20% | +15% | |

**Final output table:**

| Metric | Value | Method | Key Sources |
|--------|-------|--------|------------|
| TAM | $XM | Bottom-up + top-down triangulation | [Source 1], [Source 2] |
| SAM | $XM | TAM × [filter %] | [Assumption stated] |
| SOM (12mo) | $XM | SAM × [X% capture] | [Benchmark cited] |
| ACV Hypothesis | $X | [Comparable pricing reference] | [Source] |
| Key Assumption | [State the single most important assumption] | | |

## Critical Rules

- NEVER present a TAM/SAM/SOM without at least 2 independent sources for the TAM figure
- NEVER skip the sensitivity check — a single market size number with no range is false precision
- ALWAYS state the ACV hypothesis explicitly — market size is meaningless without it
- NEVER use analyst estimates without checking the methodology (top-down reports often overstate by 3-5×)
- ALWAYS apply SAM filters — raw TAM is not your market
- ALWAYS state the "Why Now?" — market size alone doesn't justify investment

## Example Usage

1. "Size the market for an AI-powered contract review tool targeting in-house legal teams at mid-market US companies ($500M-$5B revenue)"
2. "Build TAM/SAM/SOM for a SaaS expense management tool — we're targeting SMBs in Brazil with 50-500 employees"
3. "Our investor is asking for market sizing — we sell DevOps automation to enterprise engineering teams"
4. "We want to expand to Europe — size the addressable market for our current product in UK + Germany + France"
5. "Is the market big enough to justify building this? We're targeting HR directors at mid-market companies with a $12K/year ACV product"
```

---

#### Task A1.2 — Criar `skills/market-sizing/README.md`

**Arquivo:** `skills/market-sizing/README.md`

```markdown
# Market Sizing

Rigorous TAM/SAM/SOM analysis with sourced calculations, sensitivity testing, and a structured output table.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Market Analysis |
| Tags | market-sizing, tam, sam, som, market-analysis, gtm, fundraising |
| Risk | Low |

## Overview

Used in both the Product GTM and Consulting GTM workflows as a foundation step. Every number must be sourced or explicitly stated as an assumption.

## Where This Fits

- **Product GTM:** First step in Foundation phase (before icp-designer)
- **Consulting GTM:** Optional input for consulting-gtm-designer (market opportunity sizing)
- **Fundraising:** Core input for pitch decks and strategic plans
```

---

#### Task A1.3 — Criar `skills/icp-designer/SKILL.md`

**Arquivo:** `skills/icp-designer/SKILL.md`
**Ação:** Criar novo skill.

```markdown
---
name: icp-designer
description: This skill should be used when the user needs to define, score, or refine an Ideal Customer Profile. Use when building ICP scoring matrices, mapping buying committees, identifying buying triggers and intent signals, defining Anti-ICP disqualifiers, or refreshing an ICP based on closed-won/lost data.
license: MIT
---

# ICP Designer

Define who to pursue with precision. A rigorous ICP system covering firmographic scoring, buying triggers, intent signals, Anti-ICP binary disqualifiers, and buying committee mapping. Used in both product GTM and B2B sales motions.

## When to Use

- Defining ICP for a new product or entering a new segment
- Refreshing ICP after accumulating 10+ customers (pattern recognition)
- Qualifying whether a specific account is worth pursuing
- Building a scored target list for outbound or ABM
- Training sales or SDR teams on qualification standards
- Reviewing pipeline quality against ICP criteria

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 25% — Phase 1/4: Firmographic & Scoring Matrix
[████████░░░░░░░░░░░░] 50% — Phase 2/4: Buying Triggers & Intent Signals
[████████████░░░░░░░░] 75% — Phase 3/4: Anti-ICP & Competitive Landscape
[████████████████████] 100% — Phase 4/4: Buying Committee Map
```

## Workflow

### Phase 1: Firmographic Profile & Scoring Matrix

**Step 1 — If 10+ customers exist:** Analyze best customers (highest LTV, best retention, fastest time-to-value, most referrals). Look for patterns across 70%+ of them — those patterns become positive scoring signals. Analyze worst customers (churned, support-heavy, low NPS) — patterns present in <30% of worst customers become disqualifiers.

**Step 2 — If fewer than 10 customers:** Build a hypothesis ICP from: founder intuition + problem interviews + comparable products' public case studies. Label it explicitly as "Hypothesis ICP — validate with first 10 customers."

**Step 3 — Firmographic profile.** Define precisely:
- Company size: employee range AND revenue range (both matter)
- Industry / vertical (specific, not "B2B" or "tech")
- Geography (countries/regions you can actually serve today)
- Tech stack signals (what tools do they already use?)
- Organizational signals (team structure, budget owner title)

**Step 4 — ICP Scoring Matrix (5 dimensions, 0-20 each, max 100).**

Score each prospect account before investing sales time:

| Dimension | What to Evaluate | Score 0-20 |
|-----------|-----------------|-----------|
| **Firmographic Fit** | Company size, industry, revenue, geography vs. ideal profile | 0=no match, 20=exact match |
| **Technographic Fit** | Tech stack compatibility, tools used, cloud provider, integrations | 0=incompatible, 20=ideal stack |
| **Buying Intent** | Signal recency + directness + frequency (see Phase 2) | 0=no signal, 20=direct active signal |
| **Persona Access** | Seniority of reachable contact, proximity to economic buyer | 0=no access, 20=economic buyer reachable |
| **Engagement Readiness** | Prior interaction, champion identified, relationship warm | 0=cold, 20=champion active |

**Tier thresholds:**
| Score | Tier | Action |
|-------|------|--------|
| 85-100 | Bullseye | Full investment — personalized 1:1, senior rep, custom approach |
| 70-84 | Strong Fit | Personalized sequence, priority outreach |
| 55-69 | Good Fit | Standard sequence, scaled personalization |
| 40-54 | Stretch | Only pursue if inbound signal present |
| <40 | Disqualified | Do not invest sales time |

**Quarterly validation:** Compare scoring results against closed-won/lost. If Bullseye accounts aren't converting, adjust dimension weights.

---

### Phase 2: Buying Triggers & Intent Signals

These transform ICP from a description into a targeting system.

**Buying Triggers — events that move a prospect from "someday" to "this quarter":**

List 5+ triggers specific to your product. Examples to adapt:
- Leadership change: new exec with a mandate to fix the problem you solve
- Funding event: Series A/B → budget to invest in infrastructure
- Regulatory deadline: compliance forcing function with a date
- Competitive threat: competitor just launched or improved
- Organizational event: merger, reorg, IPO preparation, geographic expansion
- Contract expiring: incumbent vendor relationship ending
- Growth milestone: company crossed a headcount or revenue threshold

**Intent Signals — observable indicators of active buying:**

| Signal Category | Examples | Strength |
|----------------|---------|---------|
| **Direct buying** | Pricing page visit, demo request, free trial sign-up | High (×3) |
| **Research signals** | Downloading relevant whitepapers, attending competitor webinars | Medium (×2) |
| **Hiring signals** | Job posting for roles that use your product | Medium (×2) |
| **Technographic signals** | Installing complementary or competing tools | Medium (×2) |
| **Social signals** | LinkedIn post about the problem you solve, conference speaking | Low (×1) |

**Signal scoring:**
```
Signal Score = Base × Recency Multiplier × Strength Multiplier
```
- Recency: ×3 if <7 days, ×2 if 7-30 days, ×1 if 30-90 days, ×0 if >90 days
- Staleness rule: any signal older than 4 months is discarded

---

### Phase 3: Anti-ICP & Competitive Landscape

**Anti-ICP — binary disqualifiers (hard stops regardless of other scores):**

Define 5+ conditions that immediately disqualify an account. Examples:
- No named budget owner reachable through any channel
- Active contract with direct competitor at full capacity
- No adoption of required technology platform
- RFP-only relationship with no prior engagement
- Purchase decision cycle >18 months
- Single-vendor mandate to a competitor
- No executive sponsor identified after 2 discovery conversations

> **Rule:** Anti-ICP without disqualifiers is just a wish list. Make them binary: if true, stop. No exceptions.

**Competitive landscape — always include "do nothing":**

| Alternative | Why they choose it | Why they leave it | Your wedge |
|-------------|-------------------|-------------------|-----------| 
| Competitor A | [reason] | [weakness] | [angle] |
| Competitor B | [reason] | [weakness] | [angle] |
| Internal build | Control, no vendor dependency | Maintenance cost, time to build | [angle] |
| **Do nothing** | Inertia, switching cost, budget freeze | [Trigger that breaks inertia] | [Urgency play] |

---

### Phase 4: Buying Committee Map

Even for SMB deals, document the decision chain before investing pre-sales hours.

**Map using Miller Heiman roles:**

| Role | Definition | Title (typical) | Engagement Status | Next Action |
|------|-----------|----------------|------------------|------------|
| **Economic Buyer** | Final financial authority — can say yes alone | CFO, CEO, VP | Cold / Warm / Engaged | |
| **User Buyer** | Day-to-day operational impact — lives with the decision | Director, Manager | Cold / Warm / Engaged | |
| **Technical Buyer** | Evaluates specs, risk, integration — can say no | IT, Security, Architecture | Cold / Warm / Engaged | |
| **Coach / Champion** | Internal advocate with political will | Anyone with influence | Cold / Warm / Engaged | |

**Champion identification rule:** No named Champion = no pre-sales investment beyond the initial discovery call. A Champion must be able to answer: "What happens inside the company after our meeting?"

**Multi-thread rule:** Any deal > $15K ACV requires 3+ unique stakeholders engaged before proposal. Single-threaded deals lose when the contact leaves or changes priorities.

## Critical Rules

- NEVER skip Anti-ICP definition — without it, any sales team will game the ICP scoring to justify pursuing bad accounts
- ALWAYS map the buying committee before investing significant pre-sales time
- NEVER treat ICP as permanent — refresh after every 10 new customers or quarterly, whichever comes first
- ALWAYS label hypothesis ICPs clearly — treating guesses as facts misleads the team
- NEVER conflate User Buyer and Economic Buyer — they have different success metrics and objections
- ALWAYS include "do nothing" in the competitive landscape
- NEVER require a Champion to be senior — Champions need influence, not title

## Example Usage

1. "Define ICP for our SaaS expense management tool — we have 15 customers, help me find the pattern"
2. "Score this account: [Company], 800 employees, SaaS company, CTO just posted a job for a DevOps engineer, we know the Engineering Director"
3. "Build the Anti-ICP for our AI writing tool — we keep closing accounts that churn in month 3"
4. "Map the buying committee for a $200K consulting engagement at a manufacturing company"
5. "Refresh our ICP — we've closed 25 deals and I want to know what our best customers have in common"
```

---

#### Task A1.4 — Criar `skills/icp-designer/README.md`

**Arquivo:** `skills/icp-designer/README.md`

```markdown
# ICP Designer

Rigorous Ideal Customer Profile system — 5-dimension scoring matrix, buying triggers, intent signals, Anti-ICP binary disqualifiers, and buying committee mapping.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Sales |
| Tags | icp, ideal-customer-profile, scoring, abm, anti-icp, buying-committee, qualification |
| Risk | Low |

## Overview

The most reusable skill in the GTM suite. Used in both the Product GTM workflow (Foundation phase) and Consulting GTM workflow (per-opportunity qualification). Based on gtm-skills-os icp-model-creator and ColdIQ icp-matrix-builder methodologies.

## Where This Fits

- **Product GTM:** Second step in Foundation phase (after market-sizing, before startup-growth-strategist)
- **Consulting GTM:** Per-opportunity qualification step (before presales-qualifier)
- **Outbound:** Input for signal-based-outreach ICP matrix
- **ABM:** Foundation for abx-strategy PURE scoring
```

---

#### Task A1.5 — Reescrever `skills/startup-growth-strategist/SKILL.md` (versão SLIM)

**Arquivo:** `skills/startup-growth-strategist/SKILL.md`
**Ação:** SUBSTITUIR completamente pelo conteúdo abaixo. Focado exclusivamente em unit economics + financial modeling + Bet Board + growth roadmap.

```markdown
---
name: startup-growth-strategist
description: This skill should be used when the user needs to model unit economics, validate business viability, design a hypothesis-driven Bet Board, or build a growth roadmap for an early-stage or scaling product. Run after market-sizing and icp-designer to complete the 0→1 foundation analysis.
license: MIT
---

# Startup Growth Strategist

Model unit economics, validate business viability, and design a hypothesis-driven growth roadmap. Run after `market-sizing` and `icp-designer` to complete the 0→1 foundation. Answers: "Does the business model close? What bets should we place?"

## When to Use

- Validating business model viability before scaling
- Building LTV/CAC model for investors or internal planning
- Designing a Bet Board for hypothesis-driven growth
- Modeling Bull/Base/Bear financial scenarios
- Building a quarterly growth roadmap with kill conditions
- Identifying which assumptions pose the greatest risk

## Progress Tracking

```
[████████░░░░░░░░░░░░] 33% — Phase 1/3: Unit Economics & Financial Model
[████████████░░░░░░░░] 67% — Phase 2/3: Assumption Ranking & Bet Board
[████████████████████] 100% — Phase 3/3: Growth Roadmap
```

## Workflow

### Phase 1: Unit Economics & Financial Model

**Build the unit economics table for each pricing scenario:**

| Metric | Formula | Value | Assumption / Source |
|--------|---------|-------|---------------------|
| **ACV** | Annual Contract Value | $X | Pricing hypothesis |
| **CAC** | (Sales + Marketing spend) ÷ New customers acquired | $X | Budget ÷ capacity |
| **Gross Margin** | (Revenue − COGS) ÷ Revenue | X% | Target: >70% for SaaS |
| **LTV** | ACV × Gross Margin × Avg. Retention years | $X | |
| **LTV:CAC Ratio** | LTV ÷ CAC | X:1 | Target: >3:1 |
| **Payback Period** | CAC ÷ (ACV × Gross Margin ÷ 12) | X months | Target: <18 months |
| **Churn Rate** | Customers lost ÷ Starting customers | X%/mo | |
| **Net Revenue Retention** | (Starting MRR + Expansion − Churn − Contraction) ÷ Starting MRR | X% | Target: >100% |
| **Burn Multiple** | Net Burn ÷ Net New ARR | X | Target: <1.5 efficient, <1 excellent |
| **Rule of 40** | ARR Growth % + EBITDA Margin % | X | Target: >40 for scaling |

**CAC by channel — break down the blended CAC:**

| Channel | Monthly Spend | Customers Acquired | CAC | Notes |
|---------|--------------|-------------------|-----|-------|
| Outbound | $X | X | $X | |
| Paid | $X | X | $X | |
| Inbound / SEO | $X | X | $X | |
| Referral | $X | X | $X | |
| **Blended** | $X | X | **$X** | |

**Three scenarios — always present all three:**

| Scenario | Key Assumption | ACV | CAC | LTV:CAC | Runway Months |
|----------|---------------|-----|-----|---------|---------------|
| Bear | [Worst case input] | | | | |
| Base | [Most likely] | | | | |
| Bull | [Best case] | | | | |

**Cohort revenue projection (12 months):**
- New customers per month by channel (from GTM motion)
- Monthly churn applied to existing base
- Expansion revenue added (NRR × starting MRR)
- Resulting MRR curve with cumulative ARR

**Viability check:** If LTV:CAC < 1 in Base scenario, the business model doesn't close. Fix pricing, cut CAC, or change segment before investing in growth.

---

### Phase 2: Assumption Ranking & Bet Board

**Assumption Ranking Matrix — before placing bets, rank what you're betting on:**

List every assumption the business depends on. Rank by: (a) impact if wrong × (b) current confidence.

| Assumption | If Wrong, Consequence | Confidence (1-5) | Test Priority |
|-----------|----------------------|-----------------|--------------|
| [Most dangerous] | Business model collapses | 1 = low confidence | Test FIRST |
| [Second] | Miss ARR target by 50% | 3 | Test Q1 |
| [Third] | Growth is slower | 4 | Monitor |

**Rule:** Validate the riskiest assumption first. Don't polish secondary features while the core hypothesis is unproven.

**Bet Board — structure every growth initiative as a bet:**

| Bet | Hypothesis | Metric | Target | Timebox | Kill Condition | Scale Condition |
|-----|-----------|--------|--------|---------|----------------|----------------|
| [Name] | "If we do X, we expect Y" | [KPI] | [#] | [weeks] | Metric < X after Y weeks | Metric > X |

**Bet categories:**
- **Value bets:** New capabilities that solve customer problems (test with discovery)
- **Growth bets:** Acquisition, activation, expansion mechanics
- **Moat bets:** Building defensibility (data, switching costs, network effects)
- **Efficiency bets:** Reducing CAC or COGS while maintaining quality

**Portfolio balance for bets:**
- 70% core (proven, incremental improvement)
- 20% adjacent (related problem, moderate risk)
- 10% transformational (new category, high risk, high reward)

---

### Phase 3: Growth Roadmap

**Quarterly roadmap structure:**

| Quarter | Focus | Active Bets | North Star Target | Kill Gate |
|---------|-------|------------|------------------|----------|
| Q1 | [Beachhead + PMF signal] | [2-3 bets] | [>40% "very disappointed"] | [If not hit, pivot] |
| Q2 | [First repeatable motion] | [2-3 bets] | [CAC < $X, pipeline self-generating] | |
| Q3 | [First expansion signal] | [2-3 bets] | [NRR > 100%] | |
| Q4 | [Scale or pivot decision] | [Decision gate] | [Rule of 40 > X] | |

**Growth loops — identify and document any compounding mechanism:**
- **Viral loop:** User action → new user (measure: K-factor > 1 = compounding)
- **Content loop:** Usage → user-generated content → SEO/social → new users
- **Ecosystem loop:** Integration → partner visibility → referrals
- **Data loop:** More customers → better model → better product → more customers

**North Star Metric:** Pick ONE metric that best captures value delivered to customers AND correlates with long-term revenue. Examples:
- Notion: "docs created"
- Slack: "messages sent"
- Stripe: "payment volume processed"

Document: what is our North Star? Why does improving it lead to revenue?

## Critical Rules

- NEVER present financial projections without stating the most important assumption explicitly
- ALWAYS include Bear, Base, and Bull scenarios — never a single "best case"
- NEVER place a bet without defining the kill condition — open-ended bets waste money
- ALWAYS validate the riskiest assumption first — sequence matters
- NEVER conflate blended CAC with channel CAC — they lead to different decisions
- ALWAYS check LTV:CAC in Bear scenario before investing in growth

## Example Usage

1. "Model unit economics: $299/mo ACV, CAC estimated at $800, gross margin 85%, avg retention 24 months"
2. "Build a Bet Board for our Q1-Q2 growth roadmap — we just hit PMF and want to find our first repeatable motion"
3. "Our LTV:CAC is 1.8 — help me identify what to fix: pricing, CAC, or churn"
4. "Design Bull/Base/Bear scenarios for our Series A pitch — we're projecting $2M ARR in 18 months"
5. "Rank our growth assumptions by risk and help me design experiments to validate the top 3"
```

---

#### Task A1.6 — Atualizar `skills/startup-growth-strategist/README.md`

Atualizar tabela Metadata:
- `Version` → `2.0.0`
- `Updated` → `2026-04-03`
- Atualizar Overview para refletir o escopo slim (unit economics + Bet Board)

---

#### Task A1.7 — Validar os 3 skills novos/revisados

```bash
./scripts/validate-skill-yaml.sh skills/market-sizing
./scripts/validate-skill-content.sh skills/market-sizing
./scripts/validate-skill-yaml.sh skills/icp-designer
./scripts/validate-skill-content.sh skills/icp-designer
./scripts/validate-skill-yaml.sh skills/startup-growth-strategist
./scripts/validate-skill-content.sh skills/startup-growth-strategist
```

---

---

### Batch 2: Enriquecer `product-strategy`

**Arquivo:** `skills/startup-growth-strategist/SKILL.md`
**Ação:** SUBSTITUIR completamente o conteúdo atual pelo abaixo.

```markdown
---
name: startup-growth-strategist
description: This skill should be used when the user needs to validate a business opportunity, build market sizing (TAM/SAM/SOM), define ICP with scoring, model unit economics, design GTM motion, or create a growth roadmap for an early-stage or scaling product.
license: MIT
---

# Startup Growth Strategist

Validate markets, score ICPs, model economics, and design GTM motions with data-driven precision. Built for 0→1 founders and growth leaders preparing to scale.

## When to Use

- Validating a new business idea or market entry
- Building TAM/SAM/SOM analysis for fundraising or planning
- Defining ICP before investing in sales or marketing
- Modeling unit economics (LTV/CAC, payback, Rule of 40)
- Choosing GTM motion (PLG vs SLG vs Partner vs Hybrid)
- Designing a growth roadmap with hypothesis-driven bets
- Preparing a competitive analysis with differentiation strategy

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 20% — Phase 1/5: Market Sizing (TAM/SAM/SOM)
[████████░░░░░░░░░░░░] 40% — Phase 2/5: ICP Definition & Scoring
[████████████░░░░░░░░] 60% — Phase 3/5: Unit Economics & Financial Model
[████████████████░░░░] 80% — Phase 4/5: GTM Motion & Competitive Strategy
[████████████████████] 100% — Phase 5/5: Growth Roadmap & Bet Board
```

## Workflow

### Phase 1: Market Sizing (TAM/SAM/SOM)

Build a sourced, segmented market model. Every number must trace to a source or a stated assumption.

**Step 1 — Load context.** Identify: product description, target customer type, geography, price point hypothesis. If missing, ask before proceeding.

**Step 2 — Research industry data.** Run WebSearch for: "[market] size 2024 report", "[segment] TAM analyst estimate", "[vertical] total addressable market". Use at least 2 independent sources.

**Step 3 — Define segments.** Break the market into 3-5 segments by industry, company size, geography, or use case. Justify each segment boundary.

**Step 4 — TAM calculation.**
```
TAM = Total companies matching profile × Annual Contract Value hypothesis
```
Document source for both inputs. Triangulate with top-down industry reports.

**Step 5 — SAM filter.** Apply realistic constraints: geography, sales channel capacity, compliance requirements, language. SAM = TAM × filter rationale.

**Step 6 — SOM target.** For early-stage (0→1): 1-5% of SAM in 12 months is realistic. Document the capture rate assumption.

**Step 7 — Sensitivity check.** What if ACV is 30% lower? What if addressable segment is half? Show the range, not just base case.

**Step 8 — Output table.**

| Metric | Value | Calculation Method | Sources |
|--------|-------|-------------------|---------|
| TAM | $XM | [Companies × ACV] | [Source 1, Source 2] |
| SAM | $XM | [Filter: geography + channel] | [Assumption stated] |
| SOM (12mo) | $XM | [X% capture rate] | [Benchmark cited] |
| ACV Hypothesis | $X | [Comparable pricing] | [Source] |

**Why Now?** — State the market timing argument: regulatory change, technology shift, buyer behavior change, or competitive gap that creates the window.

---

### Phase 2: ICP Definition & Scoring

Define who to pursue with precision. A vague ICP wastes every dollar of sales and marketing.

**Step 1 — Firmographic profile.** Define:
- Company size (employees, revenue range)
- Industry / vertical (specific, not "B2B")
- Geography (realistic given team)
- Tech stack signals (what do they already use?)
- Organizational structure signals (team size, reporting lines)

**Step 2 — Buyer personas.** For each buying role (Economic Buyer, User Buyer, Technical Buyer, Coach/Champion):
- Title and seniority
- Primary KPIs they're measured on
- Top 3 pain points in their own language
- Success definition (what does winning look like for them?)
- Preferred channels (LinkedIn, email, events, referral)
- Most common objections

**Step 3 — Buying triggers.** List 5+ events that create urgency to buy NOW:
- Leadership change (new CTO, new VP of Ops)
- Funding round (runway to invest)
- Regulatory deadline (compliance mandate)
- Competitive threat (competitor just launched)
- Organizational event (merger, reorg, scale-up)

**Step 4 — Intent signals.** Observable indicators of active buying:
- Hiring signals (job postings for related roles)
- Tech signals (installing complementary tools)
- Content signals (downloading relevant whitepapers)
- Event signals (attending competitor events, speaking about the problem)
- Financial signals (budget approval cycle timing)

**Step 5 — ICP Scoring Matrix (5 dimensions, 1-5 each, max 25).**

| Dimension | 1 (Weak) | 3 (Good) | 5 (Bullseye) | Score |
|-----------|----------|----------|--------------|-------|
| **Firmographic Fit** | Wrong size/industry | Partial match | Exact match | |
| **Technographic Fit** | Incompatible stack | Adjacent tools | Ideal stack | |
| **Buying Intent** | No signal | Some trigger present | Active signal + timing | |
| **Persona Access** | No contact | IC-level only | Economic buyer reachable | |
| **Engagement Readiness** | No relationship | Warm intro possible | Champion identified | |

**Tier thresholds:**
- 22-25: Bullseye — full investment, custom approach
- 17-21: Strong Fit — proactive outreach, personalized
- 12-16: Good Fit — targeted campaign, scaled personalization
- 7-11: Stretch — nurture only, no sales investment
- <7: Disqualified

**Step 6 — Anti-ICP (binary disqualifiers).** List 5+ hard stops — if ANY is true, disqualify immediately regardless of other scores:
- [Example: No dedicated budget owner reachable]
- [Example: Competitive lock-in contract active]
- [Example: No Microsoft/Azure/relevant tech adoption]
- [Example: RFP-only with no prior relationship]
- [Example: Decision cycle >18 months]

**Step 7 — Competitive landscape.** Map 3-5 alternatives the buyer considers. Include "do nothing" as an explicit row.

| Alternative | Why they choose it | Why they leave it | Your wedge |
|-------------|-------------------|-------------------|-----------|
| Competitor A | [reason] | [reason] | [angle] |
| Competitor B | [reason] | [reason] | [angle] |
| Do nothing | [inertia/cost] | [trigger to act] | [urgency play] |

**Step 8 — Buying committee map.** Even for SMB, document the decision chain:

| Role | Name/Title | Influence | Status | Next Action |
|------|-----------|-----------|--------|-------------|
| Economic Buyer | | Final authority | Cold/Warm/Engaged | |
| User Buyer | | Day-to-day impact | Cold/Warm/Engaged | |
| Technical Buyer | | Spec/risk eval | Cold/Warm/Engaged | |
| Coach | | Internal champion | Cold/Warm/Engaged | |

---

### Phase 3: Unit Economics & Financial Model

Audit viability before investing in growth. No metrics = no decisions.

**Build this table for each pricing scenario:**

| Metric | Formula | Value | Source/Assumption |
|--------|---------|-------|-------------------|
| **ACV** | Annual Contract Value | $X | Pricing hypothesis |
| **CAC** | Sales + Marketing cost ÷ New customers | $X | Budget ÷ capacity |
| **LTV** | ACV × Gross Margin × Avg. Retention (yrs) | $X | |
| **LTV:CAC Ratio** | LTV ÷ CAC | X:1 | Target: >3:1 |
| **Payback Period** | CAC ÷ (ACV × Gross Margin / 12) | X months | Target: <18 months |
| **Gross Margin** | (Revenue − COGS) ÷ Revenue | X% | Target: >70% for SaaS |
| **Burn Multiple** | Net Burn ÷ Net New ARR | X | Target: <1.5 |
| **Rule of 40** | ARR Growth % + Profit Margin % | X | Target: >40 |

**Three scenarios — always present all three:**

| Scenario | Key Assumption | ACV | CAC | LTV:CAC | Runway Implication |
|----------|---------------|-----|-----|---------|-------------------|
| Bear | [worst case] | | | | |
| Base | [most likely] | | | | |
| Bull | [best case] | | | | |

**Cohort projection (12-24 months):**
- Monthly new customers added (by channel)
- Monthly churn rate applied to existing base
- Expansion revenue from upsell (Net Revenue Retention target)
- Resulting MRR/ARR curve

---

### Phase 4: GTM Motion & Competitive Differentiation

**GTM Motion Selection — score each factor:**

| Factor | PLG Signal | SLG Signal | Your Score |
|--------|-----------|-----------|-----------|
| ACV | <$10K/yr | >$10K/yr | |
| Buyer | End user | Economic buyer | |
| Time to Value | Minutes/hours | Days/weeks | |
| Integration complexity | Low | High | |
| Trust required | Self-serve | Relationship | |
| Support needed | Docs + chatbot | CSM + onboarding | |

**GTM Triangle — three components must tell a unified story:**

**1. Positioning (Market Change → Thesis → Proof):**
```
Market Change: [Observable trend that makes the old way obsolete]
Thesis:        [Our opinionated take on what this means]
Proof:         [Evidence that our thesis is already true]
```

**2. ICP Summary:** Beachhead segment first. Narrowest definition where you can win before expanding.

> **Beachhead Rule:** Pick the single narrowest segment where: (a) you have 3+ reference customers, (b) the pain is highest, (c) you can dominate the conversation. Expand only after winning here.

**3. Channel Effort/Impact Matrix:**

| Channel | Effort (1-5) | Impact (1-5) | Priority | Owner |
|---------|-------------|-------------|---------|-------|
| Founder-led outbound | 2 | 5 | 🔴 First | Founder |
| Content / SEO | 3 | 4 | 🟡 Build | Marketing |
| Paid social | 4 | 3 | 🟡 Test | Marketing |
| Product virality | 3 | 5 | 🔴 Design in | Product |
| Partnerships | 4 | 4 | 🟢 6 months | BD |
| Events | 5 | 3 | 🟢 Later | Sales |

**Competitive Differentiation:**
- Map direct competitors, indirect competitors, and "do nothing" (already done in Phase 2 — reference it here)
- Blue Ocean check: Is there an uncontested segment where competition is irrelevant?
- Alternatives Page strategy: Build content that captures buyers comparing alternatives

---

### Phase 5: Growth Roadmap & Bet Board

**Assumption Ranking Matrix — prioritize experiments by risk:**

| Assumption | If Wrong, Impact | Confidence | Priority to Test |
|-----------|-----------------|------------|-----------------|
| [Riskiest assumption] | Kill the business | Low | Test NOW |
| [Second assumption] | Miss ARR target | Medium | Test Q1 |
| [Third assumption] | Slower growth | High | Monitor |

**GTM Bet Board:**

| Bet | Hypothesis | Metric | Target | Timebox | Kill Condition | Scale Condition |
|-----|-----------|--------|--------|---------|----------------|----------------|
| [Name] | "If we do X, we expect Y" | [KPI] | [#] | [weeks] | [If metric < X after Y weeks] | [If metric > X] |

**Growth Roadmap Structure:**

| Quarter | Focus | Key Bets | Success Criteria |
|---------|-------|----------|-----------------|
| Q1 | Beachhead validation | [2-3 bets] | [PMF signal: >40% "very disappointed"] |
| Q2 | First repeatable motion | [2-3 bets] | [CAC < $X, pipeline self-generating] |
| Q3 | First expansion signal | [2-3 bets] | [NRR > 100%, second ICP segment] |
| Q4 | Scale or pivot | [Decision gate] | [Rule of 40 > X] |

## Critical Rules

- NEVER present TAM/SAM/SOM without sourced numbers — every estimate needs a calculation or a citation
- NEVER skip the Anti-ICP — without it, any sales team will game the ICP criteria
- ALWAYS include Base, Bull, and Bear scenarios in financial modeling — no single "best case"
- ALWAYS define the Beachhead Segment before expanding ICP — win narrow first
- NEVER conflate GTM motion — PLG and SLG have fundamentally different economics, team structures, and success metrics
- ALWAYS validate the riskiest assumption first — don't polish secondary features while core hypothesis is unproven
- NEVER use generic market size numbers without applying SAM filters — TAM is not your market

## Example Usage

1. "Calculate TAM/SAM/SOM for an AI-powered recruiting platform targeting mid-market US companies"
2. "Define ICP for our SaaS expense management tool — we have 12 customers, what's the pattern?"
3. "Model unit economics: $299/mo ACV, $800 CAC, 85% gross margin, 24-month avg retention"
4. "Should we go PLG or SLG for a $5K ACV dev tools product?"
5. "Build a GTM Bet Board for our Q1 launch into the European market"
```

**Validação após criar:**
```bash
./scripts/validate-skill-yaml.sh skills/startup-growth-strategist
./scripts/validate-skill-content.sh skills/startup-growth-strategist
```
Esperado: sem erros, word count 1800-2500.

---

#### Task A1.2 — Atualizar `skills/startup-growth-strategist/README.md`

**Arquivo:** `skills/startup-growth-strategist/README.md`
**Ação:** Atualizar tabela Metadata para refletir versão 2.0.0 e data de hoje.

Localizar a linha `| Version | ...` e atualizar para `| Version | 2.0.0 |`
Localizar a linha `| Updated | ...` e atualizar para `| Updated | 2026-04-03 |`

---

### Batch 2: Enriquecer `product-strategy`

O skill é bom, mas faltam 7 adições específicas. Todas são cirúrgicas — não reescrever o skill inteiro.

#### Task A2.1 — Adicionar Beachhead Segment à seção "Where to Play"

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Após o bloco "**Where to Play:**" (linha ~155), antes de "**How to Win:**"
**Ação:** Inserir o bloco abaixo após a lista de Where to Play:

```markdown
**Beachhead Segment (obrigatório para 0→1):**
Before defining the full ICP, identify the single narrowest segment where you can win first:
- Where do you have 3+ reference customers or strong evidence?
- Where is pain highest and buyer access easiest?
- Where can you dominate the conversation before expanding?

> **Rule:** Name one company type so specific you could call 20 of them tomorrow. If you can't, your segment is still too broad. Win here before expanding.
```

---

#### Task A2.2 — Adicionar Buying Triggers e Intent Signals ao ICP

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Após o bloco "**Anti-ICP (equally important):**" (após a linha com "Why (wrong economics, wrong fit, wrong timing)")
**Ação:** Inserir após o bloco Anti-ICP:

```markdown
**Buying Triggers (events that create urgency NOW):**
Add 5+ triggers to your ICP definition — these are events that move a prospect from "someday" to "this quarter":
- Leadership change (new exec with a mandate)
- Funding event (budget to spend)
- Regulatory deadline (compliance forcing function)
- Competitive threat (pressure to catch up)
- Organizational event (merger, reorg, growth milestone)

**Intent Signals (observable indicators of active buying):**
- Hiring signals (job postings for roles related to your solution)
- Technographic signals (installing complementary or competing tools)
- Content signals (downloading whitepapers, attending competitor webinars)
- Financial signals (budget cycle timing, end-of-quarter pressure)
- Event signals (speaking at conferences about the problem you solve)

> **ICP without triggers and signals = a description. ICP with triggers and signals = a targeting system.**
```

---

#### Task A2.3 — Adicionar WTP Research Protocol à seção de Pricing

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Após o bloco "**0→1 Mode**: Run willingness-to-pay conversations early. Start simple: one price, test the market."
**Ação:** Inserir após essa linha:

```markdown
**WTP Research Protocol (Van Westendorp method — use before setting price):**

Run with 8-12 customer interviews. Ask four questions in sequence:
1. "At what price would this be so cheap you'd question the quality?"
2. "At what price would this be a bargain — a great buy for the value?"
3. "At what price would this start to feel expensive — you'd think twice?"
4. "At what price would this be too expensive to consider?"

Plot the four curves. The Acceptable Price Range sits between the intersection of "too cheap" and "too expensive" curves. The Optimal Price Point is where "bargain" and "expensive" curves intersect.

**Gabor-Granger complement:** After Van Westendorp, present 3-5 specific price points and ask "Would you buy at this price?" (Yes/No). Reveals purchase probability by price band — use to choose between tiers.

**Pricing anti-patterns to document and avoid:**
- Cost-plus pricing (cost + margin — ignores value)
- Competitor mirroring (matching price without value analysis)
- "What does the market charge?" without WTP validation
```

---

#### Task A2.4 — Adicionar estrutura de posicionamento 3 partes

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Após o bloco "**Positioning Statement Format:**" (o template "For [ICP] / Who are...")
**Ação:** Inserir após o template de Positioning Statement:

```markdown
**Alternative: GTM Triangle Positioning Structure (Market Change → Thesis → Proof):**
Use this when launching into an established market or competing against the status quo:

```
Market Change: [Observable trend making the old way obsolete — cite evidence]
Thesis:        [Your opinionated take on what this means for buyers]
Proof:         [2-3 data points or customer outcomes already validating your thesis]
```

Example:
- Market Change: "AI is writing 40% of enterprise code — but it ships with no security context"
- Thesis: "Security must be embedded at generation time, not audited after the fact"
- Proof: "3 of our customers eliminated their post-ship security backlog within 60 days"

> **Consistency check:** Run this test before finalizing. Your Market Change, ICP, and Positioning Statement must tell the same story without contradiction. If a reader can believe the Market Change but doubt your ICP fits it, your positioning has a gap.
```

---

#### Task A2.5 — Adicionar Channel Effort/Impact Matrix ao GTM Motion

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Após a tabela PLG vs SLG (a tabela com ACV, Complexity, Buyer, etc.)
**Ação:** Inserir após essa tabela:

```markdown
**Channel Effort/Impact Matrix:**
Score each channel 1-5 on Effort (resource cost, time to results) and Impact (expected pipeline contribution). Prioritize low-effort/high-impact first.

| Channel | Effort (1=easy, 5=hard) | Impact (1=low, 5=high) | Priority |
|---------|------------------------|----------------------|---------|
| Founder-led outbound | 2 | 5 | P1 — start here |
| Product virality / referral | 3 | 5 | P1 — design in from day 1 |
| Content / SEO | 3 | 4 | P2 — build in parallel |
| Events / community | 4 | 4 | P3 — after PMF |
| Paid acquisition | 4 | 3 | P3 — after unit economics proven |
| Partnerships | 4 | 4 | P3 — 6+ months out |

> **Rule:** Pick 2 channels maximum in 0→1 mode. Master them before adding more. Adding channels too early dilutes focus and makes learning impossible.
```

---

#### Task A2.6 — Adicionar "do nothing" como concorrente explícito

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Na seção "2. Market & Competitive Reality", após a tabela de análises (Porter's, 5C, etc.)
**Ação:** Após o bloco "Key questions:", inserir:

```markdown
**Competitive Landscape Table — always include "do nothing":**

| Alternative | Why buyers choose it | Why buyers leave it | Your wedge |
|-------------|---------------------|--------------------|-----------| 
| Competitor A | | | |
| Competitor B | | | |
| Internal build | | | |
| **Do nothing** | Status quo inertia, switching cost, budget freeze | [Trigger that forces action] | [Urgency play] |

> **Rule:** "Do nothing" is always your biggest competitor. If you can't articulate why buyers switch from inertia to action, you don't understand your market yet.
```

---

#### Task A2.7 — Adicionar Consistency Checklist

**Arquivo:** `skills/product-strategy/SKILL.md`
**Localização:** Na seção "Quick Reference: Strategy Quality Checklist", após os 9 itens existentes
**Ação:** Adicionar 3 itens ao checklist existente:

```markdown
- [ ] **Beachhead identified** — You can name 20 companies to call tomorrow in the narrowest segment
- [ ] **WTP validated** — You've run Van Westendorp with at least 8 customers before setting price
- [ ] **Positioning consistent** — Market Change, ICP, and Positioning Statement tell the same story without contradiction
```

---

### Batch 3: Enriquecer `abx-strategy`

O skill é forte — 3 adições cirúrgicas de alta precisão.

#### Task A3.1 — Adicionar Step 0: GTM Diagnostic como ponto de entrada

**Arquivo:** `skills/abx-strategy/SKILL.md`
**Localização:** Logo após o bloco de Progress Tracking (após o `[████...]` lines), antes de "## Core Philosophy"
**Ação:** Inserir novo bloco:

```markdown
## Step 0: GTM Diagnostic (Run Before Any ABX Work)

Before building strategy, assess current GTM maturity across 7 dimensions (score 1-5 each):

| Dimension | 1 — Not started | 3 — Partial | 5 — Systematic | Score |
|-----------|----------------|------------|----------------|-------|
| **ICP Definition** | "We sell to everyone" | ICP exists but unscored | Tiered ICP with scoring + Anti-ICP | |
| **Signal Coverage** | No signal monitoring | Manual alerts on some accounts | Automated signal feed across 4 categories | |
| **Buying Committee** | Single-threaded (IT only) | 2 contacts per account | 3+ personas mapped, coach identified | |
| **Message Resonance** | Generic pitch everywhere | Segment-level messaging | Persona-level + measured resonance rate | |
| **Pipeline Visibility** | CRM is a mess | Stages defined | Velocity tracked, marketing influence measured | |
| **Learning Loop** | No formal reviews | Monthly reviews | Weekly/bi-weekly cadence with bet updates | |
| **Sales-Marketing Alignment** | Siloed, low trust | Shared targets | Joint bet board, shared pipeline metrics | |

**Score interpretation:**
- 28-35: Scaling — focus on optimization and expansion
- 20-27: Building — execute the 90-day plan with all frameworks
- 12-19: Foundation — start with ICP + signal system before campaigns
- <12: Start here — fix ICP and buying committee mapping first

Document the 2 lowest-scoring dimensions. These become the first bets in the 90-day plan.
```

---

#### Task A3.2 — Adicionar Composite Lead Scoring Formula ao Framework 3

**Arquivo:** `skills/abx-strategy/SKILL.md`
**Localização:** Após a tabela de Tier Assignment no "Framework 3: PURE Problem Scoring"
**Ação:** Inserir após a tabela de tiers:

```markdown
### Live Lead Scoring Formula (for active pipeline management)

PURE scores accounts at entry. For active pipeline, use a dynamic composite formula that factors in engagement recency:

```
Lead Score = (Fit Score × 0.30) + (Intent Score × 0.45) + (Engagement Score × 0.25)
```

**Fit Score (0-100):** Firmographic + technographic match to ICP (from PURE + ICP tier)
**Intent Score (0-100):** Signal strength weighted by recency and directness:
- Recency multiplier: ×3 (signal <7 days), ×2 (7-30 days), ×1 (30-90 days), ×0 (>90 days — staleness rule)
- Directness multiplier: ×3 (direct buying signal), ×2 (related signal), ×1 (weak indicator)
- Frequency: ×2 if 3+ signals in 30 days
**Engagement Score (0-100):** Marketing/sales touchpoints in last 60 days (meetings, content engagement, email opens with click-through, event attendance)

**Score-to-Action SLAs:**

| Score Band | Priority | Sales Action | SLA |
|------------|----------|-------------|-----|
| 80-100 | P1 | Personal outreach from senior rep | Within 24 hours |
| 60-79 | P2 | Personalized outreach sequence | Within 3 days |
| 40-59 | P3 | Automated nurture + sales check | Within 1 week |
| 20-39 | P4 | Nurture only | No SLA |
| <20 | P5 | Deprioritize | Remove from active list |

**Staleness Rule:** Any signal older than 4 months loses all weight in the Intent Score. Re-qualify from scratch if re-engagement begins.

**Capacity allocation:** Dedicate 40% of outreach capacity to P1 accounts, 35% to P2, 25% to P3. Zero capacity to P4/P5.
```

---

#### Task A3.3 — Adicionar Signal Scoring Multipliers ao Signal Detection

**Arquivo:** `skills/abx-strategy/SKILL.md`
**Localização:** Após a tabela "Signal Detection Categories" (a tabela com Policy/Regulatory, Procurement, Leadership, Competitive, Strategic)
**Ação:** Inserir após essa tabela:

```markdown
### Signal Scoring Multipliers

When multiple signals arrive simultaneously, score their combined strength using multipliers:

| Signal Quality Dimension | Score Multiplier | Examples |
|--------------------------|-----------------|----------|
| **Recency** (×3 max) | ×3 if <7 days, ×2 if 7-30 days, ×1 if 30-90 days, ×0 if >90 days | Job posted today vs 3 months ago |
| **Directness** (×3 max) | ×3 if direct buying intent, ×2 if related, ×1 if weak | RFP published (×3) vs LinkedIn like on content (×1) |
| **Frequency** (×2 max) | ×2 if 3+ signals in 30 days | Cluster of signals = higher priority |
| **Specificity** (×1) | ×1 if explicitly names your solution category | Job posting mentions your exact tech |
| **Source reliability** (×1) | ×1 if from authoritative source | Procurement portal > anonymous tip |

**Signal + fit > fit alone > signal alone.**

An account with perfect PURE score and no signals is a cold target. An account with strong signals but poor PURE fit is a distraction. Prioritize the intersection.
```

---

## TRACK B — Novos Skills de GTM de Produto

### Batch 4: `gtm-positioning` (novo skill — Shared, crítico)

**Purpose:** O 4º P ausente em ambos os workflows. Posicionamento é o multiplicador — com ele certo, ICP converte mais rápido, canal funciona melhor, preço se justifica sozinho.

**Brief para `skill-creator`:**

```
Nome: gtm-positioning
Track: Shared (produto e consultoria)

Purpose: Definir posicionamento e mensagem de mercado usando April Dunford 6-step,
criar Messaging House com variações por persona, desenvolver battle cards por
concorrente, e medir Message-Market Fit.

Trigger phrases:
- "Preciso posicionar meu produto/oferta"
- "Como diferenciamos da concorrência?"
- "Me ajuda a criar o messaging para [produto/oferta]"
- "Nossa mensagem não está funcionando — o que mudar?"
- "Crie battle cards para [concorrente]"
- "Como sabemos se o posicionamento está certo?"

Should NOT trigger:
- "Preciso definir pricing" → gtm-product-pricing
- "Qual é o meu ICP?" → gtm-icp-designer
- "Valida se o mercado existe" → gtm-market-sizing

Frameworks obrigatórios:
1. April Dunford 6-step positioning:
   Competitive alternatives → Unique attributes → Value → Target customers
   → Market category → Relevant trends
2. GTM Triangle positioning (Market Change → Thesis → Proof)
3. Messaging House: Primary message (10 words) + 3 pillars + per-persona variations
4. Battle card por concorrente: why they choose them / why they leave / your wedge / objection handlers
5. Message-Market Fit measurement: % of calls where prospect uses your language back

Output format:
- Positioning Statement (April Dunford format)
- GTM Triangle (3 linhas)
- Messaging House (tabela)
- Battle Cards (uma por concorrente)
- Message-Market Fit KPI target

Evals (3 test cases):
1. "Posiciona nosso AI contract review tool — concorrentes são DocuSign, Ironclad, e fazer manual"
2. "Nossa mensagem é 'plataforma de analytics enterprise' e não está convertendo — onde está errado?"
3. "Cria battle card para nossa proposta de Outsourcing vs. time interno do cliente" (consulting context)
```

**Validação:**
```bash
./scripts/validate-skill-yaml.sh skills/gtm-positioning
./scripts/validate-skill-content.sh skills/gtm-positioning
```

---

### Batch 5: `gtm-product-launch` (novo skill)

**Purpose:** Skill mais importante do Track B. Guia o caminho completo do GTM canvas ao go-live, em 9 fases sequenciais. Baseado na metodologia Maja Voje (GTM-Strategist) + gtm-skills-os. Cada fase produz um output que alimenta a próxima.

#### Task B1.1 — Criar `skills/gtm-launch-strategy/SKILL.md`

**Arquivo:** `skills/gtm-launch-strategy/SKILL.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
---
name: gtm-launch-strategy
description: This skill should be used when the user needs to plan and execute a product or feature launch from GTM foundations to go-live. Use when building a complete launch strategy including market validation, pricing, positioning, launch assets, channel selection, and post-launch retrospective. Covers the full 9-phase journey from OPE Canvas to growth system.
license: MIT
---

# GTM Launch Strategy

A complete go-to-market launch system covering the full journey from strategic foundations to live market. Nine sequential phases, each producing an output that feeds the next. Based on Maja Voje's GTM Strategist methodology and the gtm-skills-os framework.

## When to Use

- Launching a new product or major feature
- Entering a new market segment or geography
- Repositioning an existing product with updated messaging
- Planning a product-led growth (PLG) launch
- Building GTM infrastructure from scratch for an early-stage product
- Doing a structured post-launch retrospective and planning next cycle

## Related Skills

- `startup-growth-strategist` — run before this skill to validate market and unit economics
- `product-strategy` — run in parallel for strategic context (ICP, positioning foundation)
- `pricing-strategy` — use in Phase 4 for deep WTP research
- `signal-based-outreach` — use in Phase 7 to build outbound engine
- `product-delivery` — use for technical staged rollout alongside GTM

## Progress Tracking

```
[██░░░░░░░░░░░░░░░░░░] 11% — Phase 1/9: GTM Foundations
[████░░░░░░░░░░░░░░░░] 22% — Phase 2/9: Market Intelligence
[██████░░░░░░░░░░░░░░] 33% — Phase 3/9: Customer Validation
[████████░░░░░░░░░░░░] 44% — Phase 4/9: Pricing & Packaging
[██████████░░░░░░░░░░] 55% — Phase 5/9: Positioning & Messaging
[████████████░░░░░░░░] 66% — Phase 6/9: Launch Assets
[██████████████░░░░░░] 77% — Phase 7/9: Channel Strategy
[████████████████░░░░] 88% — Phase 8/9: Launch Execution
[████████████████████] 100% — Phase 9/9: GTM System & Retrospective
```

## Workflow

### Phase 1: GTM Foundations

Establish the strategic context before any tactical work. Outputs save to `gtm-outputs/01-foundations.md`.

**OPE Canvas — define in one session:**
- **O (Opportunity):** What market window exists? Why now? What's changing?
- **P (Problem):** What specific pain does this solve? In buyer's language, not yours.
- **E (Expectations):** What does success look like in 90 days? In 12 months? What would kill this launch?

**SWOT for this launch specifically:**
- Strengths: What unfair advantages do you bring?
- Weaknesses: What's missing (team, product maturity, market presence)?
- Opportunities: What tailwinds exist (trend, regulation, competitor misstep)?
- Threats: What could kill this launch in 90 days?

**90-Day GTM Plan structure:**
- Month 1: Foundation and validation
- Month 2: First bets live, measuring
- Month 3: Double down on what works, kill what doesn't

**Analytics baseline:** Before launch, document the current state of every metric you'll track post-launch. You can't measure improvement without a baseline.

---

### Phase 2: Market Intelligence

Gather evidence before designing. Outputs to `gtm-outputs/02-intelligence.md`.

**Beachhead Segment identification:**
Map potential segments on two axes: Pain Intensity (1-5) × Buyer Accessibility (1-5). The top-right quadrant is your beachhead. Pick ONE segment to win first.

**Customer interview sprint (minimum 8 interviews):**
For each interview capture:
- Job title and context
- Current workflow (what do they do today?)
- Pain severity (1-10, ask "what's this costing you?")
- Attempted solutions (what else have they tried?)
- Decision process (who else is involved in buying?)
- "Last time you had this problem, what did you do?" (reveals real behavior, not hypothetical)

**Interview synthesis — look for patterns across 3+ interviews before drawing conclusions:**
- Top 3 pain points (in buyer's words, verbatim quotes)
- Common trigger events ("what made you start looking for solutions?")
- Vocabulary (what words do they use? Use their language in messaging)
- Decision criteria (what would make them choose or reject a solution?)

**Competitive intelligence:**
- 3-5 direct competitors: pricing, positioning, top features, known weaknesses
- Indirect competitors: spreadsheets, manual processes, internal builds
- "Do nothing": why is inertia the most common choice? What would break it?

---

### Phase 3: Customer Validation

Before building or launching, validate that you're solving the right problem for the right people. Outputs to `gtm-outputs/03-validation.md`.

**Assumption ranking — list every assumption the launch depends on:**

| Assumption | If wrong, consequence | Confidence (1-5) | Test method | Test cost |
|-----------|----------------------|-----------------|-------------|----------|
| [Most critical] | Launch fails | 2 | [Fastest test] | [Days] |
| [Second] | Miss ARR target | 3 | | |

**Validate top 3 assumptions before full build:**
- Fastest test for each (smoke test page, prototype demo, pre-sale conversation)
- Define pass/fail criterion before running the test
- Timeline: max 2 weeks per assumption

**Validated ICP check:**
After interviews and assumption tests, update your ICP from Phase 1. Does the evidence match your initial hypothesis? Adjust firmographics, triggers, and scoring before proceeding.

**PMF Signal (if product already exists):**
Run the Superhuman question: "How would you feel if you could no longer use this product?"
- Very disappointed / Somewhat disappointed / Not disappointed
- Target: >40% "very disappointed" before scaling

---

### Phase 4: Pricing & Packaging

Use the `pricing-strategy` skill for deep execution. This phase produces the pricing decision. Outputs to `gtm-outputs/04-pricing.md`.

**Value Metric selection (pick ONE primary):**
The value metric is what you charge for. It must:
- Scale naturally with customer value (you win when they win)
- Be simple enough to explain in 30 seconds
- Not penalize the behaviors you want to encourage

Common SaaS value metrics: per seat, per usage (API calls, events), per outcome (% of revenue), per asset (records, documents), flat fee.

**WTP Research (Van Westendorp — 8+ customers):**
Ask four questions in order:
1. "At what price would this seem too cheap to trust?"
2. "At what price would this be a great bargain?"
3. "At what price would this start to feel expensive?"
4. "At what price would this be too expensive to consider?"

Plot the curves. Acceptable Price Range = between "too cheap" and "too expensive" intersections.

**Pricing tiers (maximum 3 — no exceptions):**
| Tier | Name | Price | Target Segment | Upgrade Trigger |
|------|------|-------|---------------|----------------|
| Good | [Name] | $X/mo | [Segment] | [What makes them upgrade] |
| Better | [Name] | $X/mo | [Segment] | [What makes them upgrade] |
| Best | [Name] | $X/mo | [Segment] | — |

**Feature gate logic:** For each key feature, document which tier includes it and why. Avoid putting features that drive value demonstration behind a paywall (that kills activation).

---

### Phase 5: Positioning & Messaging

Where strategy meets language. Outputs to `gtm-outputs/05-positioning.md`.

**April Dunford 6-step positioning framework:**
1. **Competitive alternatives:** What do customers compare you to (including "do nothing")?
2. **Unique attributes:** What do you have that alternatives don't?
3. **Value:** What value does each unique attribute deliver?
4. **Target customers:** Who values those specific attributes most?
5. **Market category:** What frame of reference helps buyers understand you fastest?
6. **Relevant trends:** What market changes make your positioning urgent?

**GTM Triangle positioning structure:**
```
Market Change: [Observable trend — cite data, not opinion]
Thesis:        [Your opinionated take on what it means]
Proof:         [2-3 customer outcomes that validate the thesis]
```

**Messaging House:**
- Primary message (the one thing — 10 words max)
- 3 supporting pillars (why believe the primary message)
- Per-persona variations: same product, different value narrative per buyer role
- Top 5 objections + responses (sourced from interview data)

**Consistency check before proceeding:**
Does your Market Change explain why buyers MUST act? Does your ICP (Phase 2) match the customers who value your unique attributes? Does your pricing (Phase 4) match the value you're claiming? If any answer is no, fix it here before building assets.

---

### Phase 6: Launch Assets

Build the minimum set of assets needed to go live. Outputs to `gtm-outputs/06-assets.md`.

**Required assets for any launch:**
- [ ] Website landing page / product page (with GTM Triangle positioning)
- [ ] Demo script (15-minute structured narrative: problem → approach → proof → CTA)
- [ ] One-pager (sales leave-behind: problem, solution, proof, pricing, CTA)
- [ ] Email sequence (5-touch: awareness → interest → consideration → decision → post-signup)

**Optional assets by motion:**
- PLG: onboarding flow copy, in-app tooltips, empty state messaging
- SLG: pitch deck (CEO/economic buyer version), battle cards (top 3 competitors), ROI calculator
- Partner: partner brief, co-marketing one-pager, partner enablement guide

**Quality gate for each asset:**
- Does it use buyer's language (not internal jargon)?
- Does it address the #1 objection from interview data?
- Does it have a single clear CTA?
- Would the beachhead segment recognize themselves in it?

---

### Phase 7: Channel Strategy & Outbound Engine

**Channel selection (use Effort/Impact matrix from `product-strategy`):**
Pick 2 channels for launch. Add a third only after the first 2 are generating consistent pipeline.

**For inbound channels (content, SEO, paid):**
- Define the content calendar for Month 1-2
- Set up tracking before anything goes live
- Define "signal" that a visitor is sales-ready (page visited, time on page, form fill)

**For outbound (use `signal-based-outreach` skill for deep execution):**
- Build the ICP-scored target list (use Phase 2 Beachhead Segment)
- Set up signal monitoring (job postings, technographic changes, intent data)
- Design the 5-touch sequence (problem-aware → solution-aware → evaluation-ready)
- Define reply/meeting target rate (baseline: 5-15% reply rate for warm outbound)

**Partner channel (if applicable):**
- Identify 3-5 partners who already serve your ICP
- Design the referral or co-sell motion
- Create the partner brief (why this benefits their customers)

---

### Phase 8: Launch Execution

**Launch tier decision (determines resource allocation):**
| Tier | Scope | Activities |
|------|-------|-----------|
| Tier 1 (Major) | New product or major market entry | PR, event, full sequence activation, exec involvement |
| Tier 2 (Significant) | New segment or pricing change | Email blast, outbound sprint, content push |
| Tier 3 (Standard) | Feature or improvement | Product email, in-app notification, sales enablement |
| Tier 4 (Minor) | Bug fix or small enhancement | Release notes only |

**Launch day checklist:**
- [ ] All assets live and tracked
- [ ] Analytics confirmed firing
- [ ] Outbound sequence activated for beachhead list
- [ ] Sales team briefed (talk track, objection handlers, battle cards distributed)
- [ ] Support team briefed (FAQ, known issues, escalation path)
- [ ] Success metrics baseline documented (before state captured)

**First 30 days measurement cadence:**
- Daily: conversion rate, sign-ups or pipeline created
- Weekly: reply rates, demo schedule rate, message resonance check
- Weekly: "Kill or continue?" assessment on each active bet

---

### Phase 9: GTM System & Retrospective

**Post-launch retrospective (run at 30 and 90 days):**

| Question | Answer | Action |
|----------|--------|--------|
| Which bets worked? (metric exceeded target) | | Scale |
| Which bets failed? (metric missed kill condition) | | Kill |
| What was surprising? | | Investigate |
| What assumption was wrong? | | Update Phase 3 docs |
| What would you do differently? | | Update playbook |

**Growth loop documentation:**
Identify and document any growth loop that emerged:
- Viral loop: User action → new user acquisition
- Content loop: Usage → user-generated content → SEO → new users
- Ecosystem loop: Integration → partner visibility → referrals

**GTM System SOPs:**
Create or update:
- ICP re-qualification checklist (quarterly)
- Signal monitoring runbook
- Onboarding sequence + activation criteria
- Expansion trigger playbook (what signals upsell?)
- Win/loss interview template

## Critical Rules

- NEVER skip Phase 3 (customer validation) — launching without validated assumptions is a funded guess
- NEVER build more than 3 pricing tiers — complexity kills conversion
- ALWAYS run the consistency check at end of Phase 5 before building any assets
- ALWAYS define the Beachhead Segment before Phase 7 — multi-segment launch = no segment
- NEVER measure launch success without a pre-launch baseline
- ALWAYS do the 30-day retrospective — launches that aren't reviewed become templates for future mistakes

## Example Usage

1. "Plan a full GTM launch for our AI contract review tool targeting in-house legal teams at mid-market companies"
2. "We're entering the European market with our existing product — build a GTM launch plan"
3. "Reposition our analytics tool from 'data teams' to 'business users' — guide me through the GTM phases"
4. "Run a post-launch retrospective: we launched 6 weeks ago, here's what happened..."
5. "We're a PLG product adding an enterprise sales motion — build the GTM strategy"
```

---

#### Task B1.2 — Criar `skills/gtm-launch-strategy/README.md`

**Arquivo:** `skills/gtm-launch-strategy/README.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
# GTM Launch Strategy

Complete go-to-market launch system — from OPE Canvas to post-launch growth system. Nine sequential phases, each producing outputs that feed the next.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Product Launch |
| Tags | gtm, launch, positioning, pricing, channels, plg, slg, product-launch |
| Risk | Low |

## Overview

Built for product managers, founders, and GTM leaders planning a structured product launch. Integrates Maja Voje's GTM Strategist methodology (tested with 1000+ companies) with gtm-skills-os operational depth.

## Where This Fits in the Product GTM Workflow

```
startup-growth-strategist → product-strategy → [gtm-launch-strategy] → pricing-strategy
                                                         ↓
                                               signal-based-outreach
                                               gtm-demand-generation
                                                         ↓
                                               gtm-account-expansion
```

## Key Outputs

Each phase produces a file in `gtm-outputs/`:
- `01-foundations.md` — OPE Canvas, SWOT, 90-day plan
- `02-intelligence.md` — beachhead segment, interview synthesis, competitive map
- `03-validation.md` — validated assumptions, updated ICP, PMF signal
- `04-pricing.md` — WTP research results, pricing tiers, feature gate matrix
- `05-positioning.md` — positioning framework, messaging house, consistency check
- `06-assets.md` — asset inventory and status
- `07-channels.md` — channel strategy, outbound list, sequence design
- `08-launch.md` — launch checklist, day-1 metrics baseline
- `09-system.md` — retrospective, growth loops, SOPs
```

---

### Batch 5: `pricing-strategy` (novo skill)

**Purpose:** WTP research, value metric selection, tier design, competitive anchoring. Baseia-se no gtm-skills-os pricing-strategy — o mais rigoroso dos 3 repos neste tema.

#### Task B2.1 — Criar `skills/pricing-strategy/SKILL.md`

**Arquivo:** `skills/pricing-strategy/SKILL.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
---
name: pricing-strategy
description: This skill should be used when the user needs to design or improve a SaaS or product pricing strategy. Use when selecting a value metric, conducting willingness-to-pay research, structuring pricing tiers (Good/Better/Best), designing feature gates, benchmarking against competitors, or creating a migration plan for pricing changes.
license: MIT
---

# Pricing Strategy

Design pricing that captures value, drives expansion, and matches buyer psychology. A rigorous, step-by-step system from value metric selection through tier design, WTP research, competitive anchoring, and migration planning.

## When to Use

- Setting pricing for a new product or feature for the first time
- Revising pricing that isn't driving the right customer behavior
- Adding tiers or packaging options to an existing product
- Preparing for a pricing page redesign
- Validating whether current pricing is leaving money on the table
- Planning a price increase with a migration strategy
- Competing against a lower-cost alternative

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 20% — Phase 1/5: Value Metric Selection
[████████░░░░░░░░░░░░] 40% — Phase 2/5: Willingness-to-Pay Research
[████████████░░░░░░░░] 60% — Phase 3/5: Competitive Anchoring
[████████████████░░░░] 80% — Phase 4/5: Tier Design & Feature Gates
[████████████████████] 100% — Phase 5/5: Migration & Review Schedule
```

## Workflow

### Phase 1: Value Metric Selection

The value metric determines what you charge for. This is the single most important pricing decision.

**Four evaluation criteria — score each candidate metric 1-3:**

| Criterion | Question | Score 1-3 |
|-----------|----------|----------|
| **Value alignment** | Does this metric scale naturally with the value the customer gets? | 1=no, 3=yes |
| **Incentive alignment** | Do you win when the customer wins? | 1=misaligned, 3=perfectly aligned |
| **Simplicity** | Can you explain it in 30 seconds without confusion? | 1=confusing, 3=obvious |
| **Expansion potential** | Does natural customer growth increase their usage of this metric? | 1=no, 3=yes |

**Common value metrics — evaluate each against criteria above:**

| Metric | Best For | Watch Out For |
|--------|----------|--------------|
| Per seat | Collaboration, team tools | Penalizes adoption; customers reduce seats |
| Per usage (API calls, events) | Infrastructure, dev tools | Unpredictable bills → customer anxiety |
| Per outcome (% revenue, leads) | Performance marketing, success-based | Requires attribution — complex |
| Per asset (records, documents) | Data management, storage | May feel punitive as data grows |
| Flat fee | Simple products, low variance in value | Can't expand; leaves money on table |
| Hybrid (base + usage) | Products with floor + ceiling | More complex but often right answer |

**Rule:** Pick ONE primary value metric. Hybrid models can add a secondary metric for expansion, but never charge for two things that feel like the same thing to the buyer.

---

### Phase 2: Willingness-to-Pay Research

Never set price without WTP data. Run with 8-12 customers or prospects — mix of closed-won, closed-lost, and active evaluations.

**Van Westendorp Price Sensitivity Meter (4 questions, in order):**

Ask each participant:
1. "At what price would this product seem so cheap that you'd question its quality?"
2. "At what price would this product be a bargain — a great buy for the value?"
3. "At what price would this product start to feel expensive — you'd pay but think twice?"
4. "At what price would this product be too expensive to even consider?"

**Plot four curves and identify:**
- **Acceptable Price Range:** Between the "too cheap" and "too expensive" intersections
- **Optimal Price Point (OPP):** Where "bargain" and "expensive" curves cross
- **Indifference Price Point (IDP):** Where "bargain" and "cheap" curves cross (your floor)

**Gabor-Granger Purchase Probability (run after Van Westendorp):**
Present 3-5 specific price points (derived from Van Westendorp ranges). For each:
- "If this product cost $X/month, would you buy it?" (Yes / Maybe / No)
- Calculate purchase probability at each price point
- Use to select tier prices within the Acceptable Price Range

**WTP by segment:** Run the exercise separately for different ICP tiers if ACV differs significantly. Enterprise WTP ≠ SMB WTP.

**Red flags from WTP interviews:**
- "I'd need to get budget approval" → not current-budget (timing issue, not price issue)
- "It depends on what's included" → feature anxiety (packaging issue)
- "Our CEO needs to approve anything over $X" → economic buyer not in the room (access issue)

---

### Phase 3: Competitive Anchoring

Price exists in context. Buyers compare you — consciously or not.

**Competitive pricing matrix:**

| Competitor | Pricing Model | Entry Price | Mid-Tier | Top Tier | Key Feature Gates |
|-----------|---------------|------------|---------|---------|------------------|
| Competitor A | Per seat | $X/seat | $Y/seat | Custom | [What's locked] |
| Competitor B | Flat fee | $X/mo | $Y/mo | — | |
| You (current) | | | | | |
| You (proposed) | | | | | |

**Positioning relative to competitors:**
- Premium (price higher): requires clear, defensible differentiation
- Parity: neutral — price is not a differentiator
- Value (price lower): acceptable if you're building market share in early stage, dangerous long-term

**"Do nothing" anchor:** What does the buyer spend today on the problem you solve? Spreadsheets, manual labor, workarounds? That's your real competition for budget. If your product costs less than the manual workaround, say so explicitly.

**Anchoring strategy:**
- Start high in the presentation (show your highest tier first)
- The middle tier should feel like the "smart choice" (decoy effect)
- The entry tier should be obviously limited, not just cheap

---

### Phase 4: Tier Design & Feature Gates

**Maximum 3 tiers — always. More than 3 tiers kills conversion.**

| Element | Good (Entry) | Better (Growth) | Best (Enterprise) |
|---------|-------------|----------------|------------------|
| **Target segment** | | | |
| **Price/month** | | | |
| **Value metric limit** | [e.g. up to 5 seats] | [e.g. up to 25 seats] | [Unlimited] |
| **Upgrade trigger** | [What makes them want Better?] | [What makes them want Best?] | — |

**Feature gate design principles:**
- Gate features that are valuable but not critical for first activation
- Never gate the feature that demonstrates your core value proposition
- Gate collaboration features to drive expansion (teams pay more)
- Gate integrations that matter to power users

**Feature gate matrix:**

| Feature | Good | Better | Best | Gate Rationale |
|---------|------|--------|------|---------------|
| [Core feature] | ✅ | ✅ | ✅ | Must be in free/entry |
| [Collaboration] | ❌ | ✅ | ✅ | Drives team expansion |
| [Advanced analytics] | ❌ | ✅ | ✅ | Power user value |
| [SSO / Security] | ❌ | ❌ | ✅ | Enterprise requirement |
| [API access] | Limited | Full | Full | Dev expansion |

**Freemium / Free Trial decision:**
- Free forever tier: builds top of funnel, requires clear upgrade motivation. Use when time-to-value is very short and viral mechanics exist.
- Free trial (time-limited): creates urgency, works when product is complex enough to need onboarding. 14 days minimum to demonstrate value.
- No free option: valid for enterprise ACV >$10K where sales-led motion dominates.

---

### Phase 5: Migration Plan & Review Schedule

**Migration plan for existing customers (if changing pricing):**

| Customer Segment | Current Plan | New Plan | Migration Path | Communication |
|-----------------|-------------|---------|---------------|---------------|
| Existing on old tier | | | Grandfather for 12 months | Email 90 days before |
| Trial users | | | | |
| Annual contracts | | | Honor until renewal | |

**Grandfather rule:** Always honor existing annual contracts until renewal. Never force migration mid-contract. This is a trust issue, not a pricing issue.

**Communication sequence for price change:**
1. 90 days out: "We're updating our pricing" email — explain why (added value)
2. 60 days out: Reminder with specific impact per customer
3. 30 days out: Final reminder with link to FAQ
4. Day of change: Confirm their plan and new pricing
5. 30 days after: Check-in for questions

**Pricing review schedule:**
- Monthly: Monitor expansion MRR, churn by tier, upgrade/downgrade rates
- Quarterly: Check if conversion rates shifted (sign this pricing is working or not)
- Annually: Full WTP re-research, competitive benchmarking refresh

**Pricing health metrics:**

| Metric | Formula | Target |
|--------|---------|--------|
| Expansion MRR rate | Expansion MRR ÷ Starting MRR | >15%/month |
| Tier conversion rate | Users upgrading ÷ Users eligible | >5%/month |
| Price-to-value ratio | Customer NPS segmented by price paid | Higher price should not = lower NPS |
| Churn by tier | % churning per tier | Lower in higher tiers (validates value) |

## Critical Rules

- NEVER set more than 3 tiers — complexity kills conversion even when buyers claim they want options
- NEVER set pricing without running WTP research with at least 8 customers first
- ALWAYS include a migration plan before changing prices for existing customers
- NEVER gate the feature that demonstrates your core value proposition — that kills activation
- ALWAYS run Van Westendorp before Gabor-Granger — sequence matters
- NEVER price based on cost + margin — price based on value delivered
- ALWAYS anchor presentation with highest tier first

## Example Usage

1. "Design pricing for our B2B SaaS project management tool — current price is $15/seat/mo and we're seeing high churn in smaller companies"
2. "We're adding an enterprise tier — help me design the feature gates and migration plan for existing Pro customers"
3. "Run a WTP analysis framework for our AI writing assistant targeting marketing teams"
4. "Our competitor just cut their price by 30% — should we respond? Design the analysis"
5. "We want to move from per-seat to usage-based pricing — guide me through the transition"
```

---

#### Task B2.2 — Criar `skills/pricing-strategy/README.md`

**Arquivo:** `skills/pricing-strategy/README.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
# Pricing Strategy

Rigorous SaaS and product pricing system — from value metric selection through WTP research, tier design, competitive anchoring, and migration planning.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Pricing |
| Tags | pricing, saas, wtp, willingness-to-pay, packaging, tiers, van-westendorp |
| Risk | Low |

## Overview

Built for PMs, founders, and GTM leaders designing or improving product pricing. Integrates Van Westendorp price sensitivity methodology, Gabor-Granger purchase probability, and the gtm-skills-os 3-tier packaging framework.

## Key Methods

- **Van Westendorp Price Sensitivity Meter** — 4 questions, plots 4 curves, identifies Acceptable Price Range
- **Gabor-Granger Purchase Probability** — converts WTP ranges into specific price point recommendations
- **3-tier maximum rule** — Good/Better/Best with hard cap (more tiers = lower conversion)
- **Feature gate matrix** — which features belong in which tier and why
```

---

### Batch 6: `signal-based-outreach` (novo skill)

**Purpose:** Outbound flywheel completo — ICP matrix, intent signals com scoring formula, cold email SPARK, analytics, loop de aprendizado. Baseado em gtm-flywheel (ColdIQ) + sachacoldiq.

#### Task B3.1 — Criar `skills/signal-based-outreach/SKILL.md`

**Arquivo:** `skills/signal-based-outreach/SKILL.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
---
name: signal-based-outreach
description: This skill should be used when the user needs to build or optimize an outbound sales system driven by buyer intent signals. Use when designing ICP scoring matrices, building signal detection infrastructure, writing cold outreach sequences, analyzing campaign performance, or creating a self-reinforcing outbound flywheel.
license: MIT
---

# Signal-Based Outreach

A complete outbound flywheel — from ICP matrix to signal scoring, sequence design, analytics, and learning loops. Outreach that triggers on buyer intent signals achieves 3-5× higher reply rates than cold outbound. Built on the ColdIQ gtm-flywheel methodology.

## When to Use

- Building an outbound sales motion from scratch
- Low reply rates on current outbound sequences
- Scaling from founder-led to team outbound
- Adding intent signal monitoring to an ABM program
- Optimizing cold email performance with data
- Designing LinkedIn outreach for enterprise prospects

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 20% — Phase 1/5: ICP Matrix & Scoring
[████████░░░░░░░░░░░░] 40% — Phase 2/5: Signal Infrastructure
[████████████░░░░░░░░] 60% — Phase 3/5: Sequence Architecture
[████████████████░░░░] 80% — Phase 4/5: Campaign Analytics
[████████████████████] 100% — Phase 5/5: Learning Loop
```

## Workflow

### Phase 1: ICP Matrix & Lead Scoring

**Step 1 — Analyze best customers (minimum 10):** List current customers with highest LTV, best retention, fastest time-to-value, and most referrals. What patterns emerge?

**Step 2 — Analyze worst customers:** List churned customers, support-heavy accounts, and low-NPS. What patterns distinguish them from best customers?

**Step 3 — Identify scoring signals:**
- Patterns present in 70%+ of best customers → positive scoring indicators
- Patterns present in <30% of worst customers → negative indicators (disqualifiers if extreme)

**Step 4 — Build the ICP Scoring Matrix (5 dimensions, 0-20 each, max 100):**

| Dimension | Signals | Score 0-20 |
|-----------|---------|-----------|
| **Firmographic Fit** | Company size, industry, revenue range, geography | |
| **Technographic Fit** | Tech stack, tools used, integrations, cloud provider | |
| **Buying Intent** | Signal recency + directness + frequency (see Phase 2) | |
| **Persona Access** | Title of reachable contact, seniority, decision authority | |
| **Engagement Readiness** | Prior interaction, mutual connections, content consumption | |

**Tier thresholds:**
- 85-100: Bullseye — personalized 1:1, immediate outreach
- 70-84: Strong Fit — personalized sequence, priority
- 55-69: Good Fit — standard sequence
- 40-54: Stretch — only if inbound signal present
- <40: Disqualified — do not contact

**Step 5 — Validate quarterly:** Compare scoring results against closed-won/closed-lost. If Bullseye accounts aren't converting, adjust weights.

---

### Phase 2: Signal Infrastructure

**Four signal categories — monitor all four:**

| Category | Signal Examples | Sources |
|----------|----------------|---------|
| **First-Party** | Website visits, content downloads, email opens, demo requests | CRM, website analytics, email platform |
| **Third-Party** | Intent data spikes on relevant topics | Bombora, G2, TechTarget |
| **Public** | Job postings, press releases, funding announcements, leadership changes | LinkedIn, Crunchbase, news alerts |
| **Social** | LinkedIn post engagement, conference speaking, community mentions | LinkedIn, Slack communities |

**Signal scoring formula:**
```
Signal Score = Base × Recency Multiplier × Directness Multiplier × Frequency Multiplier
```

| Dimension | Multiplier | Examples |
|-----------|-----------|---------|
| **Recency** | ×3 (<7 days), ×2 (7-30 days), ×1 (30-90 days), ×0 (>90 days) | Fresh signal >> stale signal |
| **Directness** | ×3 (direct buying intent), ×2 (adjacent), ×1 (weak) | "Pricing page visit" ×3, "blog read" ×1 |
| **Frequency** | ×2 (3+ signals in 30 days) | Cluster = urgency |

**Staleness rule:** Any signal older than 90 days is archived. Signals older than 4 months are discarded and require re-qualification.

**Signal + fit rule:** Signal + fit > fit alone > signal alone. A Bullseye account with a strong signal is the highest priority. A strong signal with a poor-fit account is a distraction.

**Score-to-action SLAs:**

| Score | Priority | Action | SLA |
|-------|----------|--------|-----|
| 80-100 | P1 | Personal outreach, senior rep | Within 24 hours |
| 60-79 | P2 | Personalized sequence | Within 3 days |
| 40-59 | P3 | Automated nurture | Within 1 week |
| <40 | P4 | Monitor only | No SLA |

**Capacity allocation:** 40% to P1, 35% to P2, 25% to P3. Zero capacity to P4.

---

### Phase 3: Sequence Architecture

**SPARK cold email framework (ColdIQ):**
- **S**ituation: Acknowledge their specific context (signal you detected)
- **P**ain: Name the pain in their language (from ICP research)
- **A**nswer: Your specific solution to that specific pain
- **R**eference: Proof point (customer outcome or stat)
- **K**ick: Low-friction CTA (question or soft ask)

**Sequence structure (5-touch, max 14 days):**

| Touch | Channel | Timing | Approach |
|-------|---------|--------|---------|
| 1 | Email | Day 1 | SPARK framework — signal reference in opening |
| 2 | LinkedIn | Day 3 | Connection request + 1-line message referencing touch 1 |
| 3 | Email | Day 6 | Different angle — lead with proof point |
| 4 | Phone | Day 9 | Leave 30-second VM if no answer — reference signal |
| 5 | Email | Day 14 | Breakup message — ask permission to follow up in 90 days |

**ATL vs BTL messaging split:**
- ATL (Above the Line — VP/C-Level): Business impact, strategic framing, P&L language, brief and direct
- BTL (Below the Line — Manager/IC): Operational pain, workflow specifics, technical proof, more detail

**Email line limits:**
- Subject: 6 words maximum
- Opening: name their signal or context in line 1
- Body: 3-4 lines maximum
- CTA: One question. Never two asks.

**Personalization rule:** The first line must be unique per account (references their specific signal or context). Lines 2-5 can be templatized. A fully templated email gets 60-70% lower reply rate.

---

### Phase 4: Campaign Analytics

**Six-layer diagnostic — check all six before changing anything:**

| Layer | Metric | Benchmark | If Below Benchmark |
|-------|--------|-----------|-------------------|
| **Deliverability** | Inbox placement rate | >85% | Fix domain health, warm up |
| **Open Rate** | Opens ÷ Delivered | >40% for cold | Fix subject line |
| **Reply Rate** | Replies ÷ Delivered | >5% cold, >15% warm | Fix first line or offer |
| **Positive Reply Rate** | Positive ÷ Total replies | >60% | Fix targeting or pitch |
| **Meeting Rate** | Meetings ÷ Positive replies | >70% | Fix conversion sequence |
| **Pipeline Rate** | Qualified opps ÷ Meetings | >30% | Fix ICP scoring |

**Benchmarks by outreach type:**
- Cold email (zero relationship): 3-7% reply rate
- Signal-triggered (detected intent): 10-20% reply rate
- Warm intro: 20-40% reply rate
- Event follow-up: 15-25% reply rate

**Winning copy extraction:** After 50+ sends, identify top 20% performing emails by reply rate. What do they have in common? Subject structure? Opening line type? Offer framing? Extract the pattern and test it as a template.

---

### Phase 5: Learning Loop

**Monthly review cadence:**

| Week | Activity |
|------|----------|
| Week 1 | Pull campaign analytics, identify top/bottom performers |
| Week 2 | Run objection mining (what reasons do people give for not replying?) |
| Week 3 | Update ICP scoring based on closed-won/lost data |
| Week 4 | Redesign one underperforming sequence element, launch A/B test |

**Objection mining (AER framework):**
For each common objection, develop:
- **A**cknowledge: Validate the concern without dismissing it
- **E**xplain: Reframe the context or add relevant evidence
- **R**edirect: Move toward a next step or question

Document the top 5 objections and their best responses. Update these monthly from call recordings or reply analysis.

**Deal patterns analysis:**
After 20+ closed deals, identify:
- What signals predicted a closed deal?
- What sequence touch had the highest conversion?
- What persona had the highest meeting-to-close rate?
- What company attributes appeared in 80%+ of closed deals?

Update the ICP Matrix and signal weights based on these patterns.

## Critical Rules

- NEVER send a fully templated email — the first line must reference a specific signal or context
- NEVER use more than 5 touches before marking as inactive — more touches past 5 decreases reply probability
- ALWAYS apply the staleness rule — signals older than 90 days lose scoring weight; >4 months = discard
- NEVER allocate outreach capacity to P4/P5 accounts when P1/P2 accounts exist
- ALWAYS diagnose deliverability before changing copy — bad inbox placement will tank any sequence
- NEVER ask two things in one email CTA — one ask only
- ALWAYS run the 6-layer diagnostic before redesigning a campaign

## Example Usage

1. "Build a signal-based outreach system for our fintech SaaS targeting CFOs at mid-market companies ($50M-$500M revenue)"
2. "Our cold email reply rate is 2% — run the 6-layer diagnostic and tell me what to fix"
3. "Design a 5-touch sequence targeting CTOs who just posted a job for a DevOps engineer (buying signal for our tool)"
4. "Score this list of 50 accounts against our ICP matrix and prioritize for outreach this week"
5. "We're getting a lot of 'not the right time' responses — help me build the objection handler using AER"
```

---

#### Task B3.2 — Criar `skills/signal-based-outreach/README.md`

**Arquivo:** `skills/signal-based-outreach/README.md`
**Ação:** Criar com o conteúdo abaixo.

```markdown
# Signal-Based Outreach

Complete outbound flywheel — ICP scoring matrix, signal infrastructure with scoring formula, SPARK cold email framework, six-layer campaign diagnostic, and learning loop.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Sales |
| Tags | outbound, signals, cold-email, icp, sequence, abm, sales-development |
| Risk | Low |

## Overview

Built for sales leaders, SDRs, founders, and GTM engineers designing or optimizing outbound motions. Based on ColdIQ's gtm-flywheel methodology — tested across 70+ client campaigns at a $7M ARR agency.

## Key Methods

- **ICP Scoring Matrix (5 dimensions):** Firmographic, Technographic, Buying Intent, Persona Access, Engagement Readiness
- **Signal scoring formula:** Base × Recency × Directness × Frequency multipliers
- **SPARK framework:** Situation, Pain, Answer, Reference, Kick
- **Six-layer campaign diagnostic:** Deliverability → Open → Reply → Positive → Meeting → Pipeline
- **AER objection framework:** Acknowledge, Explain, Redirect
```

---

### Batch 7: `gtm-demand-generation` e `gtm-account-expansion`

**Nota:** Estes dois skills são o Batch 7 e requerem aprovação do user após Batch 6.

#### Task B4.1 — Criar `skills/gtm-demand-generation/SKILL.md`

**Purpose:** Channel architecture, growth loops, trial-to-paid conversion, funnel optimization. Cobre o gap mais crítico identificado — nenhum skill atual toca demand gen.

**Arquivo:** `skills/gtm-demand-generation/SKILL.md`
**Ação:** Criar com frontmatter e body completo cobrindo:

**Frontmatter:**
```yaml
---
name: gtm-demand-generation
description: This skill should be used when the user needs to design or optimize a demand generation system for a SaaS or product company. Use when building paid, organic, or viral acquisition channels, designing growth loops, optimizing trial-to-paid conversion, building the marketing funnel, or diagnosing why top-of-funnel is not converting.
license: MIT
---
```

**Seções obrigatórias do body:**
1. **Phase 1: Channel Architecture** — Channel selection matrix (effort/impact), paid (SEM, social, display) setup foundations, organic (SEO, content, community) foundations, viral/referral loop design. Regra: master 2 channels before adding a 3rd.
2. **Phase 2: Growth Loop Design** — Identificar loops existentes (viral, content, ecosystem, sales). Documentar cada loop como: Trigger → Action → Reward → Invitation. Medir loop coefficient (K-factor para viral).
3. **Phase 3: Trial-to-Paid Conversion** — Definir "Activation Moment" (the specific action that predicts conversion). Activation funnel (signed-up → activated → retained → paid). Identify friction points. Design onboarding that leads to Activation Moment. Target: >20% trial-to-paid in 30 days.
4. **Phase 4: Funnel Optimization** — TOFU/MOFU/BOFU metrics. Identify the leakiest part of the funnel first. Conversion benchmarks por canal. A/B test cadence (one variable at a time).
5. **Phase 5: Measurement & Attribution** — UTM structure, attribution model (first-touch vs last-touch vs multi-touch). CAC por canal. ROAS por canal. North Star Metric definition.
6. **Critical Rules + Example Usage** (5 examples)

---

#### Task B4.2 — Criar `skills/gtm-demand-generation/README.md`

Criar README com tabela Metadata padrão (Version 1.0.0, Author Eric Andrade, Created 2026-04-03).

---

#### Task B4.3 — Criar `skills/gtm-account-expansion/SKILL.md`

**Purpose:** NRR strategy, upsell triggers, churn prevention, customer health scoring. Cobre o gap de expansão/retention.

**Arquivo:** `skills/gtm-account-expansion/SKILL.md`
**Ação:** Criar com frontmatter e body completo cobrindo:

**Frontmatter:**
```yaml
---
name: gtm-account-expansion
description: This skill should be used when the user needs to design or improve a customer expansion strategy. Use when building upsell and cross-sell playbooks, designing customer health scoring, creating churn prevention programs, improving Net Revenue Retention, or planning customer success motions that drive expansion revenue.
license: MIT
---
```

**Seções obrigatórias do body:**
1. **Phase 1: Customer Health Scoring** — Definir health score com 4-6 dimensions (product usage, support tickets, NPS/CSAT, payment health, stakeholder engagement, expansion indicators). Score 0-100. Traffic light (Green/Yellow/Red). Automatizar monitoramento semanal.
2. **Phase 2: Expansion Triggers & Upsell Playbook** — Identificar 5+ triggers para upsell (usage hitting tier limit, new use case adoption, team growth, funding event, success milestone). Por trigger: quem contata, mensagem, oferta, timing. Regra: upsell after value demonstrated, never before.
3. **Phase 3: Churn Prevention** — Early warning signals (login frequency drop, feature abandonment, support spike, stakeholder change). Churn risk scoring. Intervention playbook by risk level. Win-back sequence for churned accounts.
4. **Phase 4: NRR Strategy** — NRR formula (Expansion MRR + Contraction MRR + Churn MRR) ÷ Starting MRR. Target: >110% for strong SaaS. Diagnose the gap between current and target NRR. Identify which customer segment drives most expansion.
5. **Phase 5: Customer Success Cadence** — Check-in frequency by tier (High-touch: monthly, Mid-touch: quarterly, Low-touch: automated). QBR structure. Success plan template. Executive sponsor engagement.
6. **Critical Rules + Example Usage**

---

#### Task B4.4 — Criar `skills/gtm-account-expansion/README.md`

Criar README com tabela Metadata padrão.

---

## TRACK C — Skills de GTM de Consultoria Microsoft

> **Referência:** O conteúdo completo de cada skill está definido em `docs/plan/2026-04-03-gtm-consulting-skills-plan-v2.md`. Este plano substitui o v2 — execute pelo conteúdo aqui e ignore o v2 para a sequência de batches. O conteúdo dos skills (SKILL.md body) está integral no v2.

### Batch 8: Consulting GTM — Oferta e Preço (novos)

**Skills:** `gtm-consulting-offer-design` + `gtm-consulting-pricing`

**Usar `skill-creator` para ambos.**

#### Brief: `gtm-consulting-offer-design`

```
Nome: gtm-consulting-offer-design
Track: Consulting

Purpose: Projetar a oferta de consulting — definir escopo, entregáveis,
metodologia, team structure, Hero Offers como entry point, e proposta
comercial estruturada. O "Produto" do GTM de consultoria.

Trigger phrases:
- "Preciso estruturar minha oferta de [Assessment/SI/Outsourcing]"
- "Como defino o escopo de uma Hero Offer?"
- "Me ajuda a desenhar o deliverable de [oferta]"
- "Qual a diferença entre nossa oferta e do concorrente?"
- "Preciso criar uma proposta para [cliente]"

Should NOT trigger:
- "Quanto cobrar?" → gtm-consulting-pricing
- "Quem perseguir?" → gtm-consulting-icp
- "Como fazer o GTM desta oferta?" → gtm-consulting-designer

Frameworks obrigatórios:
1. Offer Design Canvas: Problem → Approach → Deliverables → Team → Timeline → Success metrics
2. Hero Offer structure: Entry point (fixed-fee $30-75K, 2-6 weeks) → Natural upsell path
3. Diferenciação metodológica: o que Avanade entrega diferente dos concorrentes (IP, accelerators, Microsoft depth)
4. Oferta vs. concorrente: feature/benefit/value table por oferta tipo
5. Proposta comercial: Executive Summary → Problem → Approach → Deliverables → Investment → Why Avanade

Offer types in scope:
- Assessment ($15K-$80K): discovery, current state, roadmap
- System Integration ($150K-$2M+): design, build, test, deploy
- Outsourcing (multi-year): operations, SLA, governance
- Shared Services: process standardization, scale

Output format:
- Offer Design Canvas preenchido
- Hero Offer brief (1 page)
- Differentiation table vs. 2-3 concorrentes
- Proposta outline (seções + key messages)

Evals:
1. "Estrutura uma Hero Offer de AI Assessment para uma conta Whitespace no setor de Manufatura"
2. "Desenha os entregáveis de um engagement de SI de $500K em Azure para o setor financeiro"
3. "Como diferenciamos nosso Outsourcing de um centro de serviços offshore genérico?"
```

#### Brief: `gtm-consulting-pricing`

```
Nome: gtm-consulting-pricing
Track: Consulting

Purpose: Definir modelo comercial e preço para engagements de consulting —
escolher entre T&M, fixed-fee, outcome-based ou retainer, justificar
rate premium, estruturar pricing por tipo de oferta e tier de conta.

Trigger phrases:
- "Quanto cobrar por este engagement?"
- "Fixed-fee ou T&M para este projeto?"
- "Como justificamos nosso rate versus o concorrente mais barato?"
- "Preciso precificar uma proposta de Outsourcing"
- "O cliente está pedindo outcome-based — vale a pena?"

Should NOT trigger:
- "Preciso precificar meu SaaS" → gtm-product-pricing
- "Estrutura a oferta de consulting" → gtm-consulting-offer-design
- "Como qualificar este deal?" → gtm-consulting-qualifier

Frameworks obrigatórios:
1. Modelo comercial por tipo de oferta:
   - Assessment: fixed-fee (previsibilidade para cliente, low risk para Avanade)
   - SI: T&M com teto (control + flexibility) ou fixed-fee se escopo maduro
   - Outsourcing: SLA-based com baseline + gain-share opcional
   - Retainer: recurring com volume commitment
2. Rate justification framework: value delivered vs. cost of alternatives vs. risk of doing nothing
3. Pricing psychology: anchoring alto, decoy tier, framing como investimento não custo
4. Discount governance: quando dar, quanto, quem aprova, contrapartidas esperadas
5. Commercial model tradeoffs matrix: T&M vs Fixed vs Outcome vs Retainer por contexto

Output format:
- Commercial model recommendation com rationale
- Rate range por role/seniority
- Pricing narrative (como apresentar ao CFO)
- Discount governance table

Evals:
1. "É melhor T&M ou fixed-fee para um SI de $800K no setor de saúde — cliente é conservador em risco"
2. "Justifica nosso rate de $220/hora para um Arquiteto Azure quando o offshore custa $80"
3. "O cliente quer outcome-based para o Outsourcing — como estruturamos sem assumir risco excessivo?"
```

**Gate:** User revisa os 2 skills antes de prosseguir para Batch 9.

---

### Batch 9: Consulting GTM — ICP e Designer

**Skills:** `gtm-consulting-icp` + `gtm-consulting-designer`

**Usar `skill-creator` para ambos. Brief base disponível em `docs/plan/2026-04-03-gtm-consulting-skills-plan-v2.md`** — usar o conteúdo do v2 como input para o skill-creator (não copiar diretamente, deixar o skill-creator estruturar com triggers e evals).

**Gate:** User revisa os 2 skills antes de prosseguir para Batch 10.

---

### Batch 9: Consulting GTM Batch 2

**Skills:** `presales-qualifier`, `executive-account-briefing`, `consulting-pursuit-governance`, `microsoft-cosell-strategy`, `consulting-account-expansion`

**Instrução de execução:** Continuar lendo o v2 para o conteúdo de cada skill. Se algum skill não tiver conteúdo completo no v2, criar baseado nos padrões estabelecidos neste plano.

---

## TRACK D — Release

### Batch 10: Version Bump e Documentation Sync

**Pré-requisito:** Todos os Tracks A, B, C completos e aprovados pelo user.

#### Task D1 — Contar skills e definir nova versão

Atual: 55 skills, v1.22.0

Adicionados:
- Shared: `gtm-market-sizing` + `gtm-icp-designer` = 2
- Product: `gtm-product-launch` + `gtm-product-pricing` + `gtm-product-outreach` + `gtm-product-demand-gen` + `gtm-product-expansion` + `gtm-product-workflow` = 6
- Consulting: `gtm-consulting-icp` + `gtm-consulting-designer` + `gtm-consulting-cosell` + `gtm-consulting-qualifier` + `gtm-consulting-briefing` + `gtm-consulting-pursuit` + `gtm-consulting-expansion` + `gtm-consulting-workflow` = 8
- **Total novos: 19 skills**

Renomeados (contam como novos, dir. antigo removido): `startup-growth-strategist` → `gtm-product-growth`
Enriquecidos (não contam): `product-strategy`, `abx-strategy`

Total: **74 skills**, versão alvo: **v1.27.0**

#### Task D2 — Executar release script

```bash
node scripts/release.js minor
# Será executado 2 vezes (minor do 1.22 → 1.23 → 1.24)
# Ou: fazer um único bump para 1.24.0 atualizando package.json manualmente
```

Verificar após:
```bash
./scripts/verify-version-sync.sh
```

#### Task D3 — Atualizar CLAUDE.md

Na seção `## Project Overview`: atualizar `v1.22.0` → `v1.24.0` e `55 skills` → `67 skills`.

Na seção `## Repository Architecture` (skills tree): adicionar os 12 novos skills em ordem alfabética.

Na seção `## Skill Types`: adicionar os novos skills nas categorias corretas:
- **GTM & Product**: `gtm-launch-strategy`, `pricing-strategy`, `signal-based-outreach`, `gtm-demand-generation`, `gtm-account-expansion`
- **GTM & Consulting Microsoft**: `microsoft-consulting-icp`, `consulting-gtm-designer`, `microsoft-cosell-strategy`, `presales-qualifier`, `executive-account-briefing`, `consulting-pursuit-governance`, `consulting-account-expansion`

#### Task D4 — Atualizar README.md

- H1 title: `# 🤖 Claude Superskills v1.24.0`
- Badge: `![Version](https://img.shields.io/badge/version-1.24.0-blue.svg)`
- Badge: `![Skills](https://img.shields.io/badge/skills-67-green.svg)`
- Footer: `*Version 1.24.0 | April 2026*`
- Adicionar novos skills nas tabelas de categoria relevantes

#### Task D5 — CHANGELOG.md

Adicionar entrada:
```markdown
## [1.26.0] - 2026-04-03

### Added (16 novos skills com convenção gtm- / gtm-product- / gtm-consulting-)

**Shared (ambos os tracks):**
- `gtm-market-sizing` — TAM/SAM/SOM 8-step workflow com fontes, filtros SAM, sensibilidade
- `gtm-icp-designer` — ICP scoring matrix 5 dimensões, buying triggers, Anti-ICP binários, buying committee

**Product GTM (gtm-product-):**
- `gtm-product-growth` — Unit economics + Bet Board + Assumption Ranking + Growth Roadmap *(rebuild de startup-growth-strategist)*
- `gtm-product-launch` — 9 fases OPE Canvas → validação → pricing → positioning → assets → go-live (Maja Voje)
- `gtm-product-pricing` — Value metric, WTP Van Westendorp + Gabor-Granger, 3 tiers (hard cap), feature gates
- `gtm-product-outreach` — ICP matrix 5D, signal scoring formula, SPARK cold email, 6-layer diagnostic
- `gtm-product-demand-gen` — Channel architecture, growth loops, trial-to-paid conversion, funnel optimization
- `gtm-product-expansion` — NRR strategy, customer health scoring, upsell triggers, churn prevention
- `gtm-product-workflow` — Orquestrador do workflow completo de GTM de Produto

**Consulting GTM (gtm-consulting-):**
- `gtm-consulting-icp` — ICP por tier × oferta (MVC/HG/Emerging/Whitespace), PURE scoring, Anti-ICP
- `gtm-consulting-designer` — GTM Canvas completo para offerings de consulting Microsoft
- `gtm-consulting-cosell` — Co-sell + funding Microsoft, DPOR, co-sell status, MCI/AMPP
- `gtm-consulting-qualifier` — Qualificação de deal antes de investir pre-sales
- `gtm-consulting-briefing` — Preparação de reunião C-level com contexto de conta
- `gtm-consulting-pursuit` — Gestão de pursuit ativo com gates e risk scoring
- `gtm-consulting-expansion` — Expansão de revenue em contas existentes
- `gtm-consulting-workflow` — Orquestrador do workflow completo de GTM de Consultoria

### Improved
- `product-strategy` — Beachhead Segment, WTP Van Westendorp protocol, 3-part positioning (Market Change→Thesis→Proof), Buying Triggers + Intent Signals no ICP, channel effort/impact matrix, "do nothing" competitor, consistency checklist
- `abx-strategy` — Step 0 GTM Diagnostic (7 dimensões), composite lead scoring `(Fit×0.30)+(Intent×0.45)+(Engagement×0.25)`, signal decay multipliers com SLAs, staleness rule 4 meses
```

#### Task D6 — Atualizar GitHub About

```bash
gh repo edit --description "67 Universal AI Skills for Claude Code, GitHub Copilot & 6 more platforms. GTM launch strategy, pricing, outbound, product strategy, consulting GTM, Obsidian, document conversion — no API keys required."
```

#### Task D7 — Regenerar indexes

```bash
npm run generate-all --prefix cli-installer
```

#### Task D8 — Criar package do plugin

```bash
./scripts/package-plugin.sh
```

#### Task D9 — Commit, tag, push

```bash
git add skills/ README.md CLAUDE.md CHANGELOG.md \
        cli-installer/package.json cli-installer/package-lock.json \
        .claude-plugin/plugin.json CATALOG.md skills_index.json

git commit -m "feat: add 19 GTM skills (gtm-product-* + gtm-consulting-* + gtm-*) and enrich 2 existing — bump to v1.27.0"

git tag v1.27.0
git push origin main && git push origin v1.27.0
```

---

## Validation Commands (rodar após cada batch)

```bash
# Validar um skill específico
./scripts/validate-skill-yaml.sh skills/<skill-name>
./scripts/validate-skill-content.sh skills/<skill-name>

# Validar todos os skills de uma vez
for skill in skills/*/; do
  ./scripts/validate-skill-yaml.sh "$skill"
  ./scripts/validate-skill-content.sh "$skill"
done

# Verificar sincronização de versões
./scripts/verify-version-sync.sh

# Contar skills
ls -d skills/*/ | wc -l
```

---

## Status Summary

| Batch | Track | Skills (nomes finais) | Criação | Novos | Status |
|-------|-------|----------------------|---------|-------|--------|
| **1** | Shared | `gtm-market-sizing`, `gtm-icp-designer` | skill-creator | +2 | ⏳ |
| **1** | Product | `gtm-product-growth` *(rebuild de startup-growth-strategist)* | skill-creator | rename | ⏳ |
| **2** | Enrich | `product-strategy` (7 adições cirúrgicas) | manual edit | 0 | ⏳ |
| **3** | Enrich | `abx-strategy` (Step 0 + scoring formula + decay) | manual edit | 0 | ⏳ |
| **4** | Shared | `gtm-positioning` ← **4º P ausente** | skill-creator | +1 | ⏳ |
| **5** | Product | `gtm-product-launch`, `gtm-product-pricing` | skill-creator | +2 | ⏳ |
| **6** | Product | `gtm-product-outreach` | skill-creator | +1 | ⏳ |
| **7** | Product | `gtm-product-demand-gen`, `gtm-product-expansion`, `gtm-product-workflow` | skill-creator | +3 | ⏳ |
| **8** | Consulting | `gtm-consulting-offer-design` ← **Produto ausente**, `gtm-consulting-pricing` ← **Preço ausente** | skill-creator | +2 | ⏳ |
| **9** | Consulting | `gtm-consulting-icp`, `gtm-consulting-designer` | skill-creator | +2 | ⏳ |
| **10** | Consulting | `gtm-consulting-qualifier`, `gtm-consulting-briefing`, `gtm-consulting-pursuit`, `gtm-consulting-cosell`, `gtm-consulting-expansion`, `gtm-consulting-workflow` | skill-creator | +6 | ⏳ |
| **11** | Release | v1.27.0, 74 skills, docs sync, GitHub push | scripts | — | ⏳ |
| | | **Total novos** | | **+19** | |

**Cobertura dos 4Ps após execução:**

| P | Produto GTM | Consultoria GTM |
|---|-------------|-----------------|
| **Praça** | gtm-market-sizing + gtm-icp-designer + gtm-product-outreach + gtm-product-demand-gen | gtm-market-sizing + gtm-consulting-icp + gtm-consulting-cosell |
| **Produto** | product-strategy + gtm-product-growth + gtm-product-launch | gtm-consulting-offer-design |
| **Preço** | gtm-product-pricing | gtm-consulting-pricing |
| **Promoção** | gtm-positioning + gtm-product-launch | gtm-positioning + gtm-consulting-designer |
