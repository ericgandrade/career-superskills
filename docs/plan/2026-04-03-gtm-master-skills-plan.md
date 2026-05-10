# GTM Master Skills Plan

> **For Claude:** Use `executing-plans` to implement this plan task-by-task. Respect the Decision Gates.

**Objective:** Elevate the 3 existing GTM skills and create 19 new skills covering Product GTM + Microsoft Consulting GTM, with a uniform naming convention, positioning as a central pillar, and two workflow skills that orchestrate each full track.

**Current version:** v1.22.0 (55 skills)
**Target version:** v1.27.0 (74 skills)

---

## Skill Creation Approach

> **CRITICAL for execution:** Every new skill in this plan must be created using the `skill-creator` skill.
> Do not write SKILL.md manually. The skill-creator handles triggers, description optimization and evals.

**For each new skill, the executor must:**
1. Invoke `skill-creator` with the skill brief (purpose, trigger phrases, frameworks, output format)
2. Let the skill-creator conduct the interview, write the SKILL.md and create the evals
3. Run the test cases and iterate until approval
4. Run description optimization to maximize triggering accuracy
5. Validate with `./scripts/validate-skill-yaml.sh` and `./scripts/validate-skill-content.sh`

**Required evals per skill (minimum 3 test cases):**
- 1 main use case (canonical trigger phrase)
- 1 edge case (incomplete input, ambiguous context)
- 1 negative case (should-not-trigger — similar prompt that should NOT activate this skill)

---

## Approved Naming Convention

| Prefix | When to use | Example |
|--------|-------------|---------|
| `gtm-` | Skill shared by both tracks | `gtm-market-sizing` |
| `gtm-product-` | Exclusive to Product SaaS/PLG GTM | `gtm-product-launch` |
| `gtm-consulting-` | Exclusive to Microsoft Consulting GTM | `gtm-consulting-icp` |

## Complete Renaming Table

| Old name (previous plan) | New name (this plan) | Track | GTM P |
|--------------------------|----------------------|-------|-------|
| `market-sizing` | **`gtm-market-sizing`** | Shared | Place (segmentation) |
| `icp-designer` | **`gtm-icp-designer`** | Shared | Place (targeting) |
| *(new — critical gap)* | **`gtm-positioning`** | Shared | Promotion (message) |
| `startup-growth-strategist` *(existing — slim rebuild)* | **`gtm-product-growth`** | Product | Product (viability) |
| `gtm-launch-strategy` | **`gtm-product-launch`** | Product | Place + Promotion |
| `pricing-strategy` | **`gtm-product-pricing`** | Product | Price |
| `signal-based-outreach` | **`gtm-product-outreach`** | Product | Place (outbound channel) |
| `gtm-demand-generation` | **`gtm-product-demand-gen`** | Product | Place (inbound channel) |
| `gtm-account-expansion` | **`gtm-product-expansion`** | Product | Price + Product |
| *(new)* | **`gtm-product-workflow`** | Product | Orchestrator |
| `microsoft-consulting-icp` | **`gtm-consulting-icp`** | Consulting | Place (targeting) |
| *(new — critical gap)* | **`gtm-consulting-offer-design`** | Consulting | Product (offer) |
| *(new — critical gap)* | **`gtm-consulting-pricing`** | Consulting | Price (T&M/fixed/outcome) |
| `consulting-gtm-designer` | **`gtm-consulting-designer`** | Consulting | Place + Promotion |
| `microsoft-cosell-strategy` | **`gtm-consulting-cosell`** | Consulting | Place (partner channel) |
| `presales-qualifier` | **`gtm-consulting-qualifier`** | Consulting | Place (qualification) |
| `executive-account-briefing` | **`gtm-consulting-briefing`** | Consulting | Promotion (engagement) |
| `consulting-pursuit-governance` | **`gtm-consulting-pursuit`** | Consulting | Process (execution) |
| `consulting-account-expansion` | **`gtm-consulting-expansion`** | Consulting | Price + Product |
| *(new)* | **`gtm-consulting-workflow`** | Consulting | Orchestrator |

**Existing skills keeping their current name** (enriched, not renamed):
- `product-strategy` — broader scope than GTM (positioning + strategy)
- `abx-strategy` — generic B2B scope (signal scoring, pipeline)

**Reference sources used in this plan:**
- `GTM-Strategist/gtm-strategist-skills` — Maja Voje methodology, 12 phases, tested with 1000+ companies. Skills in `.claude/skills/` on `master` branch.
- `iamachilles/gtm-skills-os` — 151 skills, 28,500+ lines, operationally the deepest. Skills in `skills/<name>/SKILL.md` (flat).
- `kenny589/gtm-flywheel` — 15 skills from a $7M ARR agency. Skills in root directories.

---

## The Two GTM Workflows

> Invoke `gtm-consulting-workflow` or `gtm-product-workflow` to execute the full workflow.
> Invoke individual skills for specific steps.

---

