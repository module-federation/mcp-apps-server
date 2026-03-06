# Release Workflow

## Preview

Trigger: `.github/workflows/preview.yml` (manual)

Publishes a preview build via [pkg.pr.new](https://github.com/stackblitz-labs/pkg.pr.new) without occupying an npm version. Once triggered, pkg.pr.new automatically comments the install URL on the commit.

```bash
npm install https://pkg.pr.new/@module-federation/mcp-apps-server@{commit-sha}
```

> Prerequisite: install the [pkg-pr-new GitHub App](https://github.com/apps/pkg-pr-new) on the repository.

---

## Latest

Trigger: `.github/workflows/release.yml` (manual)

Publishes the current version to npm using OIDC Trusted Publishing. No `NPM_TOKEN` required.

**Before triggering, run locally:**

```bash
# 1. Describe the change (patch / minor / major)
pnpm changeset

# 2. Bump the version and generate CHANGELOG
pnpm run version:bump

# 3. Commit and push
git add .
git commit -m "chore: release"
git push
```

Then manually trigger the Release workflow in GitHub Actions.

Published packages include [Provenance](https://docs.npmjs.com/generating-provenance-statements) attestation linking the package to its CI build.

---

## One-time Setup

1. **npmjs.com** — Go to the package page → Settings → Trusted Publisher, add a GitHub Actions entry:
   - Owner: `module-federation`
   - Repository: `mcp-apps-server`
   - Workflow: `release.yml`
   - Environment: `npm`

2. **GitHub** — Create an environment named `npm` under repository Settings → Environments.

3. **GitHub App** — Install [pkg-pr-new](https://github.com/apps/pkg-pr-new) to enable preview publishing.
