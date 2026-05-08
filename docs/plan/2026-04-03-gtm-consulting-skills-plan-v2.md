# GTM Consulting Skills — Implementation Plan v2

> **For Claude:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.

**Goal:** Adicionar 7 skills ao claude-superskills cobrindo o ciclo completo de GTM para consulting Microsoft — do ICP ao co-sell, da qualificação à expansão de conta. Fills a gap not covered by `abx-strategy`, `product-strategy`, or `startup-growth-strategist`.

**Architecture:** Single-source-of-truth model. Cada skill vive em `skills/<name>/SKILL.md` + `skills/<name>/README.md`. Frontmatter mínimo (name, description, license only). Sem version bump até os skills estarem completos e validados. Implementação em 2 batches com gate de aprovação entre eles.

**Tech Stack:** Markdown skill files, YAML frontmatter, validation scripts (`validate-skill-yaml.sh`, `validate-skill-content.sh`), Python index generators.

**Context:** Eric Andrade, VP/Client Solutions Lead, Avanade Brazil. Responsável por criar e melhorar GTMs para ofertas de consulting Microsoft (Assessment, System Integration, Outsourcing, Shared Services). Segmentação operacional interna: MVC / High Growth / Emerging / Whitespace. Pillars: AI Business Solutions | Security | Cloud & AI Platforms | Advisory.

**Reference documents used in this plan:**
- `GTM_Mastery_Avanade.md` — 8 GTM pillars, coverage model, co-sell strategy, common GTM gaps
- `MASTER_21_Frameworks_Vendas.md` — 21 sales frameworks adapted to Avanade context with scripts

---

## Session Context (Estado da Sessão em 2026-04-03)

### Status atual

- ✅ Plano v2 escrito e salvo neste arquivo
- ✅ Plano v1 (`2026-04-03-gtm-consulting-skills-plan.md`) ainda existe — supersedido por este v2
- ⏳ **Nenhum skill foi implementado ainda** — próximo passo é Batch 1
- ⏳ Batch 1 aguarda aprovação/execução: `microsoft-consulting-icp` + `consulting-gtm-designer`
- ⏳ Batch 2 aguarda Batch 1 completo + review

### Próximo passo imediato

Executar Batch 1 com `executing-plans`:
1. `skills/microsoft-consulting-icp/SKILL.md` + `README.md`
2. `skills/consulting-gtm-designer/SKILL.md` + `README.md`
3. Regenerar indexes
4. Validar com scripts

### Fluxo de uso dos skills (como o Eric usaria no dia a dia)

```
ESTRATÉGIA (trimestral)
    ↓
microsoft-consulting-icp        → "Quem devo perseguir?" (ICP por tier × oferta)
consulting-gtm-designer         → "Como vendo para eles?" (GTM Canvas)
microsoft-cosell-strategy       → "Como alavanco a Microsoft?" (co-sell + funding)

EXECUÇÃO (por oportunidade)
    ↓
presales-qualifier              → "Vale investir pre-sales neste deal?"
executive-account-briefing      → "Como me preparo para a reunião C-level?"
consulting-pursuit-governance   → "Como governo este pursuit ativo?"

EXPANSÃO (por conta existente)
    ↓
consulting-account-expansion    → "Como cresço revenue nesta conta?"
```

### Repositórios externos identificados (não triados ainda)

O Eric identificou 10 repositórios públicos de GTM skills para análise futura. Estes podem ser relevantes para **GTM de produto (PRD)** — não para consulting services, que é o foco deste plano.

**Repositórios para triage futura (PRD GTM):**
- https://github.com/iamachilles/gtm-skills-os/tree/main/skills
- https://github.com/kenny589/gtm-flywheel
- https://github.com/gooseworks-ai/goose-skills
- https://github.com/GTM-Strategist/gtm-strategist-skills
- https://github.com/gtmagents/gtm-agents
- https://github.com/onvoyage-ai/gtm-engineer-skills
- https://github.com/sachacoldiq/ColdIQ-s-GTM-Skills
- https://github.com/ivangfalco/gtm-skills
- https://github.com/Prospeda/gtm-skills
- https://github.com/Othmane-Khadri/gtm-agent-playbook

**Avaliação inicial (sem triage completa):**
- Os docs `GTM_Mastery_Avanade.md` e `MASTER_21_Frameworks_Vendas.md` são suficientes para os 7 skills de consulting deste plano
- Para skills de **PRD GTM** (SaaS, PLG, trial conversion, ARR/NRR metrics), os repositórios acima têm mais potencial
- Os 21 frameworks têm ingredientes úteis para PRD também (JOLT para trial-to-paid, SPIN para demo, GAP Selling para demo narrative) — mas base estrutural precisaria de referências de produto (April Dunford, Wes Bush)
- Triage desses repos deve acontecer em sessão separada, seguindo o processo do `obsidian-skills-adoption-plan.md` como modelo

### Decisões de design fixadas (não reverter)

1. **Segmentação primária:** MVC / High Growth / Emerging / Whitespace (não Enterprise/SMC)
2. **Hero Offer:** opção recomendada para Whitespace/Emerging — NUNCA gate obrigatório
3. **Arc Assessment → SI → Outsourcing:** caminho possível, não sequência forçada
4. **Diferenciação de abx-strategy:** esse skill é para B2B genérico; estes são para Microsoft consulting
5. **Framework source:** os 21 frameworks são usados como ingredientes adaptados ao contexto Avanade, não copiados verbatim

### Regras de frontmatter (crítico — Claude Code quebra se errar)

```yaml
---
name: kebab-case-name
description: This skill should be used when... (single line)
license: MIT
---
```

**PROIBIDO em SKILL.md:** `version`, `author`, `platforms`, `category`, `tags`, `risk`, `created`, `updated` — causam erro de loading no Claude Code.
**Esses campos vão em README.md** na tabela `## Metadata`.

---

## Segmentação Operacional (Base para Todos os Skills)

| Tier | Critério | Motion |
|------|----------|--------|
| **MVC** | $5M+ Revenue com Avanade | BDE (Business Development Executive). Named coverage. Full pursuit governance. |
| **High Growth** | $1.1M–$4.9M Revenue | BDE. Named coverage. Expansion-focused. |
| **Emerging** | >$1M Revenue (crescimento esperado) | DDC (Digital Demand Center) + BDE híbrido |
| **Whitespace** | Net new — sem relacionamento | DDC. Digital first. Hero Offer como entry opcional. |

> **Hero Offer rule:** Hero Offers (fixed-fee, $30–75K, 2–6 semanas) são **opção de entrada recomendada para Whitespace e Emerging**, não gate obrigatório. Cliente pode comprar SI direto se o contexto permitir. O importante é vender.

---

## Decision Gates