### Workflow A — Microsoft Consulting GTM
**Invoke:** `/gtm-consulting-workflow`

```
+------------------------------------------------------------------+
|  QUARTERLY CADENCE — "Place, Product, Price and Message"         |
+------------------------------------------------------------------+
|                                                                  |
|  PLACE ─────────────────────────────────────────────────────     |
|  gtm-market-sizing         → TAM/SAM/SOM of target segment       |
|  gtm-consulting-icp        → Who to pursue? (tier x offer)       |
|                                                                  |
|  PRODUCT ───────────────────────────────────────────────────     |
|  gtm-consulting-offer-design → WHAT to sell (scope,              |
|                                 deliverables, methodology,       |
|                                 Hero Offers)                     |
|                                                                  |
|  PRICE ─────────────────────────────────────────────────────     |
|  gtm-consulting-pricing    → T&M vs fixed-fee vs outcome-based   |
|                               rate justification, pricing psychology|
|                                                                  |
|  PROMOTION ──────────────────────────────────────────────────    |
|  gtm-positioning           → Positioning + Messaging House       |
|                               + battle cards (SHARED)            |
|  gtm-consulting-designer   → GTM Canvas + offer motion           |
|  gtm-consulting-cosell     → Microsoft channel + funding         |
|                                                                  |
+------------------------------------------------------------------+
|  PER OPPORTUNITY — "Is this deal worth governing?"               |
+------------------------------------------------------------------+
|                                                                  |
|  gtm-icp-designer          → Fit score for this account (5 dim.) |
|         ↓                                                        |
|  gtm-consulting-qualifier  → Worth investing pre-sales?          |
|         ↓                                                        |
|  gtm-consulting-briefing   → Prepare C-level meeting             |
|         ↓                                                        |
|  gtm-consulting-pursuit    → Govern active pursuit               |
|                                                                  |
+------------------------------------------------------------------+
|  QUARTERLY — "How to grow in existing accounts?"                 |
+------------------------------------------------------------------+
|                                                                  |
|  gtm-consulting-expansion  → Revenue expansion in account        |
|                                                                  |
+------------------------------------------------------------------+

Support (any stage):
  abx-strategy [ENRICHED]  → Signal scoring, PURE, multi-thread
  mckinsey-strategist       → Complex problem structure
```

---

### Workflow B — Product SaaS/PLG GTM
**Invoke:** `/gtm-product-workflow`

```
+------------------------------------------------------------------+
|  FOUNDATION — "Place, Product, Price, Promotion — get it right"  |
|  Cadence: Annual / when launching new product                    |
+------------------------------------------------------------------+
|                                                                  |
|  PLACE (who and where) ─────────────────────────────────────     |
|  gtm-market-sizing    → TAM/SAM/SOM with sources and sensitivity |
|  gtm-icp-designer     → ICP 5D, Anti-ICP, triggers, committee   |
|                                                                  |
|  PRODUCT (viability) ──────────────────────────────────────      |
|  gtm-product-growth   → Unit economics, Bet Board, roadmap       |
|  product-strategy [ENR.] → Beachhead, GTM motion, moat          |
|                                                                  |
|  PROMOTION (message) ──────────────────────────────────────      |
|  gtm-positioning      → April Dunford, Messaging House,          |
|                          battle cards, message-market fit (SHARED)|
|                                                                  |
+------------------------------------------------------------------+
|  LAUNCH — "How to take to market?"                               |
|  Cadence: Per product or major feature (quarterly)               |
+------------------------------------------------------------------+
|                                                                  |
|  PRICE ──────────────────────────────────────────────────────    |
|  gtm-product-pricing  → Value metric, WTP Van Westendorp,        |
|                          Gabor-Granger, 3 tiers, feature gates   |
|                                                                  |
|  PLACE + PROMOTION ──────────────────────────────────────────    |
|  gtm-product-launch   → 9 phases OPE Canvas → validation →       |
|                          assets → channels → go-live             |
|                                                                  |
+------------------------------------------------------------------+
|  GROWTH — "How to generate and convert demand?"                  |
|  Cadence: Per channel (monthly)                                  |
+------------------------------------------------------------------+
|                                                                  |
|  gtm-product-outreach  → Signal scoring, SPARK, 6-layer diag.   |
|  gtm-product-demand-gen → Paid, organic, growth loops,           |
|                            trial-to-paid, funnel optimization    |
|                                                                  |
+------------------------------------------------------------------+
|  EXPANSION — "How to grow in existing accounts?"                 |
|  Cadence: Quarterly                                              |
+------------------------------------------------------------------+
|                                                                  |
|  gtm-product-expansion → NRR, health scoring, upsell triggers,  |
|                           churn prevention                       |
|                                                                  |
+------------------------------------------------------------------+

Support (any stage):
  abx-strategy [ENR.]   → Enterprise deals >$10K ACV, long cycle
  product-discovery      → Continuous problem/solution validation
  product-delivery       → Staged rollout + launch metrics
  product-leadership     → Portfolio-level GTM + board
```

