# 💼 Career Superskills v1.0.1

20 AI skills for job search, resume optimization, career transitions, and professional development. Install once across all 8 AI platforms.

![Version](https://img.shields.io/badge/version-1.0.2-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Skills](https://img.shields.io/badge/skills-20-brightgreen.svg)
![Platforms](https://img.shields.io/badge/platforms-8-orange.svg)

## 🚀 Quick Install

**One-liner (recommended):**
```bash
curl -fsSL https://raw.githubusercontent.com/ericgandrade/career-superskills/main/scripts/install.sh | bash
```

**Or use NPX (zero-install):**
```bash
npx career-superskills
```

The installer detects and installs to all 8 AI platforms on your machine: Claude Code, GitHub Copilot, OpenAI Codex, OpenCode, Gemini CLI, Antigravity, Cursor IDE, AdaL CLI.

**Other methods:**
```bash
# npm global
npm install -g career-superskills

# With bundles
npx career-superskills --bundle resume -y
npx career-superskills --bundle job-search -y
```

**Local install (no npm/npx required):**
```bash
git clone https://github.com/ericgandrade/career-superskills
cd career-superskills

./scripts/local-install.sh        # Interactive
./scripts/local-install.sh -y     # Auto-install to all detected platforms
./scripts/local-install.sh -y -q  # Silent (CI / scripted)
```

**Uninstall:**
```bash
curl -fsSL https://raw.githubusercontent.com/ericgandrade/career-superskills/main/scripts/uninstall.sh | bash
```

## 🔌 Claude Code Plugin (Native)

Requires **Claude Code v1.0.33+** (`claude --version` to check).

**Method 1: Interactive UI (Inside a running `claude` session) — Recommended**

```text
/plugin marketplace add ericgandrade/career-superskills
/plugin install career-superskills@career-superskills
```

**Method 2: Local test (no install needed)**

```bash
git clone https://github.com/ericgandrade/career-superskills
claude --plugin-dir ./career-superskills
```

### Once installed — all 20 skills under the `career-superskills:` namespace

```
/career-superskills:resume-tailor
/career-superskills:resume-ats-optimizer
/career-superskills:resume-bullet-writer
/career-superskills:interview-prep-generator
/career-superskills:job-description-analyzer
/career-superskills:cover-letter-generator
/career-superskills:linkedin-profile-optimizer
/career-superskills:salary-negotiation-prep
... (12 more)
```

## ✨ Features

- **20 Career-Focused Skills** - Full job search lifecycle
- **Zero-Config Install** - Run once, works everywhere
- **Curated Bundles** - Resume, Job Search, Portfolio
- **8 Platform Support** - GitHub Copilot, Claude Code, Codex, OpenCode, Gemini, Antigravity, Cursor, AdaL
- **No API Keys Required** - All skills run natively in your AI tool

## 📦 Available Skills

### 📄 Resume & CV
| Skill | Version | Purpose |
|-------|---------|---------|
| **resume-tailor** | v2.1.0 | Customize resume for a specific job posting while maintaining truthfulness — repositions experience, aligns language with role keywords |
| **resume-ats-optimizer** | v2.1.0 | Optimize resume for Applicant Tracking Systems — keyword analysis, formatting checks, and ATS compatibility scoring against a job description |
| **resume-bullet-writer** | v2.1.0 | Transform weak resume bullets into achievement-focused statements with metrics and impact |
| **resume-formatter** | v2.1.0 | Ensure ATS-friendly formatting and create a clean, scannable resume layout |
| **resume-quantifier** | v2.1.0 | Find opportunities to add metrics to resume bullets and estimate numbers when exact data is unavailable |
| **resume-section-builder** | v2.1.0 | Create targeted resume sections optimized for different experience levels and roles — summary, skills, education, and more |
| **resume-version-manager** | v2.1.0 | Track different resume versions, maintain a master resume, and manage tailored versions for multiple applications |
| **executive-resume-writer** | v2.1.0 | Create C-suite or VP-level resumes emphasizing strategic leadership, board-level impact, and P&L ownership |
| **tech-resume-optimizer** | v2.1.0 | Optimize resume for software engineering, product management, or technical roles — highlights engineering contributions and system design |
| **academic-cv-builder** | v2.1.0 | Format a curriculum vitae for academic positions — faculty, research, postdoc — organizing publications, grants, and teaching experience |
| **creative-portfolio-resume** | v2.1.0 | Balance visual design with ATS compatibility for creative role resumes — design, marketing, writing, UX |

### 🎯 Job Search & Applications
| Skill | Version | Purpose |
|-------|---------|---------|
| **job-description-analyzer** | v2.1.0 | Analyze job postings, calculate resume-to-job match scores, identify skill gaps, and create a targeted application strategy |
| **cover-letter-generator** | v2.1.0 | Create personalized, compelling cover letters from a resume and job description — handles career change narratives |
| **interview-prep-generator** | v2.1.0 | Generate STAR stories, behavioral practice questions, and talking points from a resume or job description |
| **linkedin-profile-optimizer** | v2.1.0 | Optimize LinkedIn profile for recruiter searchability — headlines, About sections, experience entries, and keyword density |
| **salary-negotiation-prep** | v2.1.0 | Research market rates, build a negotiation strategy, and create counter-offer scripts with data-backed confidence |
| **offer-comparison-analyzer** | v2.1.0 | Compare multiple job offers side-by-side with total compensation analysis — equity, benefits, scoring against personal priorities |
| **career-changer-translator** | v2.1.0 | Translate skills from one industry to another — identify transferable experience and rewrite bullets in target industry language |

### 🎨 Portfolio & Professional Presence
| Skill | Version | Purpose |
|-------|---------|---------|
| **portfolio-case-study-writer** | v2.1.0 | Transform resume bullets into detailed portfolio case studies — UX, product, or technical case studies with problem-solving narrative |
| **reference-list-builder** | v2.1.0 | Format professional references, prepare referees with talking points, and structure reference documentation for applications |

## 🎯 Curated Bundles

```bash
# Resume & CV (11 skills)
npx career-superskills --bundle resume -y

# Job Search (7 skills)
npx career-superskills --bundle job-search -y

# Portfolio & Presence (4 skills)
npx career-superskills --bundle portfolio -y

# All Career Skills (complete collection)
npx career-superskills --bundle all -y
```

## 🚀 Quick Start Examples

```bash
# Tailor resume to a specific job posting
claude -p "tailor my resume for this job: [paste JD]"

# Generate interview prep questions
claude -p "generate STAR interview stories from my resume for a PM role"

# Analyze compensation offer
claude -p "compare these two offers: [paste details]"

# Optimize LinkedIn for recruiter visibility
claude -p "rewrite my LinkedIn headline and About section for a VP Engineering role"
```

## 💻 Supported Platforms

- **GitHub Copilot CLI** - Terminal AI assistant (`~/.github/skills/`)
- **Claude Code** - Anthropic's Claude in development (`~/.claude/skills/`)
- **OpenAI Codex** - GPT-powered coding assistant (`~/.codex/skills/`)
- **OpenCode** - Open source AI coding assistant (`~/.agent/skills/`)
- **Gemini CLI** - Google's Gemini in terminal (`~/.gemini/skills/`)
- **Antigravity** - AI coding assistant (`~/.gemini/antigravity/skills/`)
- **Cursor IDE** - AI-powered code editor (`~/.cursor/skills/`)
- **AdaL CLI** - AI development assistant (`~/.adal/skills/`)

## ⌨️ Compatibility & Invocation

| Tool | Type | Invocation Example | Path |
|------|------|--------------------|------|
| **Claude Code** | CLI | `/resume-tailor help me...` | `~/.claude/skills/` |
| **Gemini CLI** | CLI | `Use resume-ats-optimizer to...` | `~/.gemini/skills/` |
| **Codex CLI** | CLI | `Use interview-prep-generator to...` | `~/.codex/skills/` |
| **Antigravity** | IDE | *(Agent Mode)* `Use skill...` | `~/.gemini/antigravity/skills/` |
| **Cursor** | IDE | `@cover-letter-generator` in Chat | `~/.cursor/skills/` |
| **Copilot** | Ext | *(Paste skill content manually)* | N/A |
| **OpenCode** | CLI | `opencode run @salary-negotiation-prep` | `~/.agent/skills/` |
| **AdaL CLI** | CLI | *(Auto)* Skills load on-demand | `~/.adal/skills/` |

## ⚡ CLI Commands & Shortcuts

| Command | Shortcut | Purpose |
|---------|----------|---------|
| `install` | `i` | Install skills |
| `list` | `ls` | List installed skills |
| `status` | `st` | Show global install status + version differences |
| `update` | `up` | Smart update (outdated + missing skills) |
| `uninstall` | `rm` | Remove skills |
| `doctor` | `doc` | Check installation |

```bash
npx career-superskills i -a -y -q    # Install all, skip prompts, quiet mode
npx career-superskills status         # Show global status + skill version differences
npx career-superskills up -y          # Update outdated + install missing skills
npx career-superskills ls -q          # List with minimal output
npx career-superskills --list-bundles # Show available bundles
```

## 📋 System Requirements

- Node.js 14+ (for installer)
- One or more supported platforms installed

## 🔒 Privacy

career-superskills does not collect, store, transmit, or share any user data.

- **No external servers** — no backend, no telemetry, no network requests of its own
- **No API keys required** — all skills run within your AI tool using native tools
- **No logging** — nothing is recorded outside of your local session
- **Open source** — all skill logic is fully auditable

## 📄 License

MIT - See [LICENSE](./LICENSE) for details.

## 🔗 Quick Links

- 📝 [Changelog](CHANGELOG.md) - Release history
- 🐛 [Issues](https://github.com/ericgandrade/career-superskills/issues) - Report problems

## Part of the Superskills Family

| Package | Skills | Focus | Install |
|---------|--------|-------|---------|
| [claude-superskills](https://github.com/ericgandrade/claude-superskills) | 18 | Core: orchestration, planning, research & content | `npx claude-superskills` |
| [obsidian-superskills](https://github.com/ericgandrade/obsidian-superskills) | 6 | Obsidian knowledge management | `npx obsidian-superskills` |
| **career-superskills** | 20 | Job search & career development | `npx career-superskills` |
| [product-superskills](https://github.com/ericgandrade/product-superskills) | 8 | Product management & GTM strategy | `npx product-superskills` |
| [design-superskills](https://github.com/ericgandrade/design-superskills) | 9 | UI/UX design, brand & diagrams | `npx design-superskills` |
| [avanade-superskills](https://github.com/ericgandrade/avanade-superskills) | 3 | Avanade-branded content (private) | `git clone git@github.com:ericgandrade/avanade-superskills.git` |

---

**Built with ❤️ by [Eric Andrade](https://github.com/ericgandrade)**

*Version 1.0.2 | May 2026*
