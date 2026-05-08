# Getting Started with CLI AI Skills

Welcome to CLI AI Skills! This guide will help you get up and running in minutes.

## 🚀 Quick Start (3 Commands)

### 1. Install with Zero Config

The simplest way to get started is to run the installer with no arguments:

```bash
npx career-superskills
```

This will automatically:
- 🔍 Detect your installed platforms (GitHub Copilot CLI, Claude Code, Codex)
- 📦 Install all 4 available skills
- ✅ Configure for immediate use
- 🎯 Enable all features

**No prompts. No decisions. Just works.**

### 2. List Available Skills

See what's installed:

```bash
npx career-superskills ls
```

Or the long form:

```bash
npx career-superskills list
```

### 3. Get Help

```bash
npx career-superskills help
```

Or check a specific command:

```bash
npx career-superskills install --help
```

---

## 📦 Our 4 Universal Skills

CLI AI Skills comes with 4 expertly crafted skills that work across all supported platforms:

### 1. **skill-creator** (v1.3.0)
Automate the entire workflow of creating new skills from brainstorming to installation.
- **When to use:** Building custom skills, extending the CLI
- **Category:** Meta (tool for creating tools)
- **Status:** Safe ✅

### 2. **prompt-engineer** (v1.1.0)
Transform user prompts into optimized prompts using proven frameworks like RTF, RISEN, Chain of Thought, and more.
- **When to use:** Improving AI prompt quality, optimizing responses
- **Category:** Automation
- **Status:** Safe ✅

### 3. **youtube-summarizer** (v1.2.0)
Extract and summarize YouTube video content with technical metadata.
- **When to use:** Processing video transcripts, creating summaries
- **Category:** Content
- **Status:** Safe ✅

### 4. **audio-transcriber** (v1.2.0)
Transcribe audio files to Markdown with speaker detection, meeting minutes generation.
- **When to use:** Converting audio to text, creating meeting notes
- **Category:** Content
- **Status:** Safe ✅

---

## 🎯 Curated Bundles

Instead of installing everything, you can install just what you need:

### Essential Bundle
Perfect for getting started:
```bash
npx career-superskills --bundle essential -y
```
Includes: **skill-creator**, **prompt-engineer**

### Content Bundle
For video and audio processing:
```bash
npx career-superskills --bundle content -y
```
Includes: **youtube-summarizer**, **audio-transcriber**

### Developer Bundle
For creating custom skills:
```bash
npx career-superskills --bundle developer -y
```
Includes: **skill-creator**

### All Skills
Complete collection:
```bash
npx career-superskills --bundle all -y
```
Includes: all 4 skills

**See [Bundles Guide](../bundles/bundles.md) for detailed descriptions.**

---

## 🔍 Search Skills

Can't remember a skill name? Search for it:

```bash
npx career-superskills --search "prompt"
npx career-superskills --search "video"
npx career-superskills --search "transcription"
```

Search works across skill names, descriptions, tags, and categories.

---

## ⚡ Command Shortcuts

Use short commands for faster typing:

| Shortcut | Full Command | Purpose |
|----------|--------------|---------|
| `i` | `install` | Install skills |
| `ls` | `list` | List installed skills |
| `up` | `update` | Update skills |
| `rm` | `uninstall` | Remove skills |
| `doc` | `doctor` | Check installation health |

Examples:
```bash
npx career-superskills i -a -y    # Install all with flags
npx career-superskills ls          # Quick list
npx career-superskills up          # Update skills
```

---

## 🚩 Short Flags

Combine with short flags for maximum efficiency:

| Flag | Full Form | Purpose |
|------|-----------|---------|
| `-a` | `--all` | Operate on all skills |
| `-g` | `--global` | Global installation |
| `-l` | `--local` | Local installation |
| `-y` | `--yes` | Skip confirmations |
| `-q` | `--quiet` | Minimal output |

Examples:
```bash
npx career-superskills install -a -y -q    # Install everything silently
npx career-superskills list -q             # List without extra formatting
npx career-superskills update -g -y        # Global update with confirmation skip
```

---

## 💻 Supported Platforms

CLI AI Skills works on:

- **🤖 GitHub Copilot CLI** - GitHub's AI coding assistant
- **🧠 Claude Code** - Anthropic's Claude in VS Code
- **⚙️ Codex** - OpenAI's code understanding model

All skills are compatible with all platforms. Choose what you use!

---

## 🎓 Next Steps

Now that you're installed, explore:

1. **[Skill Anatomy Guide](skill-anatomy.md)** - Understand how skills work internally
2. **[Bundles Guide](../bundles/bundles.md)** - Deep dive into skill collections
3. **[Quality Standards](quality-standards.md)** - Learn best practices for using skills
4. **[Complete Catalog](../../CATALOG.md)** - Browse all skills with metadata

---

## ❓ Troubleshooting

### "Command not found"
```bash
# Ensure you have Node.js installed
node --version

# Install globally instead of npx
npm install -g career-superskills
npx career-superskills
```

### "No platforms detected"
```bash
# Check if your platform is installed
npx career-superskills doctor
```

### "Permission denied"
```bash
# Try with sudo (not recommended)
sudo npx career-superskills -g

# Or use local install
npx career-superskills
```

### Need more help?
Check the [main README](../../README.md) or open an issue on GitHub.

---

## 🔗 Quick Links

- 📚 [Full Documentation](../../README.md)
- 📋 [Complete Skill Catalog](../../CATALOG.md)
- 📦 [Bundles Guide](../bundles/bundles.md)
- 💡 [Skill Anatomy](skill-anatomy.md)
- ✅ [Quality Standards](quality-standards.md)
- 🤝 [Contributing](../../CONTRIBUTING.md)
- 📝 [Changelog](../../CHANGELOG.md)

---

**Happy coding! 🚀**