1. ✅ Batch 1 skills aprovados (microsoft-consulting-icp, consulting-gtm-designer)
2. ⏳ User approves Batch 1 após implementação e review
3. ⏳ User approves Batch 2 antes de implementar (presales-qualifier, consulting-pursuit-governance, executive-account-briefing, microsoft-cosell-strategy, consulting-account-expansion)

---

## Batch 1: Skills Prioritários

### Skill 1: `microsoft-consulting-icp`

**Purpose:** Define ICP para engagements de consulting Microsoft segmentado por tier de conta (MVC/HG/Emerging/Whitespace) e tipo de oferta. Inclui buyer persona mapping por oferta, PURE scoring adaptado ao contexto Avanade/Microsoft, e Anti-ICP para proteger pipeline quality.

**Why not `abx-strategy`?** `abx-strategy` target B2B genérico com <500 contas e SaaS. Este skill é específico para: Microsoft platform consulting, dinâmica do ecossistema Avanade/Microsoft, segmentação MVC/HG/Emerging/Whitespace, e diferenças no buyer committee entre um CIO comprando Assessment vs. CFO aprovando Outsourcing de 3 anos.

**Frameworks integrados:** PURE (scoring), Strategic Selling / Miller Heiman (Economic Buyer, User Buyer, Technical Buyer, Coach), TAS quadrant model (INVEST/DEVELOP/HARVEST/OPPORTUNISTIC).

**Trigger phrases:**
- "Quem é meu ICP para Assessment no MVC?"
- "Qual é o perfil de conta ideal para Outsourcing em High Growth?"
- "Ajuda a definir ICP para minha oferta de SI no Whitespace"
- "Preciso mapear o comitê de compra para uma conta MVC de Shared Services"
- "Define Anti-ICP para nossa oferta de Cloud no Emerging"

---

#### Task 1.1: Create `skills/microsoft-consulting-icp/SKILL.md`

**Files:**
- Create: `skills/microsoft-consulting-icp/SKILL.md`

**Step 1: Create file with correct frontmatter**

```yaml
---
name: microsoft-consulting-icp
description: This skill should be used when the user needs to define Ideal Customer Profiles for Microsoft platform consulting offers. Use when segmenting accounts by tier (MVC, High Growth, Emerging, Whitespace), mapping buyer committees per offer type (Assessment, System Integration, Outsourcing, Shared Services), scoring opportunity fit with PURE model, or defining Anti-ICP to protect pipeline quality.
license: MIT
---
```

**MANDATORY frontmatter rules:**
- ONLY `name`, `description`, `license` — NO other fields
- NO `version`, `author`, `platforms`, `category`, `tags`, `risk`, `created`, `updated`
- `name` kebab-case, `description` single line

**Step 2: Write skill body**

Required sections in order: Purpose → When to Use → Progress Tracking → Workflow → Critical Rules → Example Usage

**Full SKILL.md body:**

```markdown
# Microsoft Consulting ICP

Define Ideal Customer Profiles para engagements de consulting Microsoft, segmentado por tier de conta e tipo de oferta.

## Purpose

Define Ideal Customer Profiles (ICPs) para engagements de consulting Microsoft. Segmenta contas pelos tiers operacionais Avanade (MVC, High Growth, Emerging, Whitespace), mapeia buyer personas por tipo de oferta, aplica PURE scoring adaptado ao contexto de serviços profissionais, e produz Anti-ICP explícito para proteger win-rate e qualidade de pipeline.

## When to Use

- Designing or refreshing GTM for a specific consulting offer
- Qualifying whether an account is viable for a given offer type and tier
- Mapping the buying committee before engaging an account
- Reviewing pipeline quality against ICP criteria
- Training pre-sales teams on qualification standards
- Deciding whether to pursue a Whitespace account via Hero Offer vs. direct SI motion

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 25% — Phase 1/4: Account Tier Classification
[████████░░░░░░░░░░░░] 50% — Phase 2/4: Offer-Fit Scoring (PURE)
[████████████░░░░░░░░] 75% — Phase 3/4: Buyer Committee Mapping
[████████████████████] 100% — Phase 4/4: Anti-ICP Definition
```

## Workflow

### Phase 1: Account Tier Classification

Classify the account using Avanade's operational segmentation:

| Tier | Revenue Signal | Relationship Signal | Motion |
|------|---------------|---------------------|--------|
| **MVC** | $5M+ with Avanade | Named, strategic, exec relationship active | BDE-led. Full pursuit governance. Priority co-sell. |
| **High Growth** | $1.1M–$4.9M | Named, expansion runway visible | BDE-led. Expansion-focused. Upsell plays active. |
| **Emerging** | >$1M potential | Limited or early relationship | DDC + BDE hybrid. Assessment or Hero Offer as entry. |
| **Whitespace** | No current revenue | No relationship | DDC-led. Digital first. Hero Offer as optional entry point. |

Collect firmographic signals: revenue, employee count, IT budget, Microsoft maturity (M365/Azure adoption level), existing Avanade relationship depth, vertical (FSI, HPS, CMT, PRD, ERG, EXP).

Apply TAS quadrant logic to MVC/HG accounts:
- **INVEST:** High strategic value + strong relationship → full coverage
- **DEVELOP:** High strategic value + weak relationship → relationship building first
- **HARVEST:** Lower strategic value + strong relationship → efficiently monetize, standardize
- **OPPORTUNISTIC:** Lower strategic value + weak relationship → pursue only if inbound or low-cost entry

### Phase 2: Offer-Fit Scoring (PURE Model)

Score fit for each offer type using PURE:

| Criterion | Question | Score 0–3 |
|-----------|----------|-----------|
| **P**ain | How acute is the operational/strategic pain driving this purchase? | 0=mild, 3=severe |
| **U**rgency | Is there a forcing function (regulatory deadline, exec mandate, contract expiring)? | 0=none, 3=imminent |
| **R**eadiness | Does the organization have budget approved and governance to buy? | 0=not ready, 3=ready now |
| **E**conomics | Is there visible ROI or cost reduction that justifies the offer price? | 0=unclear, 3=explicit |

**Offer-specific fit signals:**

- **Assessment ($15K–$80K):** CIO/CTO sponsor exists; technology debt visible; Azure or M365 already adopted; digital transformation pressure active; budget for discovery allocated. Also valid as Hero Offer for Whitespace/Emerging entry.
- **System Integration ($150K–$2M+):** Budget confirmed; project scope definable; Microsoft platform selected or in active evaluation; decision expected within quarter; IT governance in place. Can be first offer if relationship and qualification support it — Hero Offer NOT required as gate.
- **Outsourcing (multi-year):** CFO involved; operational cost pressure explicit; FTE headcount at risk or restructuring active; multi-year commitment appetite exists; SLA expectations defined.
- **Shared Services:** CSC or GBS structure exists or planned; process standardization mandate active; scale across geographies required; CFO/COO sponsorship.

