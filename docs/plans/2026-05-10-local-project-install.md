# Local Project Install Feature — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use `executing-plans` to implement this plan task-by-task.

**Goal:** Add a local (project-scoped) install mode to all 5 superskills CLI installers so users can install skills into their current project directory instead of always writing to global `~/` paths.

**Architecture:** A new `--local` flag (and interactive scope prompt) routes install/update/uninstall flows through per-platform local paths (e.g., `.claude/skills/`, `.github/skills/` relative to `process.cwd()`). The platform installers already accept a `targetDirOverride` parameter — we only need to supply local paths to it. `skill-diff.js` gains a `buildPlatformSkillDiffForDir()` overload that accepts an arbitrary root instead of hardcoding the global home path. Changes are then propagated verbatim to the 4 sibling repos.

**Tech Stack:** Node.js, `fs-extra`, `inquirer`, `chalk`, `ora`, `semver` — all already installed. No new dependencies.

---

## UX Cases: Before vs. After

### Before (global only)

```
npx career-superskills
  → Detect platforms (shows ~/.claude/skills/, ~/.github/skills/, etc.)
  → Select platforms (checkbox, all detected checked)
  → Download/cache skills
  → Diff report (compares against global dirs)
  → "Apply recommended / Reinstall all / Uninstall / Cancel"
  → Writes to ~/.<platform>/skills/

npx career-superskills --local   ← THROWS: "Scope flags are no longer supported"
```

### After (global + local)

```
npx career-superskills
  → NEW: "Install scope?" [Global (default) | Local (current project)]
  → [Global path: same as before]
  → [Local path: writes to ./.claude/skills/, ./.github/skills/, etc.]

npx career-superskills --local   ← Skips scope prompt, goes straight to local
npx career-superskills --global  ← Skips scope prompt, goes straight to global (default)
npx career-superskills update --local  ← Updates only local install
npx career-superskills status         ← Shows both global AND local counts per platform
npx career-superskills uninstall --local  ← Removes local skills only

# Auto-detect: if .claude/skills/ exists in CWD, status shows "Local install detected"
```

### Local paths per platform

| Platform | Global (current) | Local (new) |
|----------|-----------------|-------------|
| Claude Code | `~/.claude/skills/` | `./.claude/skills/` |
| GitHub Copilot | `~/.github/skills/` | `./.github/skills/` |
| OpenAI Codex | `~/.codex/skills/` | `./.codex/skills/` |
| OpenCode | `~/.agent/skills/` | `./.agent/skills/` |
| Gemini CLI | `~/.gemini/skills/` | `./.gemini/skills/` |
| Antigravity | `~/.gemini/antigravity/skills/` | `./.gemini/antigravity/skills/` |
| Cursor IDE | `~/.cursor/skills/` | `./.cursor/skills/` |
| AdaL CLI | `~/.adal/skills/` | `./.adal/skills/` |

---

## Reference Repo

Implement everything in `career-superskills` first. All other repos share identical
`cli-installer/` structure — propagation is a file-copy (Task 10).

All paths below are relative to:
```
/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/career-superskills/cli-installer/
```

---

## Task 1: Add `getLocalSkillsPath()` to path-resolver.js

**Files:**
- Modify: `lib/utils/path-resolver.js`

**Background:** `getUserSkillsPath(platform)` returns `~/.{platform}/skills/`. We need a parallel
function that returns `./.{platform}/skills/` relative to a given project root (defaults to `process.cwd()`).

**Step 1: Read current file**

Open `lib/utils/path-resolver.js`. Current `getUserSkillsPath` is at lines 37-52.

**Step 2: Add the new function after line 52**

```javascript
/**
 * Get project-local skills path for a platform.
 * Returns a path relative to projectRoot (defaults to process.cwd()).
 * Mirrors getUserSkillsPath but rooted in the project directory.
 * @param {string} platform - Platform name
 * @param {string} [projectRoot] - Project root directory (defaults to process.cwd())
 * @returns {string}
 */
function getLocalSkillsPath(platform, projectRoot = process.cwd()) {
  const platformDirs = {
    'codex':       path.join(projectRoot, '.codex', 'skills'),
    'copilot':     path.join(projectRoot, '.github', 'skills'),
    'claude':      path.join(projectRoot, '.claude', 'skills'),
    'opencode':    path.join(projectRoot, '.agent', 'skills'),
    'gemini':      path.join(projectRoot, '.gemini', 'skills'),
    'antigravity': path.join(projectRoot, '.gemini', 'antigravity', 'skills'),
    'cursor':      path.join(projectRoot, '.cursor', 'skills'),
    'adal':        path.join(projectRoot, '.adal', 'skills')
  };

  return platformDirs[platform] || path.join(projectRoot, `.${platform}`, 'skills');
}
```

