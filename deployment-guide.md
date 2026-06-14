# Deployment Guide

Nexora AI compiles to a fully static site. The `dist/` folder produced by `npm run build` contains plain HTML, CSS, and JavaScript — no server required.

---

## Build for Production

```bash
npm run build
```

Output goes to `dist/`. This folder is what you deploy.

---

## Deployment Platforms

### Vercel (Recommended)

1. Push your project to a GitHub/GitLab/Bitbucket repository.
2. Import the repository at [vercel.com](https://vercel.com).
3. Vercel auto-detects Vite. Settings:
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`
4. Click **Deploy**.

Or use the CLI:
```bash
npm install -g vercel
vercel deploy --prod
```

### Netlify

**Via drag-and-drop:**
1. Run `npm run build`
2. Drag the `dist/` folder to [app.netlify.com/drop](https://app.netlify.com/drop)

**Via CLI:**
```bash
npm install -g netlify-cli
npm run build
netlify deploy --prod --dir dist
```

**SPA routing** — create `dist/_redirects`:
```
/* /index.html 200
```
Or add `netlify.toml` at project root:
```toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### GitHub Pages

```bash
npm install --save-dev gh-pages
npm run build
npx gh-pages -d dist
```

Add to `package.json` scripts:
```json
"deploy": "npm run build && gh-pages -d dist"
```

> **Note:** For GitHub Pages at a sub-path, set `BASE_PATH`:
> ```bash
> BASE_PATH=/your-repo-name/ npm run build
> ```

### AWS S3 + CloudFront

```bash
npm run build
aws s3 sync dist/ s3://your-bucket-name --delete
```

Enable **Static website hosting** on the S3 bucket and configure CloudFront to redirect 403/404 errors to `index.html` for SPA routing.

### Any Static File Server (nginx)

```nginx
server {
  listen 80;
  root /var/www/nexora/dist;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }

  location ~* \.(js|css|png|jpg|svg|ico|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }
}
```

---

## Environment Variables at Build Time

| Variable    | Default | Usage                                             |
|-------------|---------|---------------------------------------------------|
| `PORT`      | `5173`  | Dev/preview server port only — not for production |
| `BASE_PATH` | `/`     | Set when deploying to a URL sub-path              |

### Sub-path Deployment Example

If your site will be at `https://example.com/app/`:

```bash
BASE_PATH=/app/ npm run build
```

---

## Verifying the Production Build Locally

```bash
npm run build
npm run serve
```

Opens a preview server at [http://localhost:5173](http://localhost:5173) serving the production `dist/` build. Use this to confirm everything looks correct before pushing live.

---

## Caching Strategy

For best performance, configure your host to:

- Set `Cache-Control: max-age=31536000, immutable` on `/assets/**` (hashed filenames — safe to cache forever)
- Set `Cache-Control: no-cache` on `index.html` (must always be fresh)