**Tier assignment:**
- 10–12: Tier 1 — pursue actively, full pre-sales investment
- 7–9: Tier 2 — qualify further before investing
- 4–6: Tier 3 — nurture, no pre-sales investment
- 0–3: Discard

### Phase 3: Buyer Committee Mapping

Map using Strategic Selling (Miller Heiman) roles:

| Role | Definition | Per Offer Type |
|------|-----------|----------------|
| **Economic Buyer** | Final financial authority | Assessment: CIO/CFO; SI: CIO+CFO; Outsourcing: CFO; Shared Services: CFO/COO |
| **User Buyer** | Day-to-day operational impact | Assessment: IT Director; SI: Project Sponsor; Outsourcing: COO/Ops; Shared Services: Process Owner |
| **Technical Buyer** | Technical evaluation and risk | Assessment: IT Architecture; SI: IT Architecture+Security; Outsourcing: IT+Legal; Shared Services: IT+HR |
| **Coach** | Internal advocate with political will | Must identify before investing pre-sales hours |

For each stakeholder document: title, primary concern, success metric, likely objection, messaging angle, engagement status (Cold/Warm/Engaged).

Champion identification rule: No named coach = no pre-sales investment beyond initial qualification call.

### Phase 4: Anti-ICP Definition

Define explicitly who NOT to pursue per offer type and tier:

**Universal Anti-ICP signals (disqualify regardless of tier):**
- No Microsoft investment (no M365, no Azure, competitive lock-in)
- Procurement-only contact without executive sponsor identified
- Budget not allocated with approval cycle >18 months
- No internal champion or coach accessible
- RFP-only relationship without prior engagement
- Single-vendor mandate to a direct competitor
- Active contract with competing SI at full capacity

**Tier-specific Anti-ICP:**
- **Whitespace:** No digital transformation signal, purely price-driven RFP, no Microsoft partner ecosystem engagement
- **Emerging:** Leadership not aligned on technology investment, no budget owner reachable through DDC
- **MVC/HG:** Relationship stalled at IT level only with no path to economic buyer, previous delivery issue unresolved

## Critical Rules

- NEVER score an account without identifying a named executive sponsor — no sponsor = Discard regardless of firmographics
- NEVER conflate tier motions — MVC and Whitespace have fundamentally different buying governance, cycle length, and commercial structure
- ALWAYS define Anti-ICP before presenting ICP to a pre-sales team — without it, the ICP will be gamed
- ALWAYS map the buyer committee before proposing — knowing only the IT contact is insufficient for any offer beyond a small Assessment
- NEVER use generic ICP criteria — every output must reference the specific account tier AND offer type being evaluated
- NEVER force Hero Offer as a prerequisite for SI — if the account context supports direct SI qualification, proceed

## Example Usage

1. "Define ICP para Assessment no tier MVC no setor FSI — quero entrar no Bradesco com uma nova oferta de AI"
2. "Score esta conta para Outsourcing: High Growth, setor CMT, $2M potencial, CFO recém-trocado, LGPD mandatório"
3. "Mapeia o comitê de compra para um projeto de SI de $500K no Safra — já temos o IT Director como contato"
4. "Revisa meu pipeline Emerging — aplica Anti-ICP e me diz quais oportunidades descartar"
5. "Qual é o perfil de conta Whitespace que justifica uma Hero Offer de AI Assessment antes de propor SI?"
```

**Step 3: Validate**

```bash
./scripts/validate-skill-yaml.sh skills/microsoft-consulting-icp
./scripts/validate-skill-content.sh skills/microsoft-consulting-icp
```
Expected: no errors, word count 1500–2000

---

#### Task 1.2: Create `skills/microsoft-consulting-icp/README.md`

**Files:**
- Create: `skills/microsoft-consulting-icp/README.md`

**Full README.md:**

```markdown
# Microsoft Consulting ICP

Define Ideal Customer Profiles for Microsoft platform consulting offers by account tier and offer type.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Sales |
| Tags | gtm, icp, microsoft, consulting, avanade, mvc, high-growth, whitespace, presales |
| Risk | Low |

## Overview

Designed for Avanade Microsoft consulting practices. Maps the ICP question across Avanade's four operational account tiers (MVC, High Growth, Emerging, Whitespace) and four offer types (Assessment, System Integration, Outsourcing, Shared Services).

Produces a structured ICP profile including: PURE opportunity scoring, buyer committee mapping using Strategic Selling roles (Economic Buyer, User Buyer, Technical Buyer, Coach), and Anti-ICP definition to protect win-rate and pre-sales investment quality.

Different from generic B2B ICP tools — this skill is built around the Microsoft/Avanade co-sell model, Brazilian enterprise verticals (FSI, HPS, CMT, PRD), and the specific governance and buying dynamics of consulting services vs. software products.

## When to Use

- Designing or refreshing GTM for a consulting offer
- Qualifying a specific account/opportunity before investing pre-sales hours
- Mapping buyer committees for pursuit governance
- Reviewing pipeline against ICP criteria
- Training pre-sales and BDE teams on qualification standards

## Example Outputs

**ICP Profile:** Account Tier + Offer Type → PURE Score → Committee Map → Recommended Motion

**Anti-ICP Checklist:** Universal disqualifiers + tier-specific signals → Go/No-Go recommendation

**TAS Quadrant Assignment:** INVEST / DEVELOP / HARVEST / OPPORTUNISTIC for MVC/HG accounts
```

---

### Skill 2: `consulting-gtm-designer`

**Purpose:** Build GTM strategy for Microsoft platform consulting offers targeting Avanade's account tiers. Produz GTM canvas por tipo de oferta com value prop por pilar, motion de entrada, arc de expansão, e win metrics.

**Why not `product-strategy`?** `product-strategy` é para empresas de produto (PLG vs SLG, pricing tiers, freemium). Este skill é para serviços profissionais: packaging de oferta, motions de consulting, governança de pre-sales, arc Assessment → SI → Outsourcing como expansão estratégica.

**Hero Offer rule:** Hero Offer é opção de entrada recomendada para Whitespace/Emerging — não é gate obrigatório. O skill apresenta a arc Assessment → SI → Outsourcing como caminho possível, não sequência forçada.

**Frameworks integrados:** Force Management (Required Capabilities, Proof Points, Business Outcomes), GAP Selling (Estado Atual → Estado Futuro → Impacto do Gap), Challenger Selling (Tailor pillar para value prop executiva).

**Trigger phrases:**
- "Monta o GTM para AI Assessment no tier MVC — setor FSI"
- "Como entrar em contas Whitespace com oferta de Cloud & AI?"
- "Estrutura o GTM para Outsourcing no High Growth — setor CMT"
- "Qual é a estratégia de expansão de um Assessment para SI no Bradesco?"
- "Define value prop por pilar para Shared Services no segmento HPS"

---

#### Task 2.1: Create `skills/consulting-gtm-designer/SKILL.md`

**Files:**
- Create: `skills/consulting-gtm-designer/SKILL.md`

**Step 1: Create file with correct frontmatter**

```yaml
---
name: consulting-gtm-designer
description: This skill should be used when the user needs to build a Go-To-Market strategy for Microsoft platform consulting offers. Use when designing entry motion by account tier, structuring value propositions per GTM pillar, planning the consulting offer expansion arc, or defining win metrics for consulting pipeline at Avanade or similar Microsoft partner organizations.
license: MIT
---
```

**Step 2: Write skill body**

```markdown
# Consulting GTM Designer