**Step 3: Add `getLocalSkillsPath` to `module.exports`**

```javascript
module.exports = {
  getCachedSkillsPath,
  getUserSkillsPath,
  getLocalSkillsPath,       // ← add this
  getCodexSkillPaths,
  isValidSkillName,
  assertSafePath
};
```

**Step 4: Manual smoke test**

```bash
cd cli-installer
node -e "
const { getLocalSkillsPath, getUserSkillsPath } = require('./lib/utils/path-resolver');
console.log('global:', getUserSkillsPath('claude'));
console.log('local: ', getLocalSkillsPath('claude'));
console.log('local custom:', getLocalSkillsPath('claude', '/tmp/myproject'));
"
```
Expected output:
```
global: /Users/<you>/.claude/skills
local:  /Users/<you>/Library/.../career-superskills/.claude/skills
local custom: /tmp/myproject/.claude/skills
```

**Step 5: Commit**

```bash
git add cli-installer/lib/utils/path-resolver.js
git commit -m "feat: add getLocalSkillsPath() for project-scoped installs"
```

---

## Task 2: Extend `skill-diff.js` with directory-aware diff

**Files:**
- Modify: `lib/utils/skill-diff.js`

**Background:** `buildPlatformSkillDiff(platform, cacheInventory)` calls `getInstalledSkillInventory(platform)`
which hardcodes the global path via `getUserSkillsPath`. We need a `targetDir`-aware variant for local mode,
plus an exported `getInstalledSkillInventoryForDir()` helper.

**Step 1: Add `getInstalledSkillInventoryForDir()` after line 66**

```javascript
/**
 * Get skill inventory for an arbitrary directory (not just global platform path).
 * Used for local project-scoped installs.
 * @param {string} skillsRoot - Directory containing skill folders
 * @returns {Map<string, {name: string, version: string|null}>}
 */
function getInstalledSkillInventoryForDir(skillsRoot) {
  const inventory = new Map();
  for (const skillDir of listSkillDirs(skillsRoot)) {
    const name = path.basename(skillDir);
    if (!fs.existsSync(path.join(skillDir, 'SKILL.md'))) continue;
    inventory.set(name, { name, version: readSkillVersion(skillDir) });
  }
  return inventory;
}
```

**Step 2: Add `buildPlatformSkillDiffForDir()` after the new function**

```javascript
/**
 * Build a skill diff against an explicit target directory instead of the global platform path.
 * Used for local scope installs.
 * @param {string} platform - Platform name (for labeling)
 * @param {Map} cacheInventory - From getCachedSkillInventory()
 * @param {string} targetDir - The local directory to compare against
 * @returns {Object} diff object (same shape as buildPlatformSkillDiff)
 */
function buildPlatformSkillDiffForDir(platform, cacheInventory, targetDir) {
  const installedInventory = getInstalledSkillInventoryForDir(targetDir);
  const diff = {
    platform,
    missing: [],
    outdated: [],
    upToDate: [],
    unknown: [],
    newer: []
  };

  for (const [skillName, latest] of cacheInventory.entries()) {
    const installed = installedInventory.get(skillName);
    if (!installed) {
      diff.missing.push({
        skill: skillName,
        installedVersion: null,
        latestVersion: latest.version || 'unknown'
      });
      continue;
    }

    const status = compareVersions(installed.version, latest.version);
    const item = {
      skill: skillName,
      installedVersion: installed.version || 'unknown',
      latestVersion: latest.version || 'unknown'
    };

    if (status === 'outdated') diff.outdated.push(item);
    else if (status === 'up_to_date') diff.upToDate.push(item);
    else if (status === 'newer') diff.newer.push(item);
    else diff.unknown.push(item);
  }

  diff.missing.sort((a, b) => a.skill.localeCompare(b.skill));
  diff.outdated.sort((a, b) => a.skill.localeCompare(b.skill));
  diff.upToDate.sort((a, b) => a.skill.localeCompare(b.skill));
  diff.newer.sort((a, b) => a.skill.localeCompare(b.skill));
  diff.unknown.sort((a, b) => a.skill.localeCompare(b.skill));

  return diff;
}
```

**Step 3: Export the new functions**

```javascript
module.exports = {
  getCachedSkillInventory,
  getInstalledSkillInventory,
  getInstalledSkillInventoryForDir,     // ← add
  buildPlatformSkillDiff,
  buildPlatformSkillDiffForDir,         // ← add
  hasChanges,
  getRecommendedSkills
};
```

**Step 4: Smoke test**

```bash
node -e "
const { buildPlatformSkillDiffForDir, getCachedSkillInventory } = require('./lib/utils/skill-diff');
const diff = buildPlatformSkillDiffForDir('claude', new Map(), '/tmp/nonexistent');
console.log('missing (should be 0, dir doesnt exist):', diff.missing.length);
"
```