---

### Shared Skills (prefix `gtm-`)

```
  gtm-market-sizing   →  Used in BOTH workflows
  gtm-icp-designer    →  Used in BOTH workflows
  abx-strategy        →  Enterprise deals in any context
```

---

## Decision Gates

1. ✅ Plan approved (with decomposition into market-sizing + icp-designer + startup-growth-strategist slim)
2. ⏳ User approves Track A after Batch 1-3 (3 fixed skills + 2 new: market-sizing, icp-designer)
3. ⏳ User approves Track B Batch 1 (gtm-launch-strategy + pricing-strategy) before continuing
4. ⏳ User approves Track B Batch 2 (signal-based-outreach + demand + expansion) before continuing
5. ⏳ User approves Track C Batch 1 (microsoft-consulting-icp + consulting-gtm-designer)
6. ⏳ User approves Track C Batch 2 (5 remaining skills)
7. ⏳ Final version bump (v1.25.0, 69 skills)

---

## Frontmatter Rules (CRITICAL — do not get wrong)

```yaml
---
name: kebab-case-name
description: This skill should be used when... (single line, no unnecessary quotes)
license: MIT
---
```

**PROHIBITED in SKILL.md:** `version`, `author`, `platforms`, `category`, `tags`, `risk`, `created`, `updated`
**These fields belong in README.md** in the `## Metadata` table.

---

## TRACK A — Fix Existing Skills + Create New Foundation Skills

### Batch 1: Decompose `startup-growth-strategist` into 3 focused skills

**Original diagnosis:** Stub disguised as skill. Phase 5 empty. No ICP. No templates. Written as role description ("You are..."). Covered 5 phases that belong to 3 distinct skills.

**Design decision:**
- `market-sizing` [NEW] — TAM/SAM/SOM reusable by both workflows
- `icp-designer` [NEW] — ICP scoring matrix, buying triggers, intent signals, Anti-ICP, buying committee — the most reusable skill in the entire GTM suite
- `startup-growth-strategist` [SLIM] — refocused on unit economics + Bet Board + growth roadmap

**What was REMOVED from startup-growth-strategist and moved:**
- Phase 1 (TAM/SAM/SOM) → `market-sizing`
- Phase 2 (ICP Definition) → `icp-designer`
- Phase 4 GTM motion selection → already exists in `product-strategy` (enriched in Batch 2)

---

#### Task A1.1 — Create `skills/market-sizing/SKILL.md`

**File:** `skills/market-sizing/SKILL.md`
**Action:** Create new skill.

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

#### Task A1.2 — Create `skills/market-sizing/README.md`

**File:** `skills/market-sizing/README.md`

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

#### Task A1.3 — Create `skills/icp-designer/SKILL.md`

**File:** `skills/icp-designer/SKILL.md`
**Action:** Create new skill.

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

#### Task A1.4 — Create `skills/icp-designer/README.md`

**File:** `skills/icp-designer/README.md`

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

#### Task A1.5 — Rewrite `skills/startup-growth-strategist/SKILL.md` (SLIM version)

**File:** `skills/startup-growth-strategist/SKILL.md`
**Action:** REPLACE completely with the content below. Focused exclusively on unit economics + financial modeling + Bet Board + growth roadmap.

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

#### Task A1.6 — Update `skills/startup-growth-strategist/README.md`

Update Metadata table:
- `Version` → `2.0.0`
- `Updated` → `2026-04-03`
- Update Overview to reflect slim scope (unit economics + Bet Board)

---

#### Task A1.7 — Validate the 3 new/revised skills

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

### Batch 2: Enrich `product-strategy`

The skill is good, but 7 specific additions are needed. All are surgical — do not rewrite the entire skill.

#### Task A2.1 — Add Beachhead Segment to the "Where to Play" section

**File:** `skills/product-strategy/SKILL.md`
**Location:** After the "**Where to Play:**" block (around line ~155), before "**How to Win:**"
**Action:** Insert the block below after the Where to Play list:

```markdown
**Beachhead Segment (required for 0→1):**
Before defining the full ICP, identify the single narrowest segment where you can win first:
- Where do you have 3+ reference customers or strong evidence?
- Where is pain highest and buyer access easiest?
- Where can you dominate the conversation before expanding?

> **Rule:** Name one company type so specific you could call 20 of them tomorrow. If you can't, your segment is still too broad. Win here before expanding.
```

---

#### Task A2.2 — Add Buying Triggers and Intent Signals to ICP

**File:** `skills/product-strategy/SKILL.md`
**Location:** After the "**Anti-ICP (equally important):**" block
**Action:** Insert after the Anti-ICP block:

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

#### Task A2.3 — Add WTP Research Protocol to Pricing section

**File:** `skills/product-strategy/SKILL.md`
**Location:** After the "**0→1 Mode**: Run willingness-to-pay conversations early. Start simple: one price, test the market." line
**Action:** Insert after that line:

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