Build Go-To-Market strategy for Microsoft platform consulting offers by account tier and offer type.

## Purpose

Builds complete GTM strategy for Microsoft platform consulting offers (Assessment, System Integration, Outsourcing, Shared Services) across Avanade's account tiers (MVC, High Growth, Emerging, Whitespace). Covers offer positioning, value proposition by Microsoft GTM pillar, entry motion design, expansion arc planning, pre-sales governance gates, and win metrics. Produces a GTM canvas per offer-tier combination that a sales team can execute without ambiguity.

## When to Use

- Designing or refreshing GTM for a specific consulting offer type and account tier
- Defining value proposition differentiation across Microsoft pillars (AI Business Solutions, Security, Cloud & AI Platforms, Advisory)
- Planning entry motion for a new account tier or vertical
- Structuring expansion arc from entry offer to larger engagements
- Reviewing pipeline strategy alignment with account tier coverage model
- Deciding whether Hero Offer is the right entry motion for a specific context

## Progress Tracking

```
[████░░░░░░░░░░░░░░░░] 25% — Phase 1/4: Offer × Tier Definition
[████████░░░░░░░░░░░░] 50% — Phase 2/4: Value Proposition by Pillar
[████████████░░░░░░░░] 75% — Phase 3/4: Entry and Expansion Motion
[████████████████████] 100% — Phase 4/4: Win Metrics and GTM Canvas
```

## Workflow

### Phase 1: Offer × Tier Definition

Identify the combination to design GTM for:

**Offer types:**
- **Assessment** ($15K–$80K, 2–6 weeks): Discovery, diagnosis, roadmap. Entry offer. Fixed-fee possible. Also functions as Hero Offer when packaged for Whitespace/Emerging digital-first motion.
- **System Integration** ($150K–$2M+, 3–18 months): Build, configure, deploy. Can be first offer in MVC/HG if direct qualification supports it — Assessment NOT required as gate.
- **Outsourcing** (multi-year, recurring revenue): Operate, manage, run. Expansion offer. Requires delivery credibility or reference.
- **Shared Services** (multi-year, cross-geography): Standardize and centralize processes. Strategic expansion. Requires executive sponsorship and GBS/CSC maturity at client.

**Account tier implications for GTM design:**

| Tier | Sales Motion | Typical Entry | Pre-Sales Budget | Cycle Length |
|------|-------------|--------------|------------------|--------------|
| MVC | BDE-led, relationship-first, exec access | Assessment or direct SI | High — justify with PURE Tier 1 | 3–12 months |
| High Growth | BDE-led, expansion-focused | Assessment or SI | Medium — qualify at PURE Tier 2+ | 2–9 months |
| Emerging | DDC + BDE hybrid | Hero Offer or Assessment | Low — DDC digital first | 2–6 months |
| Whitespace | DDC-led, digital first | Hero Offer (optional) or direct SI if qualified | Minimal until qualified | 1–4 months |

Define scope: what is included in the offer, what is explicitly out, typical team composition, commercial structure (T&M, Fixed Fee, or Hybrid).

### Phase 2: Value Proposition by Pillar

For each Microsoft GTM pillar, build value prop specific to the offer type and account tier. Apply GAP Selling: Estado Atual (current pain) → Estado Futuro (desired outcome) → Impacto do Gap (cost of doing nothing).

**AI Business Solutions:**
- Assessment VP: "Identify where AI agents can deliver measurable productivity gains in your operations — with a vendor-agnostic roadmap before committing to implementation."
- SI VP: "Deploy Copilot and AI agents with tested integration to your workflows — measurable ROI in 90 days or defined kill criteria."
- Outsourcing VP: "Operate AI-augmented processes with guaranteed SLAs — you focus on business outcomes, we run the stack."
- Proof points: Copilot deployment outcomes, agent automation ROI, process time reduction metrics.

**Security:**
- Assessment VP: "Map your actual exposure under LGPD, BACEN, or sector regulation — specific gaps, not theoretical risk."
- SI VP: "Deploy Zero Trust architecture integrated with your Microsoft stack — compliance milestone achieved by [date]."
- Outsourcing VP: "24/7 managed security posture with SLA-backed incident response — your compliance is our contractual obligation."
- Proof points: LGPD compliance delivery, BACEN resolution, HIMSS for healthcare, incident cost avoidance.

**Cloud & AI Platforms:**
- Assessment VP: "Quantify migration ROI, identify hidden on-premise costs, and produce an Azure roadmap with TCO comparison."
- SI VP: "Execute cloud migration with validated architecture — zero business disruption, predictable cost to completion."
- Outsourcing VP: "Managed Azure operations at optimized unit economics — ongoing FinOps included."
- Proof points: migration ROI delivered, cloud unit economics reduction, platform modernization speed.

**Advisory:**
- Assessment VP: "Produce strategic clarity on your technology agenda — executive alignment, prioritized roadmap, investment case."
- SI VP: "Strategic governance alongside delivery — ensures the program stays aligned to business outcomes, not just technical milestones."
- Proof points: executive adoption rate, roadmap quality scored at delivery, decision velocity improvement.

Translate each value prop into buyer-specific language using Challenger Selling (Tailor):
- CIO: technical capability + delivery risk reduction
- CFO: ROI, payback period, cost avoidance
- COO: operational outcomes, SLA, FTE impact
- CDO/Innovation: speed to value, competitive positioning

### Phase 3: Entry and Expansion Motion

**Entry motion by tier:**

| Tier | Entry Motion | Recommended First Offer |
|------|-------------|------------------------|
| MVC | Executive-led. Long relationship cycle. Co-sell with Microsoft ATU. | Assessment or direct SI depending on pipeline maturity |
| High Growth | BDE-qualified. Expansion-first thinking from day 1. | Assessment to reduce risk before SI commitment |
| Emerging | DDC digital touch. Low-touch qualification. | Hero Offer (packaged Assessment) or Assessment |
| Whitespace | DDC-led. No relationship yet. | Hero Offer as optional entry — OR direct SI if an inbound qualified opportunity exists |

**Hero Offer design (when applicable):**
- Fixed-fee, $30–75K, 2–6 weeks
- Clear deliverable: diagnostic report, roadmap, architecture recommendation
- Designed to reduce perceived risk for first engagement
- Must have defined success criteria that create natural SI proposal opportunity
- NOT a mandatory gate — if client wants SI directly and qualification supports it, proceed

