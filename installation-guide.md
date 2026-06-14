# Installation Guide

This guide covers every installation scenario in detail.

---

## Standard Installation

### 1. Extract the Package

```bash
unzip nexora-ai.zip -d nexora-ai
cd nexora-ai
```

### 2. Install Dependencies

```bash
npm install
```

This installs all packages listed in `package.json` and creates a `node_modules/` directory. A `package-lock.json` file is included to ensure reproducible installs.

### 3. Verify the Installation

```bash
npm run typecheck
```

A successful run prints no errors. You are ready to develop.

---

## CI / Automated Environments

For CI pipelines (GitHub Actions, GitLab CI, CircleCI, etc.) use `npm ci` instead of `npm install` for faster, strict-lockfile installs:

```bash
npm ci
npm run build
```

`npm ci` requires `package-lock.json` to be present (included in this package).

### GitHub Actions Example

```yaml
name: Build
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run typecheck
      - run: npm run build
```

---

## Dependency Notes

- All packages are in `devDependencies` — this is correct for a Vite-built static app. Production output is a standalone `dist/` folder with no runtime Node.js dependency.
- If you see peer dependency warnings during `npm install`, add `--legacy-peer-deps`:

```bash
npm install --legacy-peer-deps
```

---

## Node Version Management

We recommend using **nvm** or **fnm** to manage Node versions:

```bash
# Using nvm
nvm install 20
nvm use 20

# Using fnm
fnm install 20
fnm use 20
```

---

## Offline Installation

If you need to install without internet access:

1. On a machine with internet, run: `npm install` (generates `node_modules/`)
2. Pack the folder: `tar -czf nexora-with-modules.tar.gz nexora-ai/`
3. Transfer and extract on the target machine
4. No further `npm install` needed — run `npm run build` directly

---

## Verifying the Build

After installation, confirm a clean production build:

```bash
npm run build
```

Expected output:
```
vite build
✓ built in Xs
dist/index.html
dist/assets/index-[hash].js
dist/assets/index-[hash].css
```

The `dist/` folder is your deployable artifact.