#### Task A2.4 — Add 3-part positioning structure

**File:** `skills/product-strategy/SKILL.md`
**Location:** After the "**Positioning Statement Format:**" block
**Action:** Insert after the Positioning Statement template:

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

#### Task A2.5 — Add Channel Effort/Impact Matrix to GTM Motion

**File:** `skills/product-strategy/SKILL.md`
**Location:** After the PLG vs SLG table
**Action:** Insert after that table:

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

#### Task A2.6 — Add "do nothing" as explicit competitor

**File:** `skills/product-strategy/SKILL.md`
**Location:** In the "2. Market & Competitive Reality" section, after the analysis tables
**Action:** After "Key questions:", insert:

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

#### Task A2.7 — Add Consistency Checklist

**File:** `skills/product-strategy/SKILL.md`
**Location:** In the "Quick Reference: Strategy Quality Checklist" section, after the 9 existing items
**Action:** Add 3 items to the existing checklist:

```markdown
- [ ] **Beachhead identified** — You can name 20 companies to call tomorrow in the narrowest segment
- [ ] **WTP validated** — You've run Van Westendorp with at least 8 customers before setting price
- [ ] **Positioning consistent** — Market Change, ICP, and Positioning Statement tell the same story without contradiction
```

---

### Batch 3: Enrich `abx-strategy`

The skill is strong — 3 high-precision surgical additions.

#### Task A3.1 — Add Step 0: GTM Diagnostic as entry point

**File:** `skills/abx-strategy/SKILL.md`
**Location:** Right after the Progress Tracking block, before "## Core Philosophy"
**Action:** Insert new block:

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

#### Task A3.2 — Add Composite Lead Scoring Formula to Framework 3

**File:** `skills/abx-strategy/SKILL.md`
**Location:** After the Tier Assignment table in "Framework 3: PURE Problem Scoring"
**Action:** Insert after the tiers table:

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

#### Task A3.3 — Add Signal Scoring Multipliers to Signal Detection

**File:** `skills/abx-strategy/SKILL.md`
**Location:** After the "Signal Detection Categories" table
**Action:** Insert after that table:

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

## TRACK B — New Product GTM Skills

### Batch 4: `gtm-positioning` (new skill — Shared, critical)

**Purpose:** The missing 4th P in both workflows. Positioning is the multiplier — with it right, ICP converts faster, channel works better, price justifies itself.

**Brief for `skill-creator`:**

```
Name: gtm-positioning
Track: Shared (product and consulting)

Purpose: Define market positioning and message using April Dunford 6-step,
create a Messaging House with variations per persona, develop battle cards per
competitor, and measure Message-Market Fit.

Trigger phrases:
- "I need to position my product/offer"
- "How do we differentiate from the competition?"
- "Help me create the messaging for [product/offer]"
- "Our message isn't working — what to change?"
- "Create battle cards for [competitor]"
- "How do we know if the positioning is right?"

Should NOT trigger:
- "I need to define pricing" → gtm-product-pricing
- "What is my ICP?" → gtm-icp-designer
- "Validate if the market exists" → gtm-market-sizing

Required frameworks:
1. April Dunford 6-step positioning:
   Competitive alternatives → Unique attributes → Value → Target customers
   → Market category → Relevant trends
2. GTM Triangle positioning (Market Change → Thesis → Proof)
3. Messaging House: Primary message (10 words) + 3 pillars + per-persona variations
4. Battle card per competitor: why they choose them / why they leave / your wedge / objection handlers
5. Message-Market Fit measurement: % of calls where prospect uses your language back

Output format:
- Positioning Statement (April Dunford format)
- GTM Triangle (3 lines)
- Messaging House (table)
- Battle Cards (one per competitor)
- Message-Market Fit KPI target

Evals (3 test cases):
1. "Position our AI contract review tool — competitors are DocuSign, Ironclad, and doing it manually"
2. "Our message is 'enterprise analytics platform' and it's not converting — where is it wrong?"
3. "Create battle card for our Outsourcing proposal vs. client's internal team" (consulting context)
```

**Validation:**
```bash
./scripts/validate-skill-yaml.sh skills/gtm-positioning
./scripts/validate-skill-content.sh skills/gtm-positioning
```

---

### Batch 5: `gtm-product-launch` (new skill)

**Purpose:** Most important skill in Track B. Guides the complete journey from GTM canvas to go-live, in 9 sequential phases. Based on Maja Voje (GTM-Strategist) + gtm-skills-os methodology. Each phase produces an output that feeds the next.

#### Task B1.1 — Create `skills/gtm-launch-strategy/SKILL.md`

**File:** `skills/gtm-launch-strategy/SKILL.md`
**Action:** Create with the content below.

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

#### Task B1.2 — Create `skills/gtm-launch-strategy/README.md`

**File:** `skills/gtm-launch-strategy/README.md`
**Action:** Create with the content below.

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

