# techsulting.de – Website Modernisierung: Anforderungen & Architektur

## Context

Haluk möchte seine bestehende CMS-basierte Website techsulting.de durch eine moderne, selbst gehostete Lösung ersetzen. Ziel ist maximale Kontrolle über Content und Technik, kein Abo-CMS, volle SEO-Optimierung, mehrsprachiger Blog (DE/EN), Buchungs- und Payment-Integration sowie ein KI-Chatbot als "digitales Ich". Der Stack basiert auf bekannten Vorlieben (DaisyUI 5, Tailwind CSS v4) und nutzt bewährte Patterns aus dem Derko-Projekt.

---

## 1. Tech Stack (Empfehlung)

| Bereich | Technologie | Begründung |
|---------|-------------|------------|
| Framework | **Astro 5.x** | Static-first, built-in i18n, Content Collections für .md Blog, Zero-JS by default, Netlify-kompatibel |
| Sprache | TypeScript 5.x | Typsicherheit, konsistent mit anderen Projekten |
| CSS | Tailwind CSS v4 + DaisyUI 5 | User-Präferenz, bewährt in Derko & contract-vision |
| Hosting | **Netlify** | Kostenlos, Serverless Functions für API Routes, Edge CDN, Git-Deploy |
| Mailing | Mailjet API | Kostenloses Tier (15k/Monat), bereits in Derko verwendet |
| Analytics | Plausible | DSGVO-konform, kein Cookie-Banner nötig |

**Kein Python. Kein React-Only-Framework. Kein schweres CMS-Abo.**

---

## 2. Seiten-Struktur (SEO: jede Seite = eigene URL)

```
/de/                          → Hero + Kurzvorstellung
/de/beratung/                 → IT & Unternehmensberatung
/de/ki-schulungen/            → KI-Schulungen & Workshops (mit Cal.com Buchung)
/de/ki-einfuehrung/           → KI-Einführung in Unternehmen
/de/digitale-produkte/        → Digitale Produkte (Stripe Checkout)
/de/blog/                     → Blog-Listing
/de/blog/[slug]/              → Blog-Post Detail
/de/kontakt/                  → Kontaktformular
/de/admin/login               → Admin-Login (nur für Haluk)
/de/admin/blog-editor         → Blog-Editor (geschützt per JWT)
/de/impressum/                → Impressum
/de/datenschutz/              → Datenschutz
/de/agb/                      → AGB
(identisch für /en/*)
```

Alle Seiten in `sitemap.xml` (außer `/admin/*`, `/api/*`, Draft-Posts).

---

## 3. Blog-System (Markdown-basiert)

### Frontmatter-Schema

```markdown
---
title: "KI-Einführung für mittelständische Unternehmen"
description: "Wie Sie KI strategisch in Ihre Prozesse integrieren."
date: 2025-03-15
lang: de                        # de | en
author: Haluk
tags: ["KI", "Unternehmen", "Strategie"]
translationKey: "ai-intro-sme"  # Verknüpft DE/EN-Versionen
autoTranslated: false
ogImage: "/images/blog/ki-intro-og.jpg"
featured: false
draft: false
---
```

### Workflow

- **Upload via FTP/SSH**: `.md`-Datei in `src/content/blog/de/` → Git-Push → Netlify-Deploy
- **Upload via Admin-UI**: Login → Markdown-Editor (EasyMDE) → Publish-Button → Deploy Hook
- **Auto-Translation**: Script `scripts/translate-blog.ts` nutzt DeepL Free API (500k Zeichen/Monat), klont fehlende Sprachversion, setzt `autoTranslated: true`

---

## 4. Drittanbieter-Services