**Step 5: Commit**

```bash
git add cli-installer/lib/utils/skill-diff.js
git commit -m "feat: add buildPlatformSkillDiffForDir() for local scope installs"
```

---

## Task 3: Add `promptScope()` to interactive.js

**Files:**
- Modify: `lib/interactive.js`

**Background:** We need a new prompt that asks users to choose between Global and Local scope.
Also, `promptPlatforms()` needs to show scope-contextual paths in its checkbox labels.

**Step 1: Add `promptScope()` after line 49 (before `promptPlatforms`)**

```javascript
/**
 * Ask user to choose install scope: global (~/.<platform>/skills/) or local (./<platform>/skills/).
 * @returns {Promise<'global'|'local'>}
 */
async function promptScope() {
  const cwd = process.cwd();
  const { scope } = await inquirer.prompt([{
    type: 'list',
    name: 'scope',
    message: 'Install scope:',
    choices: [
      {
        name: `Global  — ~/.<platform>/skills/  (available to all projects)`,
        value: 'global'
      },
      {
        name: `Local   — ${cwd}/.<platform>/skills/  (this project only)`,
        value: 'local'
      }
    ],
    default: 'global'
  }]);
  return scope;
}
```

**Step 2: Update `promptPlatforms()` to accept `scope` and `projectRoot` options**

Change the function signature and update each choice label to show the correct path.

Current signature (line 57):
```javascript
async function promptPlatforms(detected, options = {}) {
  const {
    message = 'Install skills for which platforms? (Press ESC to cancel)',
    defaultChecked = true,
    includeCowork = false
  } = options;
```

New signature:
```javascript
async function promptPlatforms(detected, options = {}) {
  const {
    message = 'Install skills for which platforms? (Press ESC to cancel)',
    defaultChecked = true,
    includeCowork = false,
    scope = 'global',
    projectRoot = process.cwd()
  } = options;
```

**Step 3: Update choice labels to show scope-contextual paths**

Replace the hardcoded `~/.X/skills/` paths in each choice with a helper:

Add this helper before the `choices` array is built:
```javascript
  const { getUserSkillsPath } = require('./utils/path-resolver');
  const { getLocalSkillsPath } = require('./utils/path-resolver');
  const getPath = (platform) => scope === 'local'
    ? getLocalSkillsPath(platform, projectRoot)
    : getUserSkillsPath(platform);
```

Then update each choice name to use `getPath()`. Example for claude:
```javascript
    choices.push({
      name: `✅ Claude Code (${getPath('claude')})`,
      value: 'claude',
      checked: defaultChecked
    });
```

Apply the same pattern to all 8 platforms (copilot, claude, codex, opencode, gemini, antigravity, cursor, adal).

**Step 4: Export `promptScope`**

```javascript
module.exports = { promptPlatforms, promptScope, setupEscapeHandler, confirmCancel };
```

**Step 5: Commit**

```bash
git add cli-installer/lib/interactive.js
git commit -m "feat: add promptScope() and scope-aware path labels in promptPlatforms()"
```

---

## Task 4: Re-enable scope flags in cli.js + add scope resolution

**Files:**
- Modify: `bin/cli.js`

**Background:**
- Line 48: `--local` and `--global` are in the blocked flags list — remove them.
- We need a `resolveScope(args)` function that returns `'global'` or `'local'`.
- `getPlatformTargetDir(platform)` currently always returns the global path — it needs a scope-aware version.

**Step 1: Remove `--local`, `-l`, `--global`, `-g`, `--scope` from `validateRemovedScopeFlags`**

Current (lines 47-53):
```javascript
function validateRemovedScopeFlags(args) {
  const removedFlags = ['--scope', '--claude-scope', '--global', '-g', '--local', '-l'];
  const used = removedFlags.filter((flag) => args.includes(flag));
  if (used.length > 0) {
    throw new Error(`Scope flags are no longer supported (${used.join(', ')}). Installation is always global.`);
  }
}
```

New (keep only the truly removed `--claude-scope`):
```javascript
function validateRemovedScopeFlags(args) {
  const removedFlags = ['--claude-scope'];
  const used = removedFlags.filter((flag) => args.includes(flag));
  if (used.length > 0) {
    throw new Error(`Deprecated flag: ${used.join(', ')}. Use --local or --global instead.`);
  }
}
```

**Step 2: Add `resolveScope()` utility after `validateRemovedScopeFlags`**

