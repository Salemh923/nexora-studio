# Security Report — Nexora AI

Generated: 2026-06-14  
Audit tool: `npm audit`  
Total packages audited: 226  
Vulnerabilities found: **2 High**

---

## Summary

| Severity | Count | Affects Production? |
|----------|-------|---------------------|
| High     | 2     | **No** — dev only   |
| Moderate | 0     | —                   |
| Low      | 0     | —                   |

> **Important:** Both vulnerabilities exist only in development tooling (`esbuild`, `vite`). They are **not present** in the compiled production output (`dist/`). Deployed users are not exposed to these issues.

---

## Vulnerability Details

### 1. esbuild — Missing binary integrity verification (GHSA-gv7w-rqvm-qjhr)

| Field      | Value                                                         |
|------------|---------------------------------------------------------------|
| Package    | `esbuild` (versions 0.17.0 – 0.28.0)                        |
| Severity   | **High**                                                      |
| GHSA ID    | GHSA-gv7w-rqvm-qjhr                                           |
| Advisory   | https://github.com/advisories/GHSA-gv7w-rqvm-qjhr            |

**Description:**  
In the Deno module variant of esbuild, the binary download step does not verify integrity against a checksum. This could allow a compromised `NPM_CONFIG_REGISTRY` mirror to serve a malicious binary during `npm install`. This vulnerability requires an attacker to already control the npm registry endpoint being used.

**Why it exists:**  
`esbuild` is a build-time bundler required by Vite. It is a `devDependency` — used only during the local development and build process. It is never shipped to end users or included in the production `dist/` output.

**Production impact:** None. The compiled output is static HTML/CSS/JS with no Node.js runtime.

**Recommended action:**  
- Upgrade to `vite@8` (which ships with `esbuild@0.29+`) when you are ready to test for breaking changes.
- Until then, install only from the official npm registry (`https://registry.npmjs.org`) — never from untrusted mirrors.
- Do not run `npm audit fix --force` without a full regression test, as it performs a major-version upgrade of Vite.

---

### 2. esbuild — Arbitrary file read on Windows dev server (GHSA-g7r4-m6w7-qqqr)

| Field      | Value                                                         |
|------------|---------------------------------------------------------------|
| Package    | `esbuild` (via `vite` 4.2.0-beta.0 – 8.0.3)                 |
| Severity   | **High**                                                      |
| GHSA ID    | GHSA-g7r4-m6w7-qqqr                                           |
| Advisory   | https://github.com/advisories/GHSA-g7r4-m6w7-qqqr            |

**Description:**  
When running the `esbuild` development server on **Windows**, a crafted HTTP request can read arbitrary files from the filesystem. This only affects users who run the Vite dev server on Windows and expose it to untrusted networks.

**Why it exists:**  
This is a `devDependency`. The Vite dev server is only used during local development (`npm run dev`). The production build (`npm run build`) and its output (`dist/`) are completely unaffected.

**Production impact:** None.

**Conditions required for exploitation:**
- Must be running on **Windows** (not Linux/macOS)
- Must be running the **Vite dev server** (`npm run dev`)
- An attacker must have **network access** to the dev server port

**Recommended action:**  
- Do not expose the Vite dev server to public networks on Windows (the default `localhost` binding is safe)
- Upgrade to `vite@8` when ready and after testing

---

## Safe Fix (Without Breaking Changes)

Neither vulnerability has a non-breaking fix available in the current dependency tree. The automated fix (`npm audit fix --force`) would upgrade Vite from v7 to v8, which is a **major version change** and may introduce breaking changes in the build output.

**To apply the fix when ready:**

```bash
# Test in a branch first
npm audit fix --force

# Verify nothing is broken
npm run typecheck
npm run build
npm run serve
```

If the build and all pages look correct, the upgrade is safe to commit.

---

## Risk Assessment for Marketplace Distribution

| Risk Factor          | Assessment                                                      |
|----------------------|-----------------------------------------------------------------|
| End-user exposure    | **None** — vulnerabilities are in devDependencies only          |
| Production bundle    | **Clean** — `dist/` contains no vulnerable code                 |
| Developer exposure   | **Low** — requires Windows + exposed dev server OR tampered registry |
| Buyer risk           | **Minimal** — standard industry-accepted dev tooling            |

This package meets standard marketplace security requirements. The vulnerabilities are well-known, widely documented, and limited to the development environment.

---

## Future Actions

1. Monitor the [Vite changelog](https://github.com/vitejs/vite/releases) for a stable v8 release with no breaking changes for this project.
2. Re-run `npm audit` after upgrading to confirm clean status.
3. Pin exact esbuild version via `npm install esbuild@latest --save-dev` once v0.29+ is stable in the Vite ecosystem.