| Funktion | Service | Preis | Grund |
|----------|---------|-------|-------|
| Meeting-Buchung (Teams) | **Cal.com Cloud Free** | Kostenlos (1 User) | Open Source, DSGVO-konform, Embed-Widget |
| Übersetzung | **DeepL Free API** | Kostenlos (500k Zeichen/Monat) | Beste DE↔EN Qualität |
| Payment | **Stripe Checkout** | 0% Plattformgebühr | Für Schulungen + Digitale Produkte |
| Digitale Produkte | Stripe + Signed Download-Links | 0% Plattformgebühr | Maximale Kontrolle |
| E-Mail | **Mailjet Free** | Kostenlos (15k/Monat) | Bereits in Derko verwendet |
| Analytics | **Plausible** | Kostenlos (Self-host) oder $9/Monat | Kein Cookie-Banner nötig |

**Externe Plattformen (optional für Reichweite):**
- Schulungen zusätzlich auf **Udemy / LinkedIn Learning** anbieten (Reichweite) → Astro-Seite als SEO-Hub mit Links
- Digitale Produkte zusätzlich auf **Gumroad** (10% Gebühr) → für Discovery, dann auf Stripe migrieren

---

## 5. KI-Chatbot ("Digitales Ich")

### Architektur

```
User Input
  → Input Validation (max 500 Zeichen, min 2 Zeichen)
  → Injection Detection (Regex-Patterns: "ignore instructions", "jailbreak", etc.)
  → Rate Limiting (10 Nachrichten/Minute/IP)
  → LLM API Call (GPT-4o-mini, temp=0.3, max_tokens=500)
  → Output Filter (PII-Entfernung, max 2000 Zeichen)
  → Response an User
```

### Sicherheitsmaßnahmen (OWASP LLM Top 10)

- **LLM01 Prompt Injection**: System Prompt mit Hard Rules + Injection-Pattern-Detection
- **LLM02 Insecure Output**: Output-Filter entfernt E-Mail/Telefon, truncated auf 2000 Zeichen
- **LLM04 Denial of Service**: Rate Limiting (10 req/min/IP), max Session-Messages: 20
- **System Prompt**: Never exposed, structured knowledge base als separater Block
- **Kein Memory**: Keine Chat-Historien werden gespeichert
- Umfangreiche weitere Maßnahmen gemäß `LLMAll_en-US_FINAL.pdf` (liegt unter `.github/instructions/`)

**LLM-Wahl**: **Langdock API** (OpenAI-kompatibler Endpoint → ermöglicht Modellwechsel ohne Code-Änderung: GPT-4o, Claude, Gemini, etc.)
- Integration: `LANGDOCK_API_BASE_URL` + `LANGDOCK_API_KEY` als Env Vars
- Code identisch zu OpenAI SDK (`openai` npm package mit `baseURL` override)

---

## 6. Kontaktformular (Derko-Pattern → TypeScript-Port)

Identische Sicherheitsschicht wie Derko, portiert nach TypeScript:

- CSRF Token (Crypto + HttpOnly Cookie)
- Honeypot-Feld (muss leer bleiben)
- Timestamp-Check (< 2 Sekunden = Bot)
- Rate Limiting (1 Anfrage / 60s / IP)
- Input Sanitization (Strip HTML, Whitelisting, Max-Lengths)
- Mailjet API (statt PHP `mail()`)
- Toast Notifications (DaisyUI 5 `alert`)
- AJAX Submit (kein Page Reload)

**Referenz-Implementierung**: `c:\zdev\02 Produkte\derko\` (CSRF, Honeypot, Rate Limiting, Mailjet)

---

## 7. Admin-Bereich (Blog-Verwaltung)

Single-User-Authentifizierung ohne Datenbank:

```
POST /api/admin-auth  → Passwort vs. ADMIN_PASSWORD_HASH (bcrypt, Env Var)
                      → JWT (HS256, 8h, HttpOnly Secure SameSite=Strict Cookie)