```javascript
/**
 * Determine install scope from CLI args.
 * Returns 'local' if --local or --scope local is present.
 * Returns 'global' if --global or --scope global is present.
 * Returns null if no explicit scope (caller should prompt).
 * @param {string[]} args
 * @returns {'global'|'local'|null}
 */
function resolveScope(args) {
  if (args.includes('--local') || args.includes('-l')) return 'local';
  if (args.includes('--global') || args.includes('-g')) return 'global';
  const scopeIdx = args.indexOf('--scope');
  if (scopeIdx !== -1) {
    const val = args[scopeIdx + 1];
    if (val === 'local') return 'local';
    if (val === 'global') return 'global';
  }
  return null;
}
```

**Step 3: Add `getTargetDirForPlatform()` — scope-aware path lookup**

Add after `getPlatformTargetDir` (currently line 186):

```javascript
/**
 * Return the install directory for a platform given an explicit scope.
 * @param {string} platform
 * @param {'global'|'local'} scope
 * @param {string} [projectRoot]
 * @returns {string|null}
 */
function getTargetDirForPlatform(platform, scope, projectRoot = process.cwd()) {
  const { getLocalSkillsPath } = require('./utils/path-resolver');
  return scope === 'local'
    ? getLocalSkillsPath(platform, projectRoot)
    : getUserSkillsPath(platform) || null;
}
```

**Step 4: Import `promptScope` at top of file**

Change line 4:
```javascript
const { promptPlatforms, setupEscapeHandler } = require('../lib/interactive');
```
To:
```javascript
const { promptPlatforms, promptScope, setupEscapeHandler } = require('../lib/interactive');
```

Also import `buildPlatformSkillDiffForDir` from skill-diff:
```javascript
const {
  getCachedSkillInventory,
  buildPlatformSkillDiff,
  buildPlatformSkillDiffForDir,   // ← add
  hasChanges,
  getRecommendedSkills
} = require('../lib/utils/skill-diff');
```

**Step 5: Parse scope flag early in `main()`**

In `main()` after `validateRemovedScopeFlags(args)` (currently line 685), add:
```javascript
  const scopeFlag = resolveScope(args);
```

**Step 6: Commit**

```bash
git add cli-installer/bin/cli.js
git commit -m "feat: re-enable --local/--global flags and add scope resolution"
```

---

## Task 5: Wire scope into install flow

**Files:**
- Modify: `bin/cli.js`

**Background:** The `install` command flow (lines 767-808) needs to:
1. Prompt for scope if `scopeFlag` is null (interactive mode)
2. Pass scope to `promptPlatforms` so labels show correct paths
3. Pass scope to a new `installForPlatformsWithScope()` helper instead of `installForPlatforms()`
4. Pass scope to diff building

**Step 1: Update `installForPlatforms()` to accept scope + projectRoot**

Current function (line 430):
```javascript
async function installForPlatforms(cacheDir, platforms, skills, quiet) {
  for (const platform of platforms) {
    const installer = getInstallerByPlatform(platform);
    if (!installer) continue;

    const targetDir = getPlatformTargetDir(platform);
    if (!targetDir) continue;

    await installer(cacheDir, skills, quiet, targetDir, `${platform} (global)`);
  }
}
```

New version:
```javascript
async function installForPlatforms(cacheDir, platforms, skills, quiet, scope = 'global', projectRoot = process.cwd()) {
  for (const platform of platforms) {
    const installer = getInstallerByPlatform(platform);
    if (!installer) continue;

    const targetDir = getTargetDirForPlatform(platform, scope, projectRoot);
    if (!targetDir) continue;

    const label = `${platform} (${scope})`;
    await installer(cacheDir, skills, quiet, targetDir, label);
  }
}
```

**Step 2: Update `buildDiffByPlatform()` to be scope-aware**

Current (line 442):
```javascript
function buildDiffByPlatform(platforms, cacheInventory) {
  const diffByPlatform = {};
  for (const platform of platforms) {
    diffByPlatform[platform] = buildPlatformSkillDiff(platform, cacheInventory);
  }
  return diffByPlatform;
}
```

New:
```javascript
function buildDiffByPlatform(platforms, cacheInventory, scope = 'global', projectRoot = process.cwd()) {
  const { getLocalSkillsPath } = require('./utils/path-resolver');
  const diffByPlatform = {};
  for (const platform of platforms) {
    if (scope === 'local') {
      const targetDir = getLocalSkillsPath(platform, projectRoot);
      diffByPlatform[platform] = buildPlatformSkillDiffForDir(platform, cacheInventory, targetDir);
    } else {
      diffByPlatform[platform] = buildPlatformSkillDiff(platform, cacheInventory);
    }
  }
  return diffByPlatform;
}
```

**Step 3: Thread `scope` through `runSmartInstallFlow()`**

Current signature (line 528):
```javascript
async function runSmartInstallFlow(detected, platforms, quiet, skipPrompt, options = {}) {
  const { includeCowork = false } = options;
```