### Batch 5: `pricing-strategy` (new skill)

**Purpose:** WTP research, value metric selection, tier design, competitive anchoring. Based on gtm-skills-os pricing-strategy — the most rigorous of the 3 repos on this topic.

#### Task B2.1 — Create `skills/pricing-strategy/SKILL.md`

**File:** `skills/pricing-strategy/SKILL.md`
**Action:** Create with the content below.

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

**WTP by segment:** Run the exercise separately for different ICP tiers if ACV differs significantly. Enterprise WTP is not equal to SMB WTP.

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
| [Core feature] | Yes | Yes | Yes | Must be in free/entry |
| [Collaboration] | No | Yes | Yes | Drives team expansion |
| [Advanced analytics] | No | Yes | Yes | Power user value |
| [SSO / Security] | No | No | Yes | Enterprise requirement |
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

#### Task B2.2 — Create `skills/pricing-strategy/README.md`

**File:** `skills/pricing-strategy/README.md`
**Action:** Create with the content below.

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

### Batch 6: `signal-based-outreach` (new skill)

**Purpose:** Complete outbound flywheel — ICP matrix, intent signals with scoring formula, cold email SPARK, analytics, learning loop. Based on gtm-flywheel (ColdIQ) + sachacoldiq.

#### Task B3.1 — Create `skills/signal-based-outreach/SKILL.md`

**File:** `skills/signal-based-outreach/SKILL.md`
**Action:** Create with the content below.

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

#### Task B3.2 — Create `skills/signal-based-outreach/README.md`

**File:** `skills/signal-based-outreach/README.md`
**Action:** Create with the content below.

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

### Batch 7: `gtm-demand-generation` and `gtm-account-expansion`

**Note:** These two skills are Batch 7 and require user approval after Batch 6.

#### Task B4.1 — Create `skills/gtm-demand-generation/SKILL.md`

**Purpose:** Channel architecture, growth loops, trial-to-paid conversion, funnel optimization. Covers the most critical gap identified — no current skill covers demand gen.

**File:** `skills/gtm-demand-generation/SKILL.md`
**Action:** Create with frontmatter and complete body covering:

**Frontmatter:**
```yaml
---
name: gtm-demand-generation
description: This skill should be used when the user needs to design or optimize a demand generation system for a SaaS or product company. Use when building paid, organic, or viral acquisition channels, designing growth loops, optimizing trial-to-paid conversion, building the marketing funnel, or diagnosing why top-of-funnel is not converting.
license: MIT
---
```

**Required body sections:**
1. **Phase 1: Channel Architecture** — Channel selection matrix (effort/impact), paid (SEM, social, display) setup foundations, organic (SEO, content, community) foundations, viral/referral loop design. Rule: master 2 channels before adding a 3rd.
2. **Phase 2: Growth Loop Design** — Identify existing loops (viral, content, ecosystem, sales). Document each loop as: Trigger → Action → Reward → Invitation. Measure loop coefficient (K-factor for viral).
3. **Phase 3: Trial-to-Paid Conversion** — Define "Activation Moment" (the specific action that predicts conversion). Activation funnel (signed-up → activated → retained → paid). Identify friction points. Design onboarding that leads to Activation Moment. Target: >20% trial-to-paid in 30 days.
4. **Phase 4: Funnel Optimization** — TOFU/MOFU/BOFU metrics. Identify the leakiest part of the funnel first. Conversion benchmarks by channel. A/B test cadence (one variable at a time).
5. **Phase 5: Measurement & Attribution** — UTM structure, attribution model (first-touch vs last-touch vs multi-touch). CAC per channel. ROAS per channel. North Star Metric definition.
6. **Critical Rules + Example Usage** (5 examples)

---

#### Task B4.2 — Create `skills/gtm-demand-generation/README.md`

Create README with standard Metadata table (Version 1.0.0, Author Eric Andrade, Created 2026-04-03).

---

#### Task B4.3 — Create `skills/gtm-account-expansion/SKILL.md`

**Purpose:** NRR strategy, upsell triggers, churn prevention, customer health scoring. Covers the expansion/retention gap.

**File:** `skills/gtm-account-expansion/SKILL.md`
**Action:** Create with frontmatter and complete body covering:

**Frontmatter:**
```yaml
---
name: gtm-account-expansion
description: This skill should be used when the user needs to design or improve a customer expansion strategy. Use when building upsell and cross-sell playbooks, designing customer health scoring, creating churn prevention programs, improving Net Revenue Retention, or planning customer success motions that drive expansion revenue.
license: MIT
---
```