Middleware: prüft JWT bei allen /admin/* Routen
```

Kein NextAuth, kein Supabase. 30-Zeilen-Middleware reicht.

---

## 8. UI & Design

- **Header**: Fixed (bleibt oben beim Scrollen), mit Language Switcher (DE/EN)
- **Hero**: Scroll-Animationen via Intersection Observer + CSS (kein GSAP, kein Framework)
- **Footer**: Social Media (Twitter/X, LinkedIn, Xing), Legal Links (Impressum, Datenschutz, AGB, Kontakt)
- **Cookie Banner**: DaisyUI 5 `alert`, localStorage, Kategorien: Essential / Analytics
- **Farben**: Phase 1 startet mit einem DaisyUI Built-in Theme als Platzhalter (z.B. `corporate`). Wenn Brand Colors geliefert werden, wird **ausschließlich `global.css`** angepasst (CSS Custom Properties) – kein einziges Component muss angefasst werden. Theme-Wechsel = 1 Datei ändern.
- **Bilder**: `<Image>` Component mit AVIF/WebP (wie Derko)
- **Fonts**: Self-hosted via `@fontsource` (DSGVO-konform, kein Google Fonts)

---

## 9. SEO

- `<BaseLayout>`: `<title>`, `<meta description>`, Canonical, hreflang (de/en/x-default), OG, Twitter Card
- JSON-LD: `Person` + `ProfessionalService` (Homepage), `Course` (Schulungen), `BlogPosting` (Blog)
- `@astrojs/sitemap`: Auto-generiert alle Seiten beim Build (excl. Admin, API, Drafts)
- RSS Feed: `/de/rss.xml`, `/en/rss.xml`
- Core Web Vitals: Zero-JS by default, Astro Static HTML, Self-hosted Fonts

---

## 10. Projektstruktur (Schlüsseldateien)

```
techsulting/
├── src/
│   ├── content/blog/de/        # DE Blog-Posts (.md)
│   ├── content/blog/en/        # EN Blog-Posts (.md)
│   ├── content/config.ts       # Zod-Schema für Frontmatter
│   ├── i18n/de.ts + en.ts      # UI-Übersetzungen
│   ├── pages/de/ + en/         # Alle Seiten
│   ├── pages/api/              # Serverless: contact.ts, chatbot.ts, admin-auth.ts
│   ├── components/             # Header, Footer, CookieBanner, ChatbotWidget, ContactForm
│   ├── layouts/BaseLayout.astro
│   ├── lib/                    # csrf.ts, rate-limiter.ts, mailjet.ts, chatbot-security.ts, auth.ts
│   └── styles/global.css       # Tailwind v4 + DaisyUI Custom Theme
├── scripts/translate-blog.ts   # DeepL Auto-Translation CLI
├── astro.config.mjs
└── netlify.toml
```

---

## 11. Phasenplan

| Phase | Inhalt | Deliverable |
|-------|--------|-------------|
| **1** | Astro-Setup, Routing, Header/Footer, Cookie Banner, Netlify Deploy | Leere Site live auf techsulting.de |
| **2** | Alle Content-Seiten (Hero, Beratung, Schulungen, Produkte, Legal) | Vollständige Marketing-Site |
| **3** | Blog-System, DE/EN, Auto-Translation, RSS | Blog live mit ersten Posts |
| **4** | Kontaktformular (Derko-Port), Admin-Login, Blog-Editor | Lead-Generierung + Self-Service Blog |
| **5** | KI-Chatbot mit Security Layer | Chatbot live |
| **6** | Performance, Accessibility, SEO-Audit, Launch | Launch |

---

## 12. Entscheidungen (getroffen)

| Thema | Entscheidung |
|-------|-------------|
| Hosting | Netlify |
| Chatbot LLM | Langdock API (OpenAI-kompatibler Endpoint, Modell frei wählbar) |
| Meeting-Buchung | Cal.com Free |
| Content | Haluk liefert Texte aus bestehendem CMS |

## 13. Noch offene Punkte

1. **Brand Colors**: Haluk liefert Farben für DaisyUI Custom Theme (Phase 1 startet mit Platzhalter-Theme)
2. **Stripe-Account**: Bereits vorhanden oder muss neu eingerichtet werden?
3. **Repo-Name**: Neues Git-Repo in `c:\zdev\02 Produkte\techsulting\`?
4. **Domain-Umzug**: techsulting.de DNS auf Netlify umzeigen – Zeitpunkt festlegen (nach Launch-Bereitschaft)

---

## 14. Content-Quellen (Texte)

Alle Website-Texte liegen bereits als `.md`-Dateien vor:

```
c:\zdev\01 Tests\copilot-orchestra\texte\
├── index.md                        → Hero / Startseite
├── ueber-uns.md                    → Über uns
├── referenzen.md                   → Referenzen & Projekterfahrung
├── kontakt.md                      → Kontaktseite
├── leistungen/
│   ├── it-consulting.md            → Beratungsdienstleistungen
│   ├── ki-beratung.md              → KI-Einführung in Unternehmen
│   ├── schulungen.md               → KI-Schulungen
│   ├── websites.md                 → (ggf. entfernen / anpassen)
│   └── methoden.md                 → (ggf. in Beratung integrieren)
├── digitale-produkte/
│   ├── index.md                    → Digitale Produkte Übersicht
│   └── prompts.md                  → Produktbeschreibung Prompts
└── blog/
    ├── index.md                    → Blog-Intro
    └── 2026-03-beispiel-prompt-engineering.md → Erster Blog-Post
```

**Referenzen-Seite**: Die bestehende `referenzen.md` enthält nur eine Projekttabelle (Kunde, Branche, Rolle). **Haluk ergänzt zu jedem Projekt eine kurze Beschreibung** (1–3 Sätze: Was war die Aufgabe? Was wurde erreicht?). Diese Beschreibungen werden dann als aufklappbare Karten (DaisyUI `collapse` oder `card`) auf der Referenzen-Seite dargestellt.

---

## 15. Bilder-Konzept

### Lizenzierte Bilder (von Haluk bereitgestellt)
Platzhalter werden überall eingebaut. Wenn die Bilder geliefert werden → nur Bildpfad ersetzen, kein Layout-Eingriff.

| Seite | Anzahl Bilder | Verwendung |
|-------|--------------|------------|
| Hero | 1 | Großes Hintergrundbild oder Split-Layout (Text links, Bild rechts) |
| Beratung | 2 | Je 1 Bild pro Leistungsblock (Atmosphäre / Arbeitskontext) |
| KI-Schulungen | 2 | 1 Header-Bild, 1 Bild im Content (Workshop-Atmosphäre) |
| KI-Einführung | 2 | 1 Header-Bild, 1 Bild im Content (Team / Technologie) |
| Digitale Produkte | 1 pro Produkt | Produkt-Preview / Mockup |
| Über uns | 1 | Portrait-Foto Haluk |
| Kontakt | 1 | Kleines Bild (optional, Büro/Person) |
| Blog-Posts | 1 pro Post | OG/Header-Bild (in Frontmatter: `ogImage`) |

### Referenz-Projekte (Bilder optional)
Für die Referenzen-Karten: Entweder **Branchen-Icons** (SVG, kein Bild nötig) oder optionales Projekt-Bild/Logo. Haluk entscheidet beim Befüllen der Projektkarten.

### Platzhalter-Strategie
Alle Bildplätze werden mit einem **strukturierten Platzhalter-System** implementiert:
- Lokaler Pfad: `/public/images/[seite]/[name].[ext]`
- Platzhalter: Graue Box mit Dimensions-Label (z.B. `1200×630`) – kein externer Platzhalter-Service
- Astro `<Image>` Component: AVIF/WebP automatisch, `loading="lazy"` außer Hero (dort `eager`)

---

## Kritische Referenz-Dateien

- `c:\zdev\01 Tests\copilot-orchestra\texte\` – Alle Website-Texte als .md
- `c:\zdev\02 Produkte\derko\` – Kontaktformular-Pattern (CSRF, Honeypot, Mailjet)
- `c:\zdev\01 Tests\copilot-orchestra\.github\instructions\LLMAll_en-US_FINAL.pdf` – Chatbot Security Guide
- `c:\zdev\01 Tests\copilot-orchestra\.github\instructions\daisyui.instructions.md` – DaisyUI 5 Anleitung
- `c:\zdev\05 Tools\knowledge-hub\sessions\` – Bisherige Projekt-Learnings