New:
```javascript
async function runSmartInstallFlow(detected, platforms, quiet, skipPrompt, options = {}) {
  const { includeCowork = false, scope = 'global', projectRoot = process.cwd() } = options;
  const cacheDir = await warmCache(quiet);
  const cacheInventory = getCachedSkillInventory(cacheDir);
  const diffByPlatform = buildDiffByPlatform(platforms, cacheInventory, scope, projectRoot);
```

And update all `installForPlatforms(...)` calls inside this function to pass scope + projectRoot:
```javascript
  await installForPlatforms(cacheDir, platforms, null, quiet, scope, projectRoot);
  // and:
  await runInstallPlan(cacheDir, plan, quiet, scope, projectRoot);
```

**Step 4: Update `runInstallPlan()` to forward scope**

```javascript
async function runInstallPlan(cacheDir, plan, quiet, scope = 'global', projectRoot = process.cwd()) {
  for (const [platform, skills] of Object.entries(plan)) {
    await installForPlatforms(cacheDir, [platform], skills, quiet, scope, projectRoot);
  }
}
```

**Step 5: Wire scope into the `install` command block (lines 767-808)**

Locate the `install` command block. After detecting platforms and before `runSmartInstallFlow`, add scope resolution:

```javascript
    // Resolve scope: flag takes priority, otherwise prompt
    let scope = scopeFlag;
    if (!scope && !skipPrompt) {
      scope = await promptScope();
    }
    scope = scope || 'global';

    if (!quiet) {
      const scopeLabel = scope === 'local'
        ? `local (${process.cwd()})`
        : 'global (~/)';
      console.log(chalk.cyan(`\n📦 Installation target: ${platforms.join(', ')} — scope: ${scopeLabel}\n`));
    }
```

Also pass `scope` and `projectRoot` to `promptPlatforms`:
```javascript
    platforms = await promptPlatforms(detected, {
      includeCowork: true,
      scope,
      projectRoot: process.cwd()
    });
```

And pass to `runSmartInstallFlow`:
```javascript
    await runSmartInstallFlow(detected, installPlatforms, quiet, skipPrompt, {
      includeCowork,
      scope,
      projectRoot: process.cwd()
    });
```

**Step 6: Manual integration test — local install**

```bash
cd /tmp && mkdir test-project && cd test-project
npx career-superskills install --local -y -q
ls -la .claude/skills/ 2>/dev/null && echo "✅ Local install works" || echo "❌ Local install failed"
rm -rf /tmp/test-project
```

Expected: skill directories created inside `.claude/skills/` within `/tmp/test-project`.

**Step 7: Manual integration test — global install unchanged**

```bash
cd /tmp
npx career-superskills install --global -y -q
ls ~/.claude/skills/ | head -3
echo "✅ Global install still works"
```

**Step 8: Commit**

```bash
git add cli-installer/bin/cli.js
git commit -m "feat: wire scope into install flow (installForPlatforms, buildDiffByPlatform, runSmartInstallFlow)"
```

---

## Task 6: Wire scope into `update` and `uninstall` commands

**Files:**
- Modify: `bin/cli.js`

**Background:** The `update` and `uninstall` commands also need scope awareness.

**Step 1: Update `runUpdateCommand()` to accept and forward scope**

Current signature (line 627):
```javascript
async function runUpdateCommand(detected, quiet, skipPrompt) {
```

New:
```javascript
async function runUpdateCommand(detected, quiet, skipPrompt, scope = 'global', projectRoot = process.cwd()) {
```

Forward to `runSmartInstallFlow`:
```javascript
    await runSmartInstallFlow(detected, platforms, quiet, skipPrompt, {
      includeCowork: hasCowork,
      scope,
      projectRoot
    });
```

**Step 2: Add `uninstallManagedSkillsForScope()` alongside the existing `uninstallManagedSkills()`**

```javascript
async function uninstallManagedSkillsForScope(platforms, quiet, scope = 'global', projectRoot = process.cwd()) {
  if (scope === 'local') {
    const { getLocalSkillsPath } = require('./utils/path-resolver');
    let managedSkills = getManagedSkillNames();
    let removedCount = 0;

    if (managedSkills.length === 0) {
      console.log(chalk.red('❌ Could not determine managed skills list. Uninstall aborted.\n'));
      return;
    }

    for (const platform of platforms) {
      const targetDir = getLocalSkillsPath(platform, projectRoot);
      if (!(await fs.pathExists(targetDir))) continue;
      for (const skill of managedSkills) {
        const skillDir = path.join(targetDir, skill);
        if (await fs.pathExists(skillDir)) {
          await fs.remove(skillDir);
          removedCount++;
        }
      }
    }

    if (!quiet) {
      console.log(chalk.gray(`🧹 Uninstalled ${removedCount} local skill folder(s)`));
    }
  } else {
    await uninstallManagedSkills(platforms, quiet);
  }
}
```