**Required body sections:**
1. **Phase 1: Customer Health Scoring** — Define health score with 4-6 dimensions (product usage, support tickets, NPS/CSAT, payment health, stakeholder engagement, expansion indicators). Score 0-100. Traffic light (Green/Yellow/Red). Automate weekly monitoring.
2. **Phase 2: Expansion Triggers & Upsell Playbook** — Identify 5+ triggers for upsell (usage hitting tier limit, new use case adoption, team growth, funding event, success milestone). Per trigger: who contacts, message, offer, timing. Rule: upsell after value demonstrated, never before.
3. **Phase 3: Churn Prevention** — Early warning signals (login frequency drop, feature abandonment, support spike, stakeholder change). Churn risk scoring. Intervention playbook by risk level. Win-back sequence for churned accounts.
4. **Phase 4: NRR Strategy** — NRR formula (Expansion MRR + Contraction MRR + Churn MRR) ÷ Starting MRR. Target: >110% for strong SaaS. Diagnose the gap between current and target NRR. Identify which customer segment drives most expansion.
5. **Phase 5: Customer Success Cadence** — Check-in frequency by tier (High-touch: monthly, Mid-touch: quarterly, Low-touch: automated). QBR structure. Success plan template. Executive sponsor engagement.
6. **Critical Rules + Example Usage**

---

#### Task B4.4 — Create `skills/gtm-account-expansion/README.md`

Create README with standard Metadata table.

---

## TRACK C — Microsoft Consulting GTM Skills

> **Reference:** The complete content for each skill is defined in `docs/plan/2026-04-03-gtm-consulting-skills-plan-v2.md`. This plan supersedes the v2 — execute from the content here and ignore the v2 for the batch sequence. The skill body content (SKILL.md body) is complete in the v2.

### Batch 8: Consulting GTM — Offer and Pricing (new)

**Skills:** `gtm-consulting-offer-design` + `gtm-consulting-pricing`

**Use `skill-creator` for both.**

#### Brief: `gtm-consulting-offer-design`

```
Name: gtm-consulting-offer-design
Track: Consulting

Purpose: Design the consulting offer — define scope, deliverables,
methodology, team structure, Hero Offers as entry point, and
structured commercial proposal. The "Product" of consulting GTM.

Trigger phrases:
- "I need to structure my [Assessment/SI/Outsourcing] offer"
- "How do I define the scope of a Hero Offer?"
- "Help me design the deliverable for [offer]"
- "What is the difference between our offer and the competitor's?"
- "I need to create a proposal for [client]"

Should NOT trigger:
- "How much should I charge?" → gtm-consulting-pricing
- "Who should I pursue?" → gtm-consulting-icp
- "How do I GTM this offer?" → gtm-consulting-designer

Required frameworks:
1. Offer Design Canvas: Problem → Approach → Deliverables → Team → Timeline → Success metrics
2. Hero Offer structure: Entry point (fixed-fee $30-75K, 2-6 weeks) → Natural upsell path
3. Methodological differentiation: what Avanade delivers differently from competitors (IP, accelerators, Microsoft depth)
4. Offer vs. competitor: feature/benefit/value table per offer type
5. Commercial proposal: Executive Summary → Problem → Approach → Deliverables → Investment → Why Avanade

Offer types in scope:
- Assessment ($15K-$80K): discovery, current state, roadmap
- System Integration ($150K-$2M+): design, build, test, deploy
- Outsourcing (multi-year): operations, SLA, governance
- Shared Services: process standardization, scale

Output format:
- Completed Offer Design Canvas
- Hero Offer brief (1 page)
- Differentiation table vs. 2-3 competitors
- Proposal outline (sections + key messages)

Evals:
1. "Structure a Hero Offer for AI Assessment for a Whitespace account in the Manufacturing sector"
2. "Design the deliverables for a $500K Azure SI engagement in the financial sector"
3. "How do we differentiate our Outsourcing from a generic offshore service center?"
```

#### Brief: `gtm-consulting-pricing`

```
Name: gtm-consulting-pricing
Track: Consulting

Purpose: Define the commercial model and price for consulting
engagements — choose between T&M, fixed-fee, outcome-based or
retainer, justify rate premium, structure pricing by offer type
and account tier.

Trigger phrases:
- "How much should I charge for this engagement?"
- "Fixed-fee or T&M for this project?"
- "How do we justify our rate versus the cheaper competitor?"
- "I need to price an Outsourcing proposal"
- "The client wants outcome-based — is it worth it?"

Should NOT trigger:
- "I need to price my SaaS" → gtm-product-pricing
- "Structure the consulting offer" → gtm-consulting-offer-design
- "How do I qualify this deal?" → gtm-consulting-qualifier

Required frameworks:
1. Commercial model by offer type:
   - Assessment: fixed-fee (predictability for client, low risk for Avanade)
   - SI: T&M with cap (control + flexibility) or fixed-fee if scope is mature
   - Outsourcing: SLA-based with baseline + optional gain-share
   - Retainer: recurring with volume commitment
2. Rate justification framework: value delivered vs. cost of alternatives vs. risk of doing nothing
3. Pricing psychology: high anchoring, decoy tier, framing as investment not cost
4. Discount governance: when to give, how much, who approves, expected counterparts
5. Commercial model tradeoffs matrix: T&M vs Fixed vs Outcome vs Retainer by context

Output format:
- Commercial model recommendation with rationale
- Rate range per role/seniority
- Pricing narrative (how to present to CFO)
- Discount governance table

Evals:
1. "Better T&M or fixed-fee for an $800K healthcare SI — client is risk-conservative"
2. "Justify our $220/hour rate for an Azure Architect when offshore costs $80"
3. "The client wants outcome-based for Outsourcing — how do we structure this without assuming excessive risk?"
```

