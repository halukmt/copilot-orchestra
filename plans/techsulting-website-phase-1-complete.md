## Phase 1 Complete: Projektfundament & Deploy-Pipeline

Astro 5.x Projekt unter `c:\zdev\02 Produkte\techsulting\` vollständig aufgesetzt mit DaisyUI 5 Corporate Theme, DE/EN i18n Routing, responsivem Header/Footer, Cookie-Banner und Netlify-Deploy-Konfiguration. Alle 9 Playwright E2E Tests grün, Build erfolgreich.

**Files created:**
- `astro.config.mjs`
- `netlify.toml`
- `playwright.config.ts`
- `tsconfig.json`
- `package.json`
- `.env.example`
- `.gitignore`
- `public/favicon.svg`
- `public/robots.txt`
- `src/styles/global.css`
- `src/i18n/de.ts`
- `src/i18n/en.ts`
- `src/i18n/utils.ts`
- `src/layouts/BaseLayout.astro`
- `src/components/layout/Header.astro`
- `src/components/layout/Footer.astro`
- `src/components/layout/CookieBanner.astro`
- `src/components/shared/LanguageSwitcher.astro`
- `src/pages/index.astro` (Root Redirect → /de/)
- `src/pages/de/index.astro`
- `src/pages/en/index.astro`
- `tests/layout.spec.ts`

**Functions created:**
- `useTranslations(locale)` – i18n Hilfsfunktion
- `getLangFromUrl(url)` – Locale aus URL extrahieren
- `getLocalizedPath(path, locale)` – Pfad für andere Sprache berechnen
- `showBannerIfNeeded()` – Cookie-Banner LocalStorage-Check
- `hideBanner()` – Cookie-Banner ausblenden

**Tests created:**
- Header renders on German homepage
- Header renders on English homepage
- Footer contains legal links in German
- Language Switcher switches from DE to EN
- Language Switcher switches from EN to DE
- Cookie Banner appears on first visit
- Cookie Banner disappears after accepting
- Cookie Banner disappears after essential only
- Cookie Banner does not appear on second visit (consent stored)

**Review Status:** APPROVED – 9/9 Playwright Tests grün, Build erfolgreich (3 pages, 4.18s)

**Git Commit Message:**
```
feat: initialize Astro 5.x project for techsulting.de

- Astro 5.x with TypeScript strict, hybrid output, Netlify adapter
- Tailwind CSS v4 + DaisyUI 5 corporate theme (no tailwind.config.js)
- DE/EN i18n routing with language switcher
- Responsive header with desktop/mobile menu (DaisyUI navbar)
- Footer with social links (LinkedIn, XING, Twitter/X) and legal nav
- Cookie consent banner with localStorage persistence
- Netlify deploy configuration (netlify.toml)
- 9 Playwright E2E tests (all passing)
```
