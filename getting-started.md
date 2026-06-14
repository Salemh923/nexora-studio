# Getting Started with Nexora AI

Welcome to **Nexora AI** — a professional React + Vite template for SaaS and AI products.

This guide will get you from zero to a running local development server in under 5 minutes.

---

## What You Get

| Page / Feature        | Route                  | Description                               |
|-----------------------|------------------------|-------------------------------------------|
| Showcase (Index)      | `/`                    | Interactive preview of all pages          |
| Landing Page V1       | `/home-v1`             | Full landing page, variant 1              |
| Landing Page V2       | `/home-v2`             | Full landing page, variant 2              |
| Landing Page V3       | `/home-v3`             | Full landing page, variant 3              |
| Pricing               | `/pricing`             | Plans & feature comparison                |
| Dashboard Overview    | `/dashboard-overview`  | Analytics-style dashboard layout          |
| Sign In               | `/sign-in`             | Authentication — login                    |
| Sign Up               | `/sign-up`             | Authentication — registration             |
| Forgot Password       | `/forgot-password`     | Authentication — password recovery        |
| Coming Soon           | `/coming-soon`         | Maintenance / pre-launch placeholder      |
| 404                   | `/404`                 | Not found page                            |

---

## System Requirements

| Dependency | Minimum Version |
|------------|----------------|
| Node.js    | 20.x or higher |
| npm        | 10.x or higher |

Check your versions:
```bash
node --version
npm --version
```

---

## Quick Start

```bash
# Step 1 — Extract the package
unzip nexora-ai.zip -d nexora-ai
cd nexora-ai

# Step 2 — Install dependencies
npm install

# Step 3 — Launch dev server
npm run dev
```

Visit [http://localhost:5173](http://localhost:5173) to see the showcase page.

---

## Navigating the App

The root `/` route opens the **Showcase** — an interactive page that renders browser-mockup previews of every page. Click any preview card to navigate to that page directly.

Each page is fully standalone and can be linked to independently by route.

---

## Next Steps

- **Customize content** → See [Customization Guide](./customization-guide.md)
- **Deploy to production** → See [Deployment Guide](./deployment-guide.md)
- **Common problems** → See [FAQ](./faq.md)
