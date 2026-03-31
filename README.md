# DAC 90TEF.OH — Landing Page

Landing page dedicat basculantei electrice **DAC 90TEF.OH**, hosted pe subdomain dac.ro.

## Tech Stack

| Tech | Versiune | Rol |
|------|----------|-----|
| React | 19 | UI framework |
| Next.js | 15 (SSG) | Static site generation |
| Tailwind CSS | 4 | Styling |
| TypeScript | 5.x | Type safety |
| Resend | API | Email notifications |
| GA4 + GTM | — | Analytics & event tracking |
| GitHub Pages | — | Hosting |
| GitHub Actions | — | CI/CD |

## Structura paginii (7 secțiuni)

1. **Hero** — Banner principal cu CTA
2. **About** — Despre DAC și tradiția în producție
3. **Produs** — Specificații tehnice DAC 90TEF.OH
4. **Beneficii** — De ce electric? Avantaje cheie
5. **Target** — Pentru cine este (fleet operators, municipalități, etc.)
6. **Formular** — Contact form cu validare email/telefon
7. **Contact** — Date contact DAC + locație

## Setup local

```bash
# Install dependencies
npm install

# Dev server
npm run dev

# Build static export
npm run build

# Preview build
npm run start
```

## GA4 Configuration

### Container GTM
- **GTM ID:** `GTM-XXXXXXX` (de configurat)
- **GA4 Measurement ID:** `G-XXXXXXXXXX` (de configurat)

### Events tracked (5)

| Event | Trigger | Parametri |
|-------|---------|-----------|
| `page_view` | Page load | `page_title`, `page_location` |
| `cta_primary` | Click pe CTA principal (Hero) | `button_text`, `section` |
| `cta_secondary` | Click pe CTA-uri secundare | `button_text`, `section` |
| `form_start` | Focus pe primul câmp din formular | `form_name` |
| `form_submit` | Submit formular reușit | `form_name`, `submission_id` |

### Implementare

Events sunt trimise via `dataLayer.push()` și procesate de GTM:

```typescript
// utils/analytics.ts
export const trackEvent = (event: string, params?: Record<string, string>) => {
  window.dataLayer?.push({
    event,
    ...params,
  });
};
```

## Email (Resend)

La fiecare form submit, se trimite email via Resend API:
- **To:** adresă configurabilă DAC
- **From:** noreply@subdomain.dac.ro
- **Template:** HTML email cu datele din formular

API key Resend se stochează ca environment variable (`RESEND_API_KEY`).

## Deployment

GitHub Actions workflow:
1. Push pe `main` → trigger build
2. `next build && next export`
3. Deploy `out/` pe GitHub Pages
4. Custom domain: `subdomain.dac.ro` via CNAME

## Custom Domain Setup

1. În repo Settings → Pages → Custom domain: `subdomain.dac.ro`
2. DNS la DAC: CNAME record `subdomain` → `lazart-studios.github.io`
3. Enforce HTTPS ✓

## Structura proiect (planificată)

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── globals.css
├── components/
│   ├── Hero.tsx
│   ├── About.tsx
│   ├── Product.tsx
│   ├── Benefits.tsx
│   ├── Target.tsx
│   ├── ContactForm.tsx
│   └── Contact.tsx
├── utils/
│   ├── analytics.ts
│   └── resend.ts
└── lib/
    └── gtm.ts
```

---

**Ref:** LAZART-2026-DAC-001 · Lazart Studios SRL
