# Frequently Asked Questions (FAQ)

---

## Installation

### Q: `npm install` fails with peer dependency errors

**A:** Run with the legacy peer deps flag:
```bash
npm install --legacy-peer-deps
```

This is safe — some packages in the ecosystem haven't updated their peer dependency declarations for React 19.

---

### Q: Do I need a database or backend server?

**A:** No. Nexora AI is a fully static frontend template. It produces plain HTML/CSS/JS files on build — no server, no database, no backend required.

---

### Q: Which Node.js version should I use?

**A:** Node.js 20 LTS or newer. Node 22 is also supported. Older versions (16, 18) are not recommended as Vite 7 and React 19 require newer module system features.

---

## Development

### Q: Hot reload isn't working

**A:** Make sure you launched the dev server with `npm run dev`, not by opening `index.html` directly in a browser. Vite requires its own development server for HMR to work.

---

### Q: I see TypeScript errors after editing a file

**A:** Run `npm run typecheck` to see all type errors. Fix them before building for production. Most common issues:
- Missing type annotations on new props
- Wrong import path (use `@/` for `src/` paths)

---

### Q: Where do I put static files (images, fonts, etc.)?

**A:** Create a `public/` folder in the project root. Files there are served directly at `/`. For example, `public/logo.png` becomes accessible at `/logo.png` and in components as `<img src="/logo.png" />`.

---

## Build & Deployment

### Q: The site works locally but shows a blank page after deployment

**A:** Your hosting platform needs to serve `index.html` for all routes (SPA routing). Add:

- **Netlify** — create `public/_redirects`:
  ```
  /* /index.html 200
  ```
- **Vercel** — create `vercel.json`:
  ```json
  { "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }] }
  ```
- **nginx** — add `try_files $uri /index.html;` to your location block

---

### Q: How do I deploy to a sub-path like `https://example.com/app/`?

**A:** Set the `BASE_PATH` environment variable before building:
```bash
BASE_PATH=/app/ npm run build
```

---

### Q: Can I use this template with a backend API?

**A:** Yes. Add your API calls using `fetch` or a library like `axios` or `@tanstack/react-query` (already installed). Point your calls to your API server. For production, configure CORS on your API or use a reverse proxy.

---

### Q: How do I preview the production build before deploying?

**A:**
```bash
npm run build
npm run serve
```
This serves the `dist/` folder exactly as it will appear in production.

---

## Customization

### Q: How do I change the color scheme?

**A:** Edit the CSS custom properties in `src/index.css`. See the [Customization Guide](./customization-guide.md#4-modifying-colors--theme) for details.

---

### Q: Can I add more pages?

**A:** Yes — create a new file in `src/pages/`, export a component, and add a `<Route>` in `src/App.tsx`. See the [Customization Guide](./customization-guide.md#3-adding-a-new-page).

---

### Q: Are the UI components editable?

**A:** Yes. All components in `src/components/ui/` are local source files (not locked inside `node_modules`). You can edit any of them freely.

---

## Security

### Q: `npm audit` reports vulnerabilities in esbuild/vite. Is my app unsafe?

**A:** No. The reported vulnerabilities in `esbuild` affect only the **development server** — they are not present in the production build (`dist/`). Your deployed application contains no Node.js code. See [SECURITY.md](../SECURITY.md) for a full explanation.

---

### Q: Can I run `npm audit fix --force`?

**A:** Not recommended without testing. Forcing the fix upgrades Vite to a major version (v8) which may introduce breaking changes. See [SECURITY.md](../SECURITY.md) for recommended actions.