**Gate:** User reviews the 2 skills before proceeding to Batch 9.

---

### Batch 9: Consulting GTM — ICP and Designer

**Skills:** `gtm-consulting-icp` + `gtm-consulting-designer`

**Use `skill-creator` for both. Base brief available in `docs/plan/2026-04-03-gtm-consulting-skills-plan-v2.md`** — use the v2 content as input for the skill-creator (do not copy directly, let the skill-creator structure with triggers and evals).

**Gate:** User reviews the 2 skills before proceeding to Batch 10.

---

### Batch 9: Consulting GTM Batch 2

**Skills:** `presales-qualifier`, `executive-account-briefing`, `consulting-pursuit-governance`, `microsoft-cosell-strategy`, `consulting-account-expansion`

**Execution instruction:** Continue reading the v2 for the content of each skill. If any skill does not have complete content in the v2, create based on the patterns established in this plan.

---

## TRACK D — Release

### Batch 10: Version Bump and Documentation Sync

**Prerequisite:** All Tracks A, B, C complete and approved by user.

#### Task D1 — Count skills and define new version

Current: 55 skills, v1.22.0

Added:
- Shared: `gtm-market-sizing` + `gtm-icp-designer` = 2
- Product: `gtm-product-launch` + `gtm-product-pricing` + `gtm-product-outreach` + `gtm-product-demand-gen` + `gtm-product-expansion` + `gtm-product-workflow` = 6
- Consulting: `gtm-consulting-icp` + `gtm-consulting-designer` + `gtm-consulting-cosell` + `gtm-consulting-qualifier` + `gtm-consulting-briefing` + `gtm-consulting-pursuit` + `gtm-consulting-expansion` + `gtm-consulting-workflow` = 8
- **Total new: 19 skills**

