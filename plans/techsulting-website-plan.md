# Plan: techsulting.de Website (Astro 5.x)

Vollständige Neuentwicklung von techsulting.de als Astro 5.x Static Site mit TypeScript, Tailwind CSS v4, DaisyUI 5 und Netlify Hosting. Das Projekt wird unter `c:\zdev\02 Produkte\techsulting\` erstellt. Alle 15 Content-Texte aus `texte/` werden direkt eingebaut. EN-Seiten werden maschinell via DeepL übersetzt. Tests werden als Playwright E2E geschrieben.

## Entscheidungen (bestätigt)
| Frage | Entscheidung |
|-------|-------------|
| Projektpfad | `c:\zdev\02 Produkte\techsulting\` |
| websites.md + methoden.md | Einbauen als Leistungs-Unterseiten |
| EN-Übersetzungen | Maschinell via DeepL Free API |
| Tests | Playwright E2E |

---

## Phase 1: Projektfundament & Deploy-Pipeline
- **Objective:** Astro 5.x Projekt initialisieren mit vollständiger Grundstruktur, Routing-Konfiguration (DE/EN i18n), Header, Footer, Cookie-Banner, globalem DaisyUI `corporate` Theme und Netlify-Deployment.
- **Files/Functions to Create:**
  - `astro.config.mjs` – i18n, sitemap, Netlify Adapter
  - `netlify.toml` – Deploy-Konfiguration, Serverless Functions
  - `src/styles/global.css` – Tailwind v4 + DaisyUI 5 `corporate` Theme
  - `src/layouts/BaseLayout.astro` – SEOHead, Header, Footer per Slot
  - `src/components/layout/Header.astro` – Fixed Navbar, DaisyUI navbar, Language Switcher
  - `src/components/layout/Footer.astro` – Social Links (LinkedIn, XING, Twitter/X), Legal Links
  - `src/components/layout/CookieBanner.astro` – DaisyUI alert, localStorage, Essential/Analytics
  - `src/components/shared/LanguageSwitcher.astro` – DE/EN Umschaltung
  - `src/i18n/de.ts`, `src/i18n/en.ts`, `src/i18n/utils.ts` – UI-Strings, t()-Funktion
  - `src/pages/de/index.astro` + `src/pages/en/index.astro` – Platzhalter-Pages
  - `.env.example`, `tsconfig.json`, `package.json`
- **Tests to Write:**
  - `tests/layout.spec.ts`:
    - Header rendert auf allen Seiten
    - Footer enthält Legal Links (Impressum, Datenschutz, AGB)
    - Language Switcher wechselt URL von /de/ zu /en/
    - Cookie-Banner erscheint bei erstem Besuch
    - Cookie-Banner verschwindet nach Klick auf "Akzeptieren"
- **Steps:**
  1. Astro 5.x Projekt in `c:\zdev\02 Produkte\techsulting\` erstellen (TypeScript, strict, output=hybrid, Netlify Adapter)
  2. Dependencies installieren: `daisyui@latest`, `@tailwindcss/vite`, `tailwindcss`, `@astrojs/netlify`, `@astrojs/sitemap`
  3. `global.css` mit `@import "tailwindcss"; @plugin "daisyui" { themes: corporate --default; }` konfigurieren
  4. `astro.config.mjs` mit i18n (defaultLocale: "de", locales: ["de", "en"]), sitemap, Netlify Adapter
  5. `netlify.toml` mit Build-Command, Publish-Dir, Functions-Dir anlegen
  6. Playwright installieren, Tests schreiben, ausführen → erwartet FAIL
  7. BaseLayout, Header, Footer, CookieBanner, LanguageSwitcher implementieren
  8. i18n Hilfsfunktionen (de.ts, en.ts, utils.ts)
  9. Tests erneut ausführen → erwartet PASS

---

## Phase 2: Alle Content-Seiten (DE + EN)
- **Objective:** Alle 12 Marketing-Seiten mit Content aus `texte/` aufbauen: Homepage Hero, Über uns, Leistungen (6 Unterseiten inkl. websites.md und methoden.md), Digitale Produkte (2 Seiten), Referenzen (mit DaisyUI collapse-Karten), Kontakt-Platzhalter, Legal-Seiten.
- **Files/Functions to Create:**
  - `src/layouts/LegalLayout.astro` – schlichte Prosa-Seiten
  - `src/components/shared/SEOHead.astro` – Title, Description, Canonical, hreflang, OG, Twitter Card, JSON-LD
  - `src/components/shared/PlaceholderImage.astro` – Graue Box mit Dimensions-Label
  - `src/pages/de/{index,ueber-uns,referenzen}.astro`
  - `src/pages/de/leistungen/{index,it-consulting,ki-beratung,schulungen,websites,methoden}.astro`
  - `src/pages/de/digitale-produkte/{index,prompts}.astro`
  - `src/pages/de/{impressum,datenschutz,agb}.astro`
  - Identische EN-Seiten unter `src/pages/en/` (maschinell übersetzt)
  - JSON-LD: Person+ProfessionalService (Homepage), Course (Schulungen), Product (Digitale Produkte)
- **Tests to Write:**
  - `tests/seo.spec.ts`:
    - Alle Seiten haben `<title>` und Meta Description
    - Canonical URL vorhanden
    - hreflang Attribute (de, en, x-default) auf allen Seiten
    - JSON-LD vorhanden auf Homepage, Schulungen, Digitale Produkte
  - `tests/content.spec.ts`:
    - Homepage enthält "TechSulting"
    - Referenzen-Seite enthält aufklappbare Karten
    - Legal-Seiten erreichbar und nicht leer
- **Steps:**
  1. Tests schreiben → FAIL
  2. SEOHead-Komponente mit JSON-LD implementieren
  3. Alle DE-Seiten mit Content aus `texte/` aufbauen (DaisyUI hero, card, collapse für Referenzen)
  4. EN-Seiten als maschinelle Übersetzung erstellen
  5. LegalLayout für Impressum/Datenschutz/AGB
  6. PlaceholderImage-Komponente (grauer Block mit Dimensions-Text)
  7. Tests ausführen → PASS

---

## Phase 3: Blog-System (Markdown, i18n, RSS)
- **Objective:** Blog-System mit Astro Content Collections: DE/EN Blog-Posts, Listing-Page, Detail-Page mit TOC, Auto-Translation via DeepL CLI, RSS Feed.
- **Files/Functions to Create:**
  - `src/content/config.ts` – Zod-Schema (title, description, date, lang, author, tags, translationKey, autoTranslated, ogImage, featured, draft)
  - `src/content/blog/de/2026-03-beispiel-prompt-engineering.md` – migriert aus `texte/blog/`
  - `src/content/blog/en/` – Auto-übersetzt
  - `src/layouts/BlogLayout.astro` – extends BaseLayout, TOC, Prev/Next Navigation
  - `src/components/blog/BlogCard.astro` – DaisyUI card
  - `src/pages/de/blog/index.astro` – Listing mit Tag-Filter
  - `src/pages/de/blog/[slug].astro` – Dynamic Route
  - `src/pages/en/blog/index.astro` + `en/blog/[slug].astro`
  - `src/pages/de/rss.xml.ts` + `src/pages/en/rss.xml.ts`
  - `scripts/translate-blog.ts` – DeepL Free API CLI
- **Tests to Write:**
  - `tests/blog.spec.ts`:
    - Blog-Listing-Page zeigt Posts
    - Detail-Page rendert Markdown-Inhalt
    - RSS Feed ist valides XML
    - hreflang auf Blog-Posts verlinkt DE↔EN-Version
    - Draft-Posts nicht in sitemap.xml
- **Steps:**
  1. Tests schreiben → FAIL
  2. `config.ts` mit Zod-Schema für Frontmatter
  3. Blog-Post aus `texte/blog/` in Content Collections Format migrieren
  4. BlogCard, BlogLayout (mit TOC), Listing-Page, Dynamic Route
  5. RSS Feed implementieren
  6. `scripts/translate-blog.ts` mit DeepL API
  7. Tests ausführen → PASS

---

## Phase 4: Kontaktformular, Admin-Login, Blog-Editor
- **Objective:** Sicheres Kontaktformular (Derko-Pattern als TypeScript-Port), Single-User Admin-Login via JWT, geschützter Blog-Editor mit EasyMDE, Netlify Deploy Hook für Publish-Workflow.
- **Files/Functions to Create:**
  - `src/lib/security/csrf.ts` – generateToken, verifyToken (Web Crypto API, HttpOnly Cookie)
  - `src/lib/security/rate-limiter.ts` – In-Memory Map (IP/Timestamp-basiert)
  - `src/lib/security/input-sanitizer.ts` – stripHTML, whitelistChars, maxLength
  - `src/lib/services/mailjet.ts` – REST API Client
  - `src/lib/services/auth.ts` – bcrypt verify, JWT sign/verify (HS256, 8h)
  - `src/pages/api/contact.ts` – POST: CSRF + Honeypot + Timestamp-Check + Rate Limit + Mailjet
  - `src/pages/api/admin-auth.ts` – POST: bcrypt + JWT, GET: verify
  - `src/pages/api/blog-publish.ts` – POST: JWT check + Netlify Deploy Hook
  - `src/components/forms/ContactForm.astro` – DaisyUI floating-label, Honeypot, Toast (alert)
  - `src/pages/de/kontakt.astro` – mit ContactForm Component
  - `src/pages/de/admin/login.astro` – Login-Seite
  - `src/pages/de/admin/blog-editor.astro` – EasyMDE Editor, Publish-Button
  - `src/middleware.ts` – JWT-Prüfung für /admin/* Routen
- **Tests to Write:**
  - `tests/contact-form.spec.ts`:
    - Formular sendet erfolgreich (Mock Mailjet)
    - Honeypot-Feld gefüllt → 400 rejected
    - CSRF-Token fehlt → 403 rejected
    - Rate Limit (2 schnelle Anfragen) → 429
    - XSS-Payload wird sanitized
  - `tests/admin.spec.ts`:
    - Login mit falsem Passwort → Fehlermeldung
    - Login mit richtigem Passwort → JWT Cookie gesetzt
    - GET /admin/* ohne Cookie → Redirect zu /de/admin/login
- **Steps:**
  1. Tests schreiben → FAIL
  2. Security-Layer implementieren (csrf.ts, rate-limiter.ts, input-sanitizer.ts)
  3. Services implementieren (mailjet.ts, auth.ts)
  4. API-Endpoints (contact.ts, admin-auth.ts, blog-publish.ts)
  5. ContactForm-Komponente mit Toast-Feedback (DaisyUI alert)
  6. Admin-Middleware und geschützte Routen
  7. Blog-Editor mit EasyMDE
  8. Tests ausführen → PASS

---

## Phase 5: KI-Chatbot mit Security Layer
- **Objective:** KI-Chatbot Widget (Fixed Bottom-Right) mit vollständiger OWASP LLM Top 10 Sicherheitsschicht: Prompt-Injection-Detection, Input-Validation, Rate Limiting, Output-Filter PII-Removal, Langdock API (OpenAI-kompatibler Endpoint).
- **Files/Functions to Create:**
  - `src/lib/security/chatbot-security.ts` – InjectionDetector (Regex Patterns), InputValidator (min 2 / max 500 Zeichen), OutputFilter (PII-Removal E-Mail/Telefon, max 2000 Zeichen), Session Counter (max 20 Messages)
  - `src/lib/services/langdock.ts` – OpenAI SDK mit baseURL override, System Prompt (Hard Rules + Knowledge Base)
  - `src/pages/api/chatbot.ts` – POST: Rate Limit 10/min/IP + Security Layer + Langdock
  - `src/components/chat/ChatbotWidget.astro` – Fixed Bottom-Right, DaisyUI card + chat bubbles, kein externesFramework
- **Tests to Write:**
  - `tests/chatbot.spec.ts`:
    - Widget ist auf allen Seiten sichtbar
    - "ignore instructions" → 400 (Injection Detection)
    - Leere Nachricht → 400
    - Nachricht > 500 Zeichen → 400
    - 11. Anfrage in 1 Minute → 429
    - Response enthält keine E-Mail-Adressen (PII-Filter)
- **Steps:**
  1. Tests schreiben → FAIL
  2. `chatbot-security.ts` implementieren (alle Security-Layer)
  3. `langdock.ts` mit System Prompt (Hard Rules + Knowledge Base)
  4. `chatbot.ts` API-Endpoint
  5. `ChatbotWidget.astro` mit DaisyUI-Komponenten
  6. Tests ausführen → PASS

---

## Phase 6: Performance, SEO-Audit & Launch-Vorbereitung
- **Objective:** Core Web Vitals optimieren, robots.txt finalisieren, Plausible Analytics Integration, sitemap.xml validieren, Security Headers in netlify.toml, Lighthouse ≥ 90 auf allen Hauptseiten.
- **Files/Functions to Modify:**
  - `public/robots.txt` – excl. /api/*, /admin/*
  - `netlify.toml` – Security Headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
  - `src/components/shared/SEOHead.astro` – Plausible Script (mit Cookie-Consent-Gate)
  - `astro.config.mjs` – Sitemap-Exclude-Patterns (Admin, API, Drafts)
  - `.env.example` – vollständig dokumentiert
- **Tests to Write:**
  - `tests/seo-audit.spec.ts`:
    - sitemap.xml enthält alle DE/EN-Seiten
    - /admin/* nicht in sitemap
    - robots.txt disallowed /admin/* und /api/*
    - hreflang URLs auflösbar (keine 404)
  - `tests/security-headers.spec.ts`:
    - Alle Seiten senden HSTS Header
    - CSP Header vorhanden
    - X-Frame-Options = DENY
- **Steps:**
  1. Tests schreiben → FAIL
  2. `robots.txt` finalisieren
  3. Netlify Security Headers in `netlify.toml`
  4. Plausible-Script in SEOHead (nur wenn Analytics-Consent gegeben)
  5. Sitemap-Exclude-Konfiguration
  6. Lighthouse-Audit für Hauptseiten, Issues beheben
  7. Tests ausführen → PASS
  8. `.env.example` vollständig dokumentieren

---

## Open Questions (beantwortet)
1. **Projektpfad:** `c:\zdev\02 Produkte\techsulting\` ✅
2. **websites.md + methoden.md:** Einbauen als Leistungs-Unterseiten ✅
3. **EN-Übersetzungen:** Maschinell via DeepL Free API ✅
4. **Tests:** Playwright E2E ✅