**Step 3: Update `runUninstallFlow()` to accept scope**

Current signature (line 315):
```javascript
async function runUninstallFlow(detected, quiet, skipPrompt) {
```

New:
```javascript
async function runUninstallFlow(detected, quiet, skipPrompt, scope = 'global', projectRoot = process.cwd()) {
```

Replace the call to `uninstallManagedSkills`:
```javascript
  // Old:
  await uninstallManagedSkills(selectedPlatforms, quiet);
  // New:
  await uninstallManagedSkillsForScope(selectedPlatforms, quiet, scope, projectRoot);
```

Also update the mode note shown to user:
```javascript
  // Old:
  console.log(chalk.gray('   Mode: global only'));
  // New:
  console.log(chalk.gray(`   Mode: ${scope}`));
```

**Step 4: Wire scope into `update` and `uninstall` switch cases in `main()`**

In the `update` case (line 831):
```javascript
    case 'update': {
      const detected = detectTools();
      let scope = scopeFlag;
      if (!scope && !skipPrompt) {
        scope = await promptScope();
      }
      scope = scope || 'global';
      await runUpdateCommand(detected, quiet, skipPrompt, scope, process.cwd());
      break;
    }
```

In the `uninstall` case (line 843):
```javascript
    case 'uninstall':
      if (args.includes('--nuclear')) {
        await runNuclearFlow(quiet);
      } else {
        console.log(chalk.cyan('🔍 Detecting installed AI CLI tools...\n'));
        let scope = scopeFlag;
        if (!scope && !skipPrompt) {
          scope = await promptScope();
        }
        scope = scope || 'global';
        await runUninstallFlow(detectTools(), quiet, skipPrompt, scope, process.cwd());
      }
      break;
```

**Step 5: Commit**

```bash
git add cli-installer/bin/cli.js
git commit -m "feat: wire scope into update and uninstall commands"
```

---

## Task 7: Update `status` and `list` commands to show local installs

**Files:**
- Modify: `bin/cli.js`

**Background:** `status` and `list` should show both global and local counts so users know what's installed where.

**Step 1: Update `getPlatformInstallStatus()` to also detect local count**

Current (line 202):
```javascript
function getPlatformInstallStatus(platform) {
  const managedSkills = getManagedSkillNames();
  const globalDir = getPlatformTargetDir(platform);
  const globalCount = getInstalledManagedCount(globalDir, managedSkills);

  return {
    globalDir,
    globalCount,
    hasGlobal: globalCount > 0
  };
}
```

New:
```javascript
function getPlatformInstallStatus(platform, projectRoot = process.cwd()) {
  const { getLocalSkillsPath } = require('./utils/path-resolver');
  const managedSkills = getManagedSkillNames();
  const globalDir = getPlatformTargetDir(platform);
  const globalCount = getInstalledManagedCount(globalDir, managedSkills);
  const localDir = getLocalSkillsPath(platform, projectRoot);
  const localCount = getInstalledManagedCount(localDir, managedSkills);

  return {
    globalDir,
    globalCount,
    hasGlobal: globalCount > 0,
    localDir,
    localCount,
    hasLocal: localCount > 0
  };
}
```

**Step 2: Update `printGlobalStatus()` to show local counts too**

Rename to `printInstallStatus()` and show both rows:

```javascript
function printInstallStatus(detected, projectRoot = process.cwd()) {
  const detectedPlatforms = getDetectedPlatforms(detected);
  if (detectedPlatforms.length === 0) {
    console.log(chalk.yellow('\n⚠️  No supported platforms detected.\n'));
    return;
  }

  console.log(chalk.cyan('\n📊 Installation status\n'));

  for (const platform of detectedPlatforms) {
    const status = getPlatformInstallStatus(platform, projectRoot);
    const globalDir = status.globalDir || '(n/a)';
    const globalLine = `${status.hasGlobal ? '✅' : '⬜'} ${status.globalCount} skill(s)`;
    const localLine = `${status.hasLocal ? '✅' : '⬜'} ${status.localCount} skill(s)`;

    console.log(chalk.bold(`• ${platform}`));
    console.log(chalk.dim(`  global: ${globalLine}  → ${globalDir}`));
    console.log(chalk.dim(`  local:  ${localLine}  → ${status.localDir}`));
  }
  console.log();
}
```

**Step 3: Update `runStatusCommand()` to call the renamed function**

```javascript
async function runStatusCommand() {
  const detected = detectTools();
  printInstallStatus(detected, process.cwd());
  // ... rest unchanged
}
```

**Step 4: Update `list` command to include local skills**

