# Customization Guide

This guide explains how to safely customize Nexora AI's content, branding, and structure without breaking the existing design system.

---

## Overview of Files to Edit

| What you want to change         | File to edit                    |
|---------------------------------|---------------------------------|
| Page content & copy             | `src/components/site.tsx`       |
| App routing                     | `src/App.tsx`                   |
| Global styles & CSS variables   | `src/index.css`                 |
| Showcase page (index)           | `src/pages/showcase.tsx`        |
| Page title / meta               | `index.html`                    |
| Build configuration             | `vite.config.ts`                |

---

## 1. Changing Text Content

All page components live in `src/components/site.tsx`. This single file exports:

- `HomePage` (V1)
- `HomeV2Page` (V2)
- `HomeV3Page` (V3)
- `PricingPage`
- `DashboardPage`
- `AuthPage`
- `NotFoundPage`
- `ComingSoonPage`
- `DynamicRoutePage`

Open `src/components/site.tsx` and search for the text you want to change. Replace it in place.

---

## 2. Changing the Site Title

Edit `index.html`:

```html
<title>Your Product Name</title>
<meta name="description" content="Your product description" />
```

---

## 3. Adding a New Page

**Step 1** — Create your page file, e.g. `src/pages/about.tsx`:

```tsx
export function AboutPage() {
  return (
    <main>
      <h1>About Us</h1>
    </main>
  );
}
```

**Step 2** — Register the route in `src/App.tsx`:

```tsx
import { AboutPage } from "@/pages/about";

// Inside AppRouter's <Switch>:
<Route path="/about"><AboutPage /></Route>
```

**Step 3** — Optionally add it to the Showcase in `src/pages/showcase.tsx` by adding an entry to the pages array.

---

## 4. Modifying Colors & Theme

Colors are defined as CSS custom properties in `src/index.css`. Edit the `:root` block to change the color palette.

Common variables to adjust:
```css
:root {
  --background: ...;
  --foreground: ...;
  --primary: ...;
  --primary-foreground: ...;
  --muted: ...;
  --muted-foreground: ...;
  --border: ...;
  --radius: ...;
}
```

Tailwind CSS v4 uses these CSS variables automatically via the `@theme` block in the same file.

---

## 5. Using UI Components

The `src/components/ui/` folder contains 50+ ready-to-use components from shadcn/ui. Import them by name:

```tsx
import { Button } from "@/components/ui/button";
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card";
import { Dialog, DialogTrigger, DialogContent } from "@/components/ui/dialog";
```

All components accept standard React props and are fully typed.

---

## 6. Adding External Packages

```bash
npm install <package-name>
```

Import and use in any component. For TypeScript packages, add types if needed:
```bash
npm install --save-dev @types/<package-name>
```

---

## 7. Changing the Logo / Brand Assets

Search for your brand name / logo references in `src/components/site.tsx`. Replace with your own `<img>` tags or SVG components. Place image files in the `public/` folder (create it if it doesn't exist) and reference them as `/logo.svg`.

---

## 8. Removing Unused Pages

To remove a page:
1. Delete the route from `src/App.tsx`
2. Remove its export from `src/components/site.tsx` (or delete the page file)
3. Remove its entry from the Showcase array in `src/pages/showcase.tsx`

---

## 9. Renaming the Project

Update `package.json`:
```json
{
  "name": "your-project-name",
  "version": "1.0.0"
}
```

This name is only for npm tooling — it doesn't affect what users see.
