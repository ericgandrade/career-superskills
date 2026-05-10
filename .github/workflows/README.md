# GitHub Actions Workflows

## 📦 Publish to npm

Automatic workflow that publishes the `career-superskills` package to npm when you create a version tag.

### How to Use

#### 1️⃣ Configure NPM Token (once only)

1. Create an **Automation Token** on npm:
   - Go to: https://www.npmjs.com/settings/~/tokens/create
   - Token Type: **Automation** (important!)
   - Token Name: `github-actions-career-superskills`
   - Expiration: No expiration
   - Copy the token (starts with `npm_...`)

2. Add as a Secret on GitHub:
   - Go to: https://github.com/ericgandrade/career-superskills/settings/secrets/actions
   - Click **"New repository secret"**
   - Name: `NPM_TOKEN`
   - Value: Paste the npm token
   - Click **"Add secret"**

#### 2️⃣ Publish a New Version

Whenever you want to publish a new version:

```bash
# 1. Update version in package.json
cd cli-installer
npm version patch   # For bug fixes (1.0.0 → 1.0.1)
# or
npm version minor   # For new features (1.0.0 → 1.1.0)
# or
npm version major   # For breaking changes (1.0.0 → 2.0.0)

# 2. Push the tag
git push origin main --tags

# 3. GitHub Actions publishes automatically! 🎉
```

#### 3️⃣ Monitor Publication

- Go to: https://github.com/ericgandrade/career-superskills/actions
- Watch the "Publish to npm" workflow run
- ✅ When done, the package is live on npm!

### What the Workflow Does

1. ✅ Detects when you push a `v*` tag
2. ✅ Checks out the code
3. ✅ Installs Node.js 20
4. ✅ Installs dependencies (`npm ci`)
5. ✅ Runs tests (`npm test`)
6. ✅ Publishes to npm (`npm publish`)

### Advantages

- ✅ **No local 2FA**: Token is securely stored in GitHub
- ✅ **Automatic**: Push tag → Published
- ✅ **Tested**: Runs tests before publishing
- ✅ **Traceable**: Full logs on GitHub
- ✅ **Professional**: Industry-standard CI/CD

### Troubleshooting

**Error: "npm ERR! need auth"**
- Check that the `NPM_TOKEN` secret is configured
- Check that the token type is "Automation"

**Error: "npm ERR! 403 Forbidden"**
- Token expired or revoked
- Create a new token and update the secret

**Workflow did not run**
- Make sure the tag starts with `v` (e.g. `v1.0.0`)
- Verify you pushed the tag: `git push --tags`

### Full Example

```bash
# Prepare new version
cd cli-installer
npm version patch
# Output: v1.0.1

# Commit and push (tag was created automatically)
git add package.json package-lock.json
git commit -m "chore: bump version to 1.0.1"
git push origin main
git push origin v1.0.1

# Wait for GitHub Actions to publish
# See at: https://github.com/ericgandrade/career-superskills/actions
```

### Useful Links

- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [npm Publishing Docs](https://docs.npmjs.com/using-private-packages-in-a-ci-cd-workflow)
- [Secrets Configuration](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