In the `list` case (line 812), after listing global skills, add:

```javascript
    case 'list': {
      console.log('📋 Installed Skills:\n');
      const detected = detectTools();
      const platforms = getDetectedPlatforms(detected);
      const { getLocalSkillsPath } = require('./utils/path-resolver');

      const globalSkills = await getInstalledSkillsByPlatforms(platforms);
      if (globalSkills.length > 0) {
        console.log(chalk.dim('  Global:'));
        globalSkills.forEach((skill) => console.log(`    • ${skill}`));
      } else {
        console.log(chalk.dim('  Global: (none)'));
      }

      // Local installs
      const localTargets = platforms.map((p) => getLocalSkillsPath(p, process.cwd()));
      const localInstalled = new Set();
      for (const targetDir of localTargets) {
        if (!(await fs.pathExists(targetDir))) continue;
        const entries = await fs.readdir(targetDir);
        for (const entry of entries) {
          const skillDir = path.join(targetDir, entry);
          if (!(await fs.pathExists(path.join(skillDir, 'SKILL.md')))) continue;
          localInstalled.add(entry);
        }
      }
      if (localInstalled.size > 0) {
        console.log(chalk.dim(`\n  Local (${process.cwd()}):`));
        [...localInstalled].sort().forEach((skill) => console.log(`    • ${skill}`));
      } else {
        console.log(chalk.dim(`\n  Local: (none)`));
      }
      console.log();
      break;
    }
```

**Step 5: Commit**

```bash
git add cli-installer/bin/cli.js
git commit -m "feat: show local install counts in status and list commands"
```

---

## Task 8: Update `showHelp()` and `README.md`

**Files:**
- Modify: `bin/cli.js` (showHelp function, line 862)
- Modify: `cli-installer/README.md`

**Step 1: Update `showHelp()` to document scope flags**

Add to the Options section:
```
  --local, -l     Install to current project directory
  --global, -g    Install to global home directory (default)
  --scope <s>     Set scope explicitly: local or global
```

Remove or update the note:
```
  - Installation is always global.
```
Replace with:
```
  - Default scope is global (~/.{platform}/skills/).
  - Use --local to install into the current project directory (./{platform}/skills/).
```

**Step 2: Update examples in `showHelp()`**

Add:
```
  npx career-superskills --local                   # Install to current project
  npx career-superskills --local -y                # Auto install locally
  npx career-superskills update --local            # Update local skills only
  npx career-superskills uninstall --local -y      # Remove local skills
  npx career-superskills status                    # Show global + local counts
```

**Step 3: Update `cli-installer/README.md`**

Add a "Local vs Global Install" section explaining:
- Default is global
- `--local` flag installs to project directory
- Useful for project-specific AI configurations
- Shows the local path structure table

**Step 4: Commit**

```bash
git add cli-installer/bin/cli.js cli-installer/README.md
git commit -m "docs: document --local scope flag in help and README"
```

---

## Task 9: End-to-end integration test

**No file changes — validation only.**

**Step 1: Test local install (new feature)**

```bash
cd /tmp && mkdir e2e-test && cd e2e-test
npx career-superskills install --local --yes --quiet
echo "--- Local Claude skills ---"
ls .claude/skills/ 2>/dev/null || echo "(no local claude skills)"
echo "--- Global Claude skills (should be unchanged) ---"
ls ~/.claude/skills/ | wc -l
```
Expected: Skills appear in `./.claude/skills/`, global dir unchanged.

**Step 2: Test local status shows both**

```bash
cd /tmp/e2e-test
npx career-superskills status
```
Expected: Each platform shows `global: ✅ N skill(s)` and `local: ✅ N skill(s)`.

**Step 3: Test local update (idempotent)**

```bash
cd /tmp/e2e-test
npx career-superskills update --local --yes --quiet
echo "✅ Update idempotent — no errors"
```

**Step 4: Test local uninstall**

```bash
cd /tmp/e2e-test
npx career-superskills uninstall --local --yes --quiet
ls .claude/skills/ 2>/dev/null || echo "✅ Local skills removed"
```

**Step 5: Test global mode unchanged (regression)**

```bash
cd /tmp
npx career-superskills install --global --yes --quiet
ls ~/.claude/skills/ | head -3 && echo "✅ Global install still works"
```

**Step 6: Test interactive scope prompt**

```bash
cd /tmp/e2e-test
npx career-superskills install
# Manually select "Local" at the scope prompt, verify install works
```

**Step 7: Cleanup**

```bash
rm -rf /tmp/e2e-test
```

**Step 8: Commit (if any fix was needed)**

```bash
git add -p
git commit -m "fix: integration test corrections for local install"
```

---

## Task 10: Propagate to 4 sibling repos