**Expansion arc (Assessment → SI → Outsourcing):**
- Arc is a strategic path, not a required sequence
- Assessment exit criteria → SI: client has approved roadmap, budget allocated, scope defined
- SI exit criteria → Outsourcing: delivery quality validated, operational handover defined, multi-year appetite confirmed
- Expansion timing triggers: delivery milestone, regulatory event, leadership change, contract renewal, M&A activity

**Pre-sales investment governance:**
- PURE Tier 1 (10–12): full pre-sales investment authorized
- PURE Tier 2 (7–9): limited investment — qualification call + brief proposal; escalate if champion confirmed
- PURE Tier 3 (4–6): no pre-sales hours; nurture via DDC
- PURE Tier 4 (0–3): disqualify

### Phase 4: Win Metrics and GTM Canvas

**Win metrics by offer type:**

| Offer | Leading Metric | Lagging Metric | Health Signal |
|-------|---------------|----------------|---------------|
| Assessment | Conversion to SI proposal (target: >40%) | Time-to-proposal | Client NPS post-delivery |
| System Integration | Win rate in competitive eval | Delivery margin | Reference-ability post-delivery |
| Outsourcing | Contract duration | FTE displacement ratio | SLA achievement + renewal rate |
| Shared Services | Process coverage % | Cost per process | Cross-geography expansion |

**GTM Canvas output (1-page per offer × tier):**

```
┌─────────────────────────────────────────────────────────────┐
│ GTM CANVAS: [Offer Type] × [Account Tier]                   │
├─────────────────────────────────────────────────────────────┤
│ OFFER DEFINITION: [Scope, duration, commercial structure]    │
├───────────────────────────┬─────────────────────────────────┤
│ TARGET ICP (from         │ ANTI-ICP                         │
│ microsoft-consulting-icp) │                                  │
├───────────────────────────┴─────────────────────────────────┤
│ VALUE PROP BY PILLAR: [AI Biz / Security / Cloud / Advisory] │
├───────────────────────────┬─────────────────────────────────┤
│ ENTRY MOTION              │ HERO OFFER? [Yes/No + rationale] │
├───────────────────────────┴─────────────────────────────────┤
│ EXPANSION ARC: [Next offer trigger criteria]                 │
├───────────────────────────┬─────────────────────────────────┤
│ PRE-SALES GATE (PURE)     │ WIN METRICS                      │
└───────────────────────────┴─────────────────────────────────┘
```

## Critical Rules

- NEVER design GTM without specifying the offer type AND account tier — the motion is fundamentally different across combinations
- NEVER produce value prop without translating it into buyer-specific language — CFO hears different value than CIO
- NEVER force the Assessment → SI sequence — if direct SI qualification is solid, proceed
- ALWAYS include expansion arc — an Assessment without expansion path is a one-time transaction, not GTM
- ALWAYS define pre-sales governance gate — without it, pre-sales hours will be spent on Tier 3/4 accounts
- NEVER ignore Microsoft co-sell potential — Avanade/Microsoft co-sell relationship is a differentiation asset
- ALWAYS identify whether Hero Offer is justified by context, not default

## Example Usage

1. "Monta GTM Canvas para AI Business Solutions Assessment no tier MVC — setor FSI, entering Bradesco"
2. "Estrutura a expansão de um Assessment de Cloud para SI de $500K no Safra — o que precisa ser verdade?"
3. "Define value prop de Security para High Growth no setor CMT — para CFO e CIO"
4. "Revisa GTM de Outsourcing no Emerging — onde está o gap entre nossa motion e o que o mercado precisa?"
5. "Cria GTM Canvas para Shared Services no HPS — Advisory pillar, tier MVC, entrada já feita via Assessment"
```

**Step 3: Validate**

```bash
./scripts/validate-skill-yaml.sh skills/consulting-gtm-designer
./scripts/validate-skill-content.sh skills/consulting-gtm-designer
```
Expected: no errors

---

#### Task 2.2: Create `skills/consulting-gtm-designer/README.md`

**Files:**
- Create: `skills/consulting-gtm-designer/README.md`

```markdown
# Consulting GTM Designer

Build Go-To-Market strategy for Microsoft platform consulting offers by account tier and offer type.

## Metadata

| Field | Value |
|-------|-------|
| Version | 1.0.0 |
| Author | Eric Andrade |
| Created | 2026-04-03 |
| Updated | 2026-04-03 |
| Platforms | Claude Code, GitHub Copilot, OpenAI Codex, Gemini CLI, Cursor IDE |
| Category | GTM & Sales |
| Tags | gtm, consulting, microsoft, avanade, assessment, si, outsourcing, shared-services, value-prop |
| Risk | Low |

## Overview

Designed for Avanade and Microsoft partner consulting practices. Builds complete GTM strategy for the four offer types (Assessment, System Integration, Outsourcing, Shared Services) across the four operational account tiers (MVC, High Growth, Emerging, Whitespace).

Produces: offer definition, value proposition translated per Microsoft pillar and buyer persona, entry motion by tier, expansion arc with exit criteria, pre-sales governance gates using PURE scoring, and win metrics per offer type.

Explicitly designed around the reality that Hero Offer is an optional entry tool — not a mandatory gate — and that direct SI qualification is valid when supported by evidence.

## When to Use

- Designing or refreshing GTM for a consulting offer × account tier combination
- Translating Microsoft pillar messaging into buyer-specific value propositions
- Planning entry motion for new accounts or new verticals
- Structuring the Assessment → SI → Outsourcing expansion arc
- Governing pre-sales investment with PURE-based go/no-go criteria

## Example Outputs

**GTM Canvas:** 1-page canvas per offer × tier combination with ICP, value prop, entry motion, expansion arc, and win metrics

**Value Prop Translation:** Per pillar (AI Biz Solutions, Security, Cloud & AI, Advisory) × per buyer (CIO, CFO, COO)

