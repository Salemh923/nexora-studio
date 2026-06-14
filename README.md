# Nexora AI — Professional UI Kit & Template

> A production-ready React + Vite UI template featuring multiple landing pages, a dashboard, authentication flows, and a full component library powered by shadcn/ui, Tailwind CSS v4, and Framer Motion.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Development](#development)
- [Build Commands](#build-commands)
- [Deployment](#deployment)
- [Folder Structure](#folder-structure)
- [Environment Variables](#environment-variables)
- [Troubleshooting](#troubleshooting)

---

## Overview

Nexora AI is a high-quality, marketplace-ready React template built for SaaS products, AI tools, and modern web applications. It ships with three distinct landing page variants, a full dashboard overview, auth screens, a pricing page, and an interactive component showcase — all fully responsive and animated out of the box.

---

## Features

- **3 Landing Page Variants** — Home V1, V2, V3 with distinct visual directions
- **Dashboard Overview** — Analytics-style layout with charts and data widgets
- **Authentication Pages** — Sign In, Sign Up, Forgot Password screens
- **Pricing Page** — Feature comparison and plan selection
- **Coming Soon Page** — Maintenance / launch holding page
- **Interactive Showcase** — Live browser-mockup preview of all pages
- **50+ UI Components** — Full shadcn/ui component library pre-installed
- **Framer Motion Animations** — Smooth page transitions and micro-interactions
- **Tailwind CSS v4** — Latest utility-first styling engine
- **TypeScript** — Fully typed codebase
- **React 19 + Vite 7** — Cutting-edge build tooling
- **Dark Mode Ready** — via `next-themes`
- **React Hook Form + Zod** — Form validation out of the box
- **Recharts** — Data visualization components included

---

## Installation

### Prerequisites

| Tool    | Version  |
|---------|----------|
| Node.js | ≥ 20.x   |
| npm     | ≥ 10.x   |

### Steps

```bash
# 1. Unzip the package
unzip nexora-ai.zip -d nexora-ai
cd nexora-ai

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## Development

```bash
# Start dev server with hot reload
npm run dev

# Type-check the project
npm run typecheck

# Preview production build locally
npm run serve
```

The dev server starts on port **5173** by default (overridable via `PORT` env var).

---

## Build Commands

| Command              | Description                              |
|----------------------|------------------------------------------|
| `npm run dev`        | Start development server (HMR enabled)  |
| `npm run build`      | Compile & bundle for production          |
| `npm run serve`      | Preview the production build locally    |
| `npm run typecheck`  | Run TypeScript type checking (no emit)  |

Production output is placed in the `dist/` folder.

---

## Deployment

### Static Hosting (Recommended)

Nexora builds to a fully static site — no server required. Deploy the `dist/` folder to any static host:

**Vercel:**
```bash
npm install -g vercel
npm run build
vercel deploy dist/
```

**Netlify:**
```bash
npm run build
# Drag and drop dist/ to Netlify dashboard, or use Netlify CLI:
netlify deploy --prod --dir dist
```

**GitHub Pages / Any CDN:**
```bash
npm run build
# Upload contents of dist/ to your CDN or static bucket
```

### Custom Base Path

If deploying to a sub-path (e.g. `https://example.com/nexora/`), set the `BASE_PATH` env var:

```bash
BASE_PATH=/nexora/ npm run build
```

### Custom Port

```bash
PORT=8080 npm run dev
```

---

## Folder Structure

```
nexora-ai/
├── index.html                  # HTML entry point
├── package.json                # Project manifest & scripts
├── package-lock.json           # Exact dependency lock file
├── tsconfig.json               # TypeScript configuration
├── vite.config.ts              # Vite bundler configuration
│
├── src/
│   ├── main.tsx                # React app bootstrap
│   ├── App.tsx                 # Root component & router
│   ├── index.css               # Global styles & Tailwind imports
│   │
│   ├── pages/
│   │   ├── showcase.tsx        # Interactive page showcase (index)
│   │   └── not-found.tsx       # 404 page
│   │
│   ├── components/
│   │   ├── site.tsx            # All page components (Home, Pricing, Dashboard, Auth…)
│   │   └── ui/                 # 50+ shadcn/ui base components
│   │       ├── button.tsx
│   │       ├── card.tsx
│   │       ├── dialog.tsx
│   │       └── …               # (accordion, avatar, chart, form, table, etc.)
│   │
│   ├── hooks/
│   │   ├── use-mobile.tsx      # Responsive breakpoint hook
│   │   └── use-toast.ts        # Toast notification hook
│   │
│   └── lib/
│       └── utils.ts            # Shared utility functions (cn, etc.)
│
└── docs/                       # Documentation package
    ├── getting-started.md
    ├── installation-guide.md
    ├── deployment-guide.md
    ├── customization-guide.md
    └── faq.md
```

---

## Environment Variables

All variables are optional. Set them before running dev or build commands.

| Variable    | Default  | Description                                         |
|-------------|----------|-----------------------------------------------------|
| `PORT`      | `5173`   | Port for the dev/preview server                     |
| `BASE_PATH` | `/`      | URL base path for deployment to a sub-directory     |

No `.env` file is required for local development. Create one if needed:

```bash
# .env (optional)
PORT=3000
BASE_PATH=/
```

---

## Troubleshooting

### `npm install` fails with peer dependency errors

```bash
npm install --legacy-peer-deps
```

### Port already in use

```bash
PORT=3001 npm run dev
```

### TypeScript errors after editing

```bash
npm run typecheck
```

Fix all reported type errors before running a production build.

### Blank page after deployment

Ensure your host is configured to serve `index.html` for all routes (SPA fallback). In Netlify, add a `_redirects` file:
```
/* /index.html 200
```
In Vercel, `vercel.json`:
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

### Tailwind styles not applying

Make sure you are running `npm run dev` or `npm run build` — Tailwind v4 is processed entirely through the Vite plugin and requires the build step.

### `vite: command not found` in CI

Run `npm install` before `npm run build` in your CI pipeline. All tools are in `devDependencies`.

---

## License

This template is licensed for use under the terms provided at purchase. Redistribution or resale of the source code is not permitted.

---

*Built with React 19, Vite 7, Tailwind CSS v4, shadcn/ui, and Framer Motion.*