**Background:** All 5 repos share identical `cli-installer/` architecture. Once Task 1-9 are done
and working in `career-superskills`, copy the 5 changed files to each sibling.

**Files changed in career-superskills (to propagate):**

| File | Task |
|------|------|
| `cli-installer/lib/utils/path-resolver.js` | Task 1 |
| `cli-installer/lib/utils/skill-diff.js` | Task 2 |
| `cli-installer/lib/interactive.js` | Task 3 |
| `cli-installer/bin/cli.js` | Tasks 4-8 |
| `cli-installer/README.md` | Task 8 |

**Repos to propagate to:**

| Repo | cli-installer path | Package name note |
|------|-------------------|-------------------|
| `claude-superskills` | `cli-installer/` | Note: version is `2.0.2`, cache dir is `.claude-superskills` |
| `design-superskills` | `cli-installer/` | Cache dir: `.design-superskills` |
| `obsidian-superskills` | `cli-installer/` | Cache dir: `.obsidian-superskills` |
| `product-superskills` | `cli-installer/` | Cache dir: `.product-superskills` |

**Step 1: Check for repo-specific divergences before copying**

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  echo "=== $repo ==="
  diff "$base/career-superskills/cli-installer/bin/cli.js" \
       "$base/$repo/cli-installer/bin/cli.js" | head -20
done
```

Review diffs. Look for repo-specific package name references (e.g., `.career-superskills` → `.design-superskills`).

**Step 2: Copy and patch path-resolver.js (no repo-specific content)**

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  cp "$base/career-superskills/cli-installer/lib/utils/path-resolver.js" \
     "$base/$repo/cli-installer/lib/utils/path-resolver.js"
  cp "$base/career-superskills/cli-installer/lib/utils/skill-diff.js" \
     "$base/$repo/cli-installer/lib/utils/skill-diff.js"
  cp "$base/career-superskills/cli-installer/lib/interactive.js" \
     "$base/$repo/cli-installer/lib/interactive.js"
  echo "✅ Copied shared files to $repo"
done
```

**Step 3: Patch cli.js per repo (replace package-name-specific strings)**

For each sibling repo, copy `bin/cli.js` and replace `career-superskills` with the repo's package name in:
- Cache path references (`.career-superskills/cache`)
- Console messages (the banner, uninstall path messages, help text)

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  # Extract actual package name from package.json
  pkg=$(node -e "console.log(require('$base/$repo/cli-installer/package.json').name)")
  cp "$base/career-superskills/cli-installer/bin/cli.js" \
     "$base/$repo/cli-installer/bin/cli.js"
  # Replace career-superskills with actual package name in the file
  sed -i '' "s/career-superskills/$pkg/g" "$base/$repo/cli-installer/bin/cli.js"
  echo "✅ Patched cli.js for $repo (package: $pkg)"
done
```

**Step 4: Verify each patched cli.js still has the right package references**

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  echo "=== $repo cache references ==="
  grep "cache" "$base/$repo/cli-installer/bin/cli.js" | grep -v "^//" | head -5
done
```

**Step 5: Smoke test each sibling**

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  node "$base/$repo/cli-installer/bin/cli.js" --help 2>&1 | grep -E "(local|scope|Install)" | head -5
  echo "✅ $repo help works"
done
```

Expected: `--local` appears in help output for all 4 repos.

**Step 6: Commit each sibling repo**

```bash
for repo in claude-superskills design-superskills obsidian-superskills product-superskills; do
  base="/Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects"
  cd "$base/$repo"
  /opt/homebrew/bin/git add cli-installer/bin/cli.js \
    cli-installer/lib/utils/path-resolver.js \
    cli-installer/lib/utils/skill-diff.js \
    cli-installer/lib/interactive.js \
    cli-installer/README.md
  /opt/homebrew/bin/git commit -m "feat: add local project install scope (--local flag)"
  echo "✅ Committed $repo"
done
```

**Step 7: Final commit in career-superskills**

```bash
cd /Users/avanade/Library/CloudStorage/OneDrive-Avanade/14_Code_Projects/career-superskills
git status
git commit -m "feat: complete local project install feature — all 5 repos updated"
```

---

## Summary: Files Changed Per Repo

| File | Repos |
|------|-------|
| `cli-installer/lib/utils/path-resolver.js` | All 5 |
| `cli-installer/lib/utils/skill-diff.js` | All 5 |
| `cli-installer/lib/interactive.js` | All 5 |
| `cli-installer/bin/cli.js` | All 5 (with per-repo name patch) |
| `cli-installer/README.md` | All 5 |

**Total: 5 files × 5 repos = 25 file changes.**

No new npm dependencies. No schema changes. No version bump required (feature addition, bump minor when shipping).

---

[████████████████████] 100% — Phase 4/4: Plan saved.