**Expansion Arc:** Exit criteria, timing triggers, and governance gates between offer stages
```

---

## Batch 2: Advanced Skills (pending Batch 1 approval)

> **Gate:** Implement Batch 2 only after Batch 1 (microsoft-consulting-icp + consulting-gtm-designer) is reviewed and approved.

---

### Skill 3: `presales-qualifier`

**Purpose:** Framework de qualificação de pré-vendas para deals de consulting Microsoft. Combina BANT consultivo (DDC SAL process) + MEDDPICC adaptado + go/no-go estruturado. Governa investimento de horas de pré-vendas e define kill conditions.

**Frameworks integrados:** BANT (DDC qualification for SAL handoff), MEDDPICC (enterprise deal governance: Metrics, Economic Buyer, Decision Criteria, Decision Process, Identify Pain, Champion, Competition), SPICED (Critical Event for presales timing), Sandler Pain 3 levels (surface pain → business impact → personal consequence).

**Trigger phrases:**
- "Qualifica esta oportunidade de Outsourcing para o Bradesco"
- "Qual é o go/no-go para investir pre-sales nesta conta MVC?"
- "Nosso win-rate está baixo — ajuda a revisar critérios de qualificação"
- "MEDDPICC completo para esta oportunidade de SI de $800K"
- "Define kill conditions para este deal — já temos 3 meses sem avanço"

**Content specification:**

Phase 1: BANT Consultivo (DDC SAL Qualification)
- Budget: orçamento confirmado ou aprovação prevista (pergunta certa: "Qual é o processo de aprovação orçamentária para este projeto?")
- Authority: economic buyer identificado e acessível (pergunta certa: "Quem assina o contrato? Já conversamos com esta pessoa?")
- Need: dor específica articulada com impacto financeiro (não apenas "interesse")
- Timeline: forcing function presente (Sandler: qual é a consequência de não resolver até [data]?)
- Output: SAL qualification score → MQL vs. SQL decision

Phase 2: MEDDPICC Scoring (Enterprise Deals)
- Metrics: ROI quantificado com baseline (antes/depois)
- Economic Buyer: mapeado, acessível, engajado
- Decision Criteria: critérios de avaliação do cliente documentados
- Decision Process: processo de compra entendido, próximos passos claros
- Identify Pain: dor de negócio validada com Sandler 3 levels
- Champion: coach interno com credibilidade política identificado
- Competition: landscape competitivo mapeado, posicionamento definido
- Score: 0–3 por dimensão. Total ≥ 18 = pursue actively. 12–17 = qualify further. <12 = disqualify or nurture

Phase 3: Go/No-Go Decision + Pre-Sales Governance
- Pre-sales investment tiers by PURE + MEDDPICC combined score
- Kill conditions: define explicit signals to stop investing (3+ months no progress, Economic Buyer inaccessível, champion saiu, competidor bloqueou)
- SPICED Critical Event: "Qual é o evento que torna este projeto urgente para o cliente?" — sem Critical Event = no urgency = long tail or dead pipeline

Phase 4: Win-Rate Tracking by Offer × Tier
- Track win-rate by: offer type, account tier, vertical, lead source (inbound, co-sell, outbound)
- Identify pattern: onde a qualificação está falhando? Pipeline inflado de Tier 3/4?
- Recommendation: calibrar ICP com dados de win-rate real

**Critical Rules:**
- NEVER approve pre-sales investment without identified Champion
- NEVER skip SPICED Critical Event question — no urgency = no deal in reasonable timeframe
- ALWAYS score MEDDPICC explicitly — gut feel is not qualification
- ALWAYS define kill conditions at deal entry, not mid-pursuit
- NEVER conflate DDC SAL qualification with enterprise deal qualification — different rigor required

---

### Skill 4: `consulting-pursuit-governance`

**Purpose:** Governança de pursuits de consulting — scorecard de oportunidade, gates de investimento de pré-vendas, decisão Continue/Stop, e kill signals. Prevent pipeline inflation e bad deals.

**Frameworks integrados:** MEDDPICC (deal scorecard), Strategic Selling (buying influence map, win concession strategy), Conceptual Selling (UFC — Understanding, Fulfill, Commit), JOLT Effect (quando cliente está paralyzed por risco — quando parar de esperar).

**Trigger phrases:**
- "Monta o pursuit scorecard para esta oportunidade de $1.2M"
- "Qual é a decisão go/no-go para continuar investindo pre-sales no Cielo?"
- "Identifica kill signals neste deal — já temos 5 meses de stall"
- "Como estruturo a governança de pursuit para oportunidades acima de $500K?"
- "O cliente está travado, não decide — como aplicar JOLT nesta situação?"

**Content specification:**

Phase 1: Pursuit Scorecard
- Opportunity dimensions: strategic value (tier × offer size), win probability (MEDDPICC score), delivery capacity (can we staff and deliver?), reference value (will client be referenceable?)
- Score 0–3 each dimension, weighted by offer type
- Go/No-Go threshold per tier: MVC = pursue at lower win prob threshold (strategic), Whitespace = pursue only if high win prob

Phase 2: Pre-Sales Investment Gates
- Gate 0 → Gate 1 (Initial qualification): BANT + sponsor identified → authorize discovery call
- Gate 1 → Gate 2 (Proposal): MEDDPICC ≥ 14 + champion confirmed + budget signal → authorize proposal investment
- Gate 2 → Gate 3 (Final proposal/orals): MEDDPICC ≥ 20 + Economic Buyer engaged + competitive position clear → full team investment
- Each gate: investment authorized, roles required, deliverables, timeline

Phase 3: Continue/Stop Decision Framework
- Continue signals: progress on MEDDPICC dimensions, champion active, timeline maintained, executive engagement
- Stop signals (JOLT Effect): >3 months no progress on decision criteria, Economic Buyer inaccessible, champion lost influence, competitor installed on parallel track, budget reallocated
- JOLT application: when client is paralyzed by risk — clarify "omission" (cost of not deciding) vs. "commission" (risk of deciding wrong); create safe path to decide
- Kill authorization: define who authorizes kill decision and when retrospective happens

Phase 4: Post-Pursuit Learning
- Win: what drove the win? Replicable pattern?
- Loss: which MEDDPICC dimension failed earliest? Where was the miss?
- No-decision: what caused paralysis? Can JOLT have helped?
- Quarterly review: win-rate by offer × tier, average pursuit cost by tier, kill ratio target

**Critical Rules:**
- NEVER advance a deal past Gate 2 without Economic Buyer engaged
- NEVER keep a deal alive past 4 months of no MEDDPICC progress — kill or suspend
- ALWAYS document kill decision rationale — prevents same mistake on next similar deal
- ALWAYS conduct post-pursuit retrospective within 2 weeks of win or loss
- NEVER conflate "client is slow" with "deal is alive" — JOLT or kill

---

### Skill 5: `executive-account-briefing`

**Purpose:** Gera briefings executivos para reuniões com C-level em contas Microsoft/Avanade. Perfil do executivo, agenda de reunião, perguntas de discovery calibradas por persona, e conversational hooks estratégicos.

**Frameworks integrados:** Challenger Selling (Tailor pillar — customize insight delivery to specific persona), Conceptual Selling (Personal Win — what does THIS executive win if this project succeeds?), SPIN Selling (Implication questions calibrated to executive level — business consequence, not technical detail).

**Trigger phrases:**
- "Prepara briefing executivo para reunião com CIO do Bradesco amanhã"
- "Quais são as perguntas certas para um CFO em primeira reunião de SI?"
- "Monta agenda para Executive Business Review com C-level do Safra"
- "Gera perfil executivo e conversational hooks para novo CDO da Cielo"
- "Prepara discovery questions para CFO que está avaliando Outsourcing"

**Content specification:**

Phase 1: Executive Profile Generation
- Role context: CIO vs. CFO vs. COO vs. CDO — different worldview, different success metrics, different fears
- LinkedIn + news research protocol: recent statements, strategic priorities, tenure, known initiatives, industry context
- Personal Win mapping: what does this executive win personally if this project succeeds? (Conceptual Selling)
- Risk profile: what is their biggest career risk right now? How does our offer mitigate or create risk for them?

Phase 2: Meeting Agenda Design
- Agenda structure by meeting type:
  - First contact (30 min): 5 min rapport → 10 min insight delivery (Challenger Tailor) → 10 min discovery questions → 5 min next step proposal
  - Executive Business Review (60 min): 5 min recap → 15 min business review → 15 min strategic insight → 15 min deep discovery → 10 min proposal → 5 min close/next step
- Challenger Tailor: prepare 1 insight specific to their industry + role that reframes their problem — not a product pitch

Phase 3: Discovery Questions by Persona
- CIO questions: "What is your biggest technology risk in the next 12 months?" / "Where is your current architecture limiting your business ambitions?" / "What does good look like for your team in 2 years?"
- CFO questions: "What is the cost of maintaining the current state for 3 more years?" / "How are you measuring technology ROI today?" / "What would need to be true for this investment to be approved?"
- COO questions: "Where are your operations most exposed to disruption?" / "What processes are consuming disproportionate FTE today?" / "What is your SLA reliability and where are the gaps?"
- CDO/Innovation questions: "What is your biggest obstacle to AI adoption in production?" / "Where are your experiments failing to scale?" / "What does your board expect from you on AI in the next 12 months?"

Phase 4: Strategic Hooks and Follow-Up Design
- Conversational hooks: 2–3 industry-specific observations that provoke thinking (Challenger insight delivery)
- SPIN Implication questions: move from situation (facts) to problem (acknowledgment) to implication (business consequence) to need-payoff (solution value)
- Meeting close: always leave with a defined next step, timeline, and responsible person — no open-ended "we'll follow up"
- Follow-up template: within 24 hours, 3 bullets — what we discussed, what we'll do next, what we need from them

**Critical Rules:**
- NEVER enter an executive meeting without knowing the Personal Win for that specific executive
- NEVER lead with product features — lead with insight about their industry or business
- ALWAYS define a specific next step before leaving the meeting
- ALWAYS calibrate SPIN questions to executive level — no technical detail unless explicitly requested
- NEVER use generic discovery questions — every question must reference their industry, role, or known context

---

### Skill 6: `microsoft-cosell-strategy`

**Purpose:** Estratégia de co-venda com Microsoft para contas Avanade. Mapeia papéis ATU/STU/CSU, avalia prontidão de co-sell, define engagement strategy por oferta, e identifica acesso a funding/incentivos (MACC, Solution Play funding, FastTrack, ADS).

**Frameworks integrados:** GTM Mastery Avanade — seção co-sell (co-sell como canal mais subutilizado), Solution Plays, Microsoft investment vehicles.

**Trigger phrases:**
- "Como engajar o ATU do Bradesco para co-sell na nossa proposta de AI?"
- "Qual é nossa prontidão de co-sell para esta oferta de Security?"
- "Identifica oportunidades de funding Microsoft para esta conta MVC"
- "Monta estratégia de co-venda com STU para deal de Cloud Migration de $800K"
- "Como acessar MACC ou ADS para este projeto de Outsourcing?"

**Content specification:**

Phase 1: Microsoft Account Team Mapping
- ATU (Account Team Unit — sales): owns the commercial relationship, manages Microsoft revenue targets, co-sell incentive is territory revenue
- STU (Specialist Team Unit — pre-sales): technical specialists per workload (Azure, Security, Modern Work, Business Applications), owns technical close
- CSU (Customer Success Unit — post-sale): ensures adoption and consumption, impacts renewal and expansion
- Map: who is the ATU for this account? Is the relationship warm or cold? What is their current priority for this account?
- Signal: ATU with active pipeline = highest co-sell alignment opportunity

Phase 2: Co-Sell Readiness Scoring
- Offer alignment: is our offer on the Microsoft Solution Plays list?
- Technical alignment: does our SI use Azure-native services? (consumption = co-sell credit for ATU)
- Deal size: is the deal large enough to warrant ATU time? (<$50K rarely gets ATU attention)
- Timing: is the ATU's fiscal quarter-end a leverage point?
- Score: High / Medium / Low co-sell readiness + recommended engagement approach

Phase 3: Engagement Strategy by Offer
- Assessment: invite STU to joint workshop — technical credibility + Microsoft brand
- SI: position as Solution Play delivery — ATU gets Azure consumption credit; frame as Microsoft investment
- Outsourcing: CSU alignment for adoption metrics; ATU for renewal positioning
- Hero Offer: use Microsoft-branded landing pages + partner network for Whitespace/Emerging digital entry

Phase 4: Funding and Incentives Access
- **MACC (Microsoft Azure Consumption Commitment):** identify if client has MACC — if so, Azure services in SI count toward their commitment
- **Solution Play Funding:** Microsoft invests in specific workloads annually — identify active Solution Plays; apply for co-investment in qualified deals
- **FastTrack for Azure:** Microsoft-funded technical deployment assistance for Azure migrations — reduces Avanade delivery cost on qualifying SI deals
- **ADS (Azure Deployment Support):** Microsoft funding for specific deployment scenarios — identify eligible workloads
- **Partner Marketing Funds:** co-marketing budget available for joint campaigns with ATU — applicable for Emerging/Whitespace demand generation

**Critical Rules:**
- NEVER approach Microsoft co-sell without ATU relationship established or in development — cold co-sell approaches fail
- NEVER miss MACC alignment check — it changes the commercial conversation entirely
- ALWAYS align offer to active Solution Plays — non-aligned offers get no Microsoft investment
- ALWAYS identify ATU fiscal quarter timing — end-of-quarter is highest co-sell leverage moment
- NEVER treat co-sell as just a funding mechanism — it is a trust-building channel with the Microsoft account team

---

### Skill 7: `consulting-account-expansion`

**Purpose:** Estratégia de expansão de conta para contas existentes Avanade. Diagnóstico de conta em 5 dimensões, identificação do próximo bet de expansão, roadmap de 12–24 meses, e risk register de conta.

**Frameworks integrados:** TAS (Target Account Selling — INVEST/DEVELOP/HARVEST/OPPORTUNISTIC quadrant), Strategic Selling (ongoing buyer committee mapping), GAP Selling (Estado Atual vs. Estado Futuro para expansion narrative), Value Selling (economic value of expansion vs. competition).

**Trigger phrases:**
- "Monta estratégia de expansão para o Bradesco — já somos fornecedores de SI"
- "Qual é o próximo bet de expansão para a Cielo? Já temos Outsourcing rodando"
- "Faz o diagnóstico de conta para o Safra — identifica gaps e riscos"
- "Cria o roadmap de expansão 12–24 meses para conta MVC no setor FSI"
- "Identifica riscos de perda na conta HapVida — contrato renova em 6 meses"

**Content specification:**

Phase 1: Account Snapshot (5 Dimensions)
- Current revenue and YoY trend (growing, stable, at risk?)
- Offer coverage: which of the 4 offer types do we have? What is missing?
- Relationship depth: how many C-level contacts do we have? Are they advocates, neutral, or risks?
- Microsoft alignment: how strong is our co-sell partnership on this account? ATU engaged?
- Competitive exposure: is a competitor actively pursuing this account? Any displacement risk?
- TAS quadrant assignment: INVEST / DEVELOP / HARVEST / OPPORTUNISTIC based on strategic value × relationship strength

Phase 2: Expansion Bet Identification
- Gap analysis: Estado Atual (current Avanade coverage) → Estado Futuro (potential expansion) → Impacto do Gap (revenue opportunity + risk of not expanding)
- Offer expansion plays:
  - Assessment → SI: client has roadmap, is ready to build
  - SI → Outsourcing: delivery delivered, operations candidate
  - Outsourcing → Shared Services: operational maturity achieved, scale opportunity
  - Any offer → New pillar: client in AI Biz Solutions, open door to Security or Cloud expansion
- Prioritize bets by: Strategic Value × Win Probability × Timing (is there a forcing function?)
- Identify 1–3 concrete expansion bets with evidence

Phase 3: 12–24 Month Expansion Roadmap
- Quarter-by-quarter expansion arc
- Each quarter: objective, offer target, key stakeholder to engage, milestone, success criteria
- Funding events to target: budget cycle, contract renewal, leadership change, regulatory mandate
- Microsoft co-sell integration: align expansion bets with ATU priorities and Solution Play calendar

Phase 4: Account Risk Register
- Risk categories: relationship risk (key sponsor left), competitive risk (displacement active), delivery risk (current contract in trouble), budget risk (client cutting IT spend), regulatory risk (compliance event impacting our delivery)
- For each risk: probability (H/M/L), impact (H/M/L), mitigation action, owner, timeline
- Green/Yellow/Red account health signal per dimension
- Renewal readiness: if contract renewing in <12 months, what is the renewal strategy?

**Critical Rules:**
- NEVER expand into new offer without delivery quality on current offer validated
- NEVER build expansion roadmap without identifying the Economic Buyer for each expansion bet
- ALWAYS include risk register — expansion plans without risk awareness lead to surprises
- ALWAYS align expansion bets with Microsoft ATU priorities — co-sell on expansion is even more powerful than on new logos
- NEVER treat HARVEST accounts like INVEST accounts — different coverage model, different investment level

---

## Task 3: Regenerate Indexes (After Each Batch)

After each batch of skills is created and validated:

**Step 1: Regenerate skills index**

```bash
python3 scripts/generate-skills-index.py
```
Expected: `skills_index.json` updated

**Step 2: Regenerate catalog**

```bash
python3 scripts/generate-catalog.py
```
Expected: `CATALOG.md` updated

**Step 3: Verify git status**

```bash
git status --short
```
Expected: only intentional changes

---

## Task 4: Update Documentation (Pre-Release)

**Files to update before next release (do NOT change version now):**
- `README.md` — add skills to GTM/Sales category table; bump badge from 55 to 57 (Batch 1) then 62 (Batch 2)
- `CLAUDE.md` — add skills to architecture tree and Skill Types section
- `CHANGELOG.md` — add entry for new skills
- `cli-installer/README.md` — update skill count
- `.claude-plugin/marketplace.json` — update description if skill count changed
- `bundles.json` — evaluate: `microsoft-consulting-icp` + `consulting-gtm-designer` in `product` bundle; `presales-qualifier` + `consulting-pursuit-governance` in `product` bundle

**Version bump (deferred to release):**
```bash
node scripts/release.js minor
```
Batch 1 (2 skills): 1.22.x → 1.23.0
Batch 2 (5 skills): 1.23.x → 1.24.0

---

## Validation Criteria (Both Batches)

Before marking implementation complete for any skill:

- [ ] `SKILL.md` frontmatter has ONLY `name`, `description`, `license`
- [ ] NO `created`, `updated`, `version`, `author` in any `SKILL.md`
- [ ] `README.md` has complete Metadata table with dates and version
- [ ] Passes `validate-skill-yaml.sh` and `validate-skill-content.sh`
- [ ] Word count: 1500–2000 (max 5000) per skill
- [ ] `skills_index.json` and `CATALOG.md` regenerated after each batch
- [ ] No changes to `bundles.json`, `README.md`, `CHANGELOG.md`, or version files (release-time only)
- [ ] Skills look native to repository — no foreign structure, no verbatim imported content
- [ ] Hero Offer positioned as optional (never as required gate) in all relevant skills
- [ ] MVC/HG/Emerging/Whitespace used as primary segmentation (Enterprise/SMC as secondary context)
- [ ] Framework references are Avanade-contextualized, not generic copies from 21 Frameworks doc

---

## Success Criteria

This plan succeeds when:
- All 7 skills are in `skills/` with valid structure and content
- MVC/HG/Emerging/Whitespace segmentation is consistent across all skills
- Hero Offer is correctly positioned as option across all relevant skills
- Each skill has distinct, non-overlapping scope from the others and from `abx-strategy` / `product-strategy`
- Eric can invoke any skill and receive structured output relevant to his VP/GTM context at Avanade
- Indexes regenerated and up to date
- Implementation can proceed without architectural ambiguity

---

## Skill Map: Non-Overlap Summary

| Skill | Core Job | Distinct From |
|-------|----------|---------------|
| `microsoft-consulting-icp` | Quem é meu cliente ideal por tier × oferta? | `abx-strategy` (generic B2B), `product-strategy` (product ICP) |
| `consulting-gtm-designer` | Como vendo minha oferta por tier com expansão? | `abx-strategy` (signal-driven, not services), `product-strategy` (PLG/SLG) |
| `presales-qualifier` | Vale a pena investir pre-sales neste deal? | `abx-strategy` PURE (generic), `mckinsey-strategist` (strategy, not sales) |
| `consulting-pursuit-governance` | Como governo um pursuit ativo? | `presales-qualifier` (qualification), `consulting-gtm-designer` (GTM design) |
| `executive-account-briefing` | Como me preparo para uma reunião C-level? | `storytelling-expert` (narrative only), `executive-resume-writer` (resume) |
| `microsoft-cosell-strategy` | Como maximizo a co-venda com Microsoft? | `consulting-gtm-designer` (GTM design), `abx-strategy` (no Microsoft context) |
| `consulting-account-expansion` | Como expando revenue em contas existentes? | `consulting-gtm-designer` (new logos/GTM design), `abx-strategy` (no services context) |
