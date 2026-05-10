# Versioning Guide — career-superskills

## Core Concepts

### Latest (npm tag)
- **What it is:** Default npm tag pointing to the latest stable version
- **When it changes:** Every time you publish a new version via `npm publish`
- **How users access it:** `npm install career-superskills` or `npx career-superskills`

### Main (Git branch)
- **What it is:** The primary source branch on GitHub
- **State:** May be "ahead" of the version published on npm
- **Relationship with npm:** Code in `main` → git tag `v*` → GitHub Actions → npm publish

### Semantic Versioning (SemVer)

Format: `MAJOR.MINOR.PATCH`

```
v1.2.3
 │ │ │
 │ │ └─ PATCH: Bug fixes (1.2.3 → 1.2.4)
 │ └─── MINOR: New features, backward-compatible (1.2.3 → 1.3.0)
 └───── MAJOR: Breaking changes (1.2.3 → 2.0.0)
```

## Release Workflow

### 1. Bug Fix (PATCH: 1.0.0 → 1.0.1)

**When to use:**
- Bug fixes
- Performance improvements
- Documentation corrections

**Steps:**

```bash
# 1. Commit fixes
git add .
git commit -m "fix: correct error in install command"

# 2. Bump version (creates commit + tag)
cd cli-installer
npm version patch

# 3. Update CHANGELOG
vim CHANGELOG.md
# Add version entry

# 4. Commit CHANGELOG
git add CHANGELOG.md
git commit --amend --no-edit

# 5. Push (triggers GitHub Actions)
git push origin main --tags

# 6. Wait for publication (~2 min)
# Check at: https://github.com/ericgandrade/career-superskills/actions
```

### 2. New Feature (MINOR: 1.0.0 → 1.1.0)

**When to use:**
- New command
- New functionality
- Improvements that maintain compatibility

**Steps:**

```bash
# 1. Commit the feature
git add .
git commit -m "feat: add 'info' command to show skill details"

# 2. Bump version
cd cli-installer
npm version minor

# 3. Update CHANGELOG
vim CHANGELOG.md

# 4. Commit CHANGELOG
git add CHANGELOG.md
git commit --amend --no-edit

# 5. Push
git push origin main --tags
```

### 3. Breaking Change (MAJOR: 1.0.0 → 2.0.0)

**When to use:**
- Changes that break the existing API
- Removal of commands/features
- Incompatible structural reorganization

**Steps:**

```bash
# 1. Commit changes
git add .
git commit -m "feat!: remove 'uninstall' command, use 'remove' now

BREAKING CHANGE: 'uninstall' command has been removed, use 'remove' instead"

# 2. Bump version
cd cli-installer
npm version major

# 3. Update CHANGELOG with highlighted BREAKING CHANGES
vim CHANGELOG.md

# 4. Commit CHANGELOG
git add CHANGELOG.md
git commit --amend --no-edit

# 5. Push
git push origin main --tags
```

## CHANGELOG Format

Keep in `cli-installer/CHANGELOG.md`:

```markdown
# Changelog

All notable changes to this project will be documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Features in development

## [1.1.0] - 2026-02-15

### Added
- New `info` command to show skill details
- Colored output for better readability

### Fixed
- Progress gauge now works on Windows

### Changed
- Improved performance of the `list` command

## [1.0.1] - 2026-02-05

### Fixed
- Fixed error in version checker when parsing YAML

## [1.0.0] - 2026-02-02

### Added
- Initial release
- 5 commands: install, list, update, uninstall, doctor
- Dual-platform support (Copilot + Claude)
- Visual progress gauges
- Automatic version checking
- GitHub Actions CI/CD

[Unreleased]: https://github.com/ericgandrade/career-superskills/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/ericgandrade/career-superskills/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/ericgandrade/career-superskills/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/ericgandrade/career-superskills/releases/tag/v1.0.0
```

## Useful Commands

```bash
# Check current version
cat cli-installer/package.json | grep version

# See all versions published on npm
npm view career-superskills versions

# See latest version info
npm view career-superskills

# Test a specific version
npx career-superskills@1.0.1 --version

# See local git tags
git tag

# See remote git tags
git ls-remote --tags origin

# Delete tag (if you made a mistake)
git tag -d v1.0.1                    # Local
git push origin :refs/tags/v1.0.1    # Remote
```

## Troubleshooting

### Error: "Tag already exists"

```bash
# If you already created the tag locally
git tag -d v1.0.1

# If the tag exists remotely
git push origin :refs/tags/v1.0.1

# Recreate tag
cd cli-installer
npm version patch --force
git push origin main --tags
```

### Error: "npm publish failed in GitHub Actions"

1. Check logs: https://github.com/ericgandrade/career-superskills/actions
2. Common cause: Expired npm token (expires every 90 days)
3. Solution: Create new token and update GitHub Secret NPM_TOKEN

### Publish version manually (emergency)

```bash
cd cli-installer
npm publish
# Requires OTP from authenticator or token with 2FA bypass
```

## Reminder: npm Token

⚠️ **npm token expires in 90 days**

Steps to renew:
1. Create a new Granular Access Token on npm (type "Automation")
2. Check "Bypass 2FA for noninteractive automated workflows"
3. Update GitHub Secret NPM_TOKEN
4. No need to republish; the next version will use the new token

## Recommended Strategy

1. **Continuous development:** Work in `main` normally
2. **Frequent commits:** Use conventional commits (feat:, fix:, docs:)
3. **Release when ready:** Only bump version when you want to publish
4. **Always update CHANGELOG:** Document all changes
5. **Test first:** Use `npm link` to test locally before publishing

## Current State

```
Git main branch: career-superskills (source code)
├─ package.json: v1.0.0
├─ Git tag: v1.0.0
└─ GitHub Actions: Triggered by v* tag

npm registry: career-superskills
├─ Published version: 1.0.0
├─ Tag: latest
└─ Available: npx career-superskills
```

## Useful Links

- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [npm version](https://docs.npmjs.com/cli/v8/commands/npm-version)
- [GitHub Actions Workflow](.github/workflows/publish-npm.yml)