Renamed (count as new, old dir removed): `startup-growth-strategist` → `gtm-product-growth`
Enriched (don't count): `product-strategy`, `abx-strategy`

Total: **74 skills**, target version: **v1.27.0**

#### Task D2 — Run release script

```bash
node scripts/release.js minor
# Will be run 2 times (minor from 1.22 → 1.23 → 1.24)
# Or: do a single bump to 1.24.0 by updating package.json manually
```

Verify after:
```bash
./scripts/verify-version-sync.sh
```

#### Task D3 — Update CLAUDE.md

In the `## Project Overview` section: update `v1.22.0` → `v1.24.0` and `55 skills` → `67 skills`.

In the `## Repository Architecture` section (skills tree): add the 12 new skills in alphabetical order.

In the `## Skill Types` section: add new skills to the correct categories:
- **GTM & Product**: `gtm-launch-strategy`, `pricing-strategy`, `signal-based-outreach`, `gtm-demand-generation`, `gtm-account-expansion`
- **GTM & Consulting Microsoft**: `microsoft-consulting-icp`, `consulting-gtm-designer`, `microsoft-cosell-strategy`, `presales-qualifier`, `executive-account-briefing`, `consulting-pursuit-governance`, `consulting-account-expansion`

#### Task D4 — Update README.md

- H1 title: `# Claude Superskills v1.24.0`
- Badge: `![Version](https://img.shields.io/badge/version-1.24.0-blue.svg)`
- Badge: `![Skills](https://img.shields.io/badge/skills-67-green.svg)`
- Footer: `*Version 1.24.0 | April 2026*`
- Add new skills to relevant category tables

#### Task D5 — CHANGELOG.md

Add entry:
```markdown
## [1.26.0] - 2026-04-03

### Added (16 new skills with gtm- / gtm-product- / gtm-consulting- convention)

**Shared (both tracks):**
- `gtm-market-sizing` — TAM/SAM/SOM 8-step workflow with sources, SAM filters, sensitivity
- `gtm-icp-designer` — ICP scoring matrix 5 dimensions, buying triggers, binary Anti-ICP, buying committee

**Product GTM (gtm-product-):**
- `gtm-product-growth` — Unit economics + Bet Board + Assumption Ranking + Growth Roadmap *(rebuild of startup-growth-strategist)*
- `gtm-product-launch` — 9 phases OPE Canvas → validation → pricing → positioning → assets → go-live (Maja Voje)
- `gtm-product-pricing` — Value metric, WTP Van Westendorp + Gabor-Granger, 3 tiers (hard cap), feature gates
- `gtm-product-outreach` — ICP matrix 5D, signal scoring formula, SPARK cold email, 6-layer diagnostic
- `gtm-product-demand-gen` — Channel architecture, growth loops, trial-to-paid conversion, funnel optimization
- `gtm-product-expansion` — NRR strategy, customer health scoring, upsell triggers, churn prevention
- `gtm-product-workflow` — Orchestrator for the complete Product GTM workflow

**Consulting GTM (gtm-consulting-):**
- `gtm-consulting-icp` — ICP by tier × offer (MVC/HG/Emerging/Whitespace), PURE scoring, Anti-ICP
- `gtm-consulting-designer` — Complete GTM Canvas for Microsoft consulting offerings
- `gtm-consulting-cosell` — Microsoft co-sell + funding, DPOR, co-sell status, MCI/AMPP
- `gtm-consulting-qualifier` — Deal qualification before investing pre-sales
- `gtm-consulting-briefing` — C-level meeting preparation with account context
- `gtm-consulting-pursuit` — Active pursuit management with gates and risk scoring
- `gtm-consulting-expansion` — Revenue expansion in existing accounts
- `gtm-consulting-workflow` — Orchestrator for the complete Consulting GTM workflow

### Improved
- `product-strategy` — Beachhead Segment, WTP Van Westendorp protocol, 3-part positioning (Market Change→Thesis→Proof), Buying Triggers + Intent Signals in ICP, channel effort/impact matrix, "do nothing" competitor, consistency checklist
- `abx-strategy` — Step 0 GTM Diagnostic (7 dimensions), composite lead scoring `(Fit×0.30)+(Intent×0.45)+(Engagement×0.25)`, signal decay multipliers with SLAs, staleness rule 4 months
```

#### Task D6 — Update GitHub About

```bash
gh repo edit --description "67 Universal AI Skills for Claude Code, GitHub Copilot & 6 more platforms. GTM launch strategy, pricing, outbound, product strategy, consulting GTM, Obsidian, document conversion — no API keys required."
```

#### Task D7 — Regenerate indexes

```bash
npm run generate-all --prefix cli-installer
```

#### Task D8 — Create plugin package

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

## Validation Commands (run after each batch)

```bash
# Validate a specific skill
./scripts/validate-skill-yaml.sh skills/<skill-name>
./scripts/validate-skill-content.sh skills/<skill-name>

# Validate all skills at once
for skill in skills/*/; do
  ./scripts/validate-skill-yaml.sh "$skill"
  ./scripts/validate-skill-content.sh "$skill"
done

# Verify version synchronization
./scripts/verify-version-sync.sh

# Count skills
ls -d skills/*/ | wc -l
```

---

## Status Summary

| Batch | Track | Skills (final names) | Creation | New | Status |
|-------|-------|----------------------|----------|-----|--------|
| **1** | Shared | `gtm-market-sizing`, `gtm-icp-designer` | skill-creator | +2 | Pending |
| **1** | Product | `gtm-product-growth` *(rebuild of startup-growth-strategist)* | skill-creator | rename | Pending |
| **2** | Enrich | `product-strategy` (7 surgical additions) | manual edit | 0 | Pending |
| **3** | Enrich | `abx-strategy` (Step 0 + scoring formula + decay) | manual edit | 0 | Pending |
| **4** | Shared | `gtm-positioning` — **missing 4th P** | skill-creator | +1 | Pending |
| **5** | Product | `gtm-product-launch`, `gtm-product-pricing` | skill-creator | +2 | Pending |
| **6** | Product | `gtm-product-outreach` | skill-creator | +1 | Pending |
| **7** | Product | `gtm-product-demand-gen`, `gtm-product-expansion`, `gtm-product-workflow` | skill-creator | +3 | Pending |
| **8** | Consulting | `gtm-consulting-offer-design` — **missing Product**, `gtm-consulting-pricing` — **missing Price** | skill-creator | +2 | Pending |
| **9** | Consulting | `gtm-consulting-icp`, `gtm-consulting-designer` | skill-creator | +2 | Pending |
| **10** | Consulting | `gtm-consulting-qualifier`, `gtm-consulting-briefing`, `gtm-consulting-pursuit`, `gtm-consulting-cosell`, `gtm-consulting-expansion`, `gtm-consulting-workflow` | skill-creator | +6 | Pending |
| **11** | Release | v1.27.0, 74 skills, docs sync, GitHub push | scripts | — | Pending |
| | | **Total new** | | **+19** | |

**4P coverage after execution:**

| P | Product GTM | Consulting GTM |
|---|-------------|----------------|
| **Place** | gtm-market-sizing + gtm-icp-designer + gtm-product-outreach + gtm-product-demand-gen | gtm-market-sizing + gtm-consulting-icp + gtm-consulting-cosell |
| **Product** | product-strategy + gtm-product-growth + gtm-product-launch | gtm-consulting-offer-design |
| **Price** | gtm-product-pricing | gtm-consulting-pricing |
| **Promotion** | gtm-positioning + gtm-product-launch | gtm-positioning + gtm-consulting-designer |
