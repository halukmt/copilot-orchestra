---
title: "Projektplan: Relaunch techsulting.de"
version: "1.0"
date: "2026-03"
---

# Projektplan: Relaunch techsulting.de

## 1. Projektziele

- Positionierung als IT-Consulting- und KI-Beratungsmarke mit „Wir"-Auftritt
- Neue Angebotscluster: IT-Consulting, KI-Beratung, Schulungen, Webdesign, Digitale Produkte, Externe KI-Tools
- Integrierter Blog für Reichweite, SEO, Social-Media-Content und Video-Skripte
- Einfacher Content-Workflow in VS Code via Markdown-Dateien
- Maximale SEO-Wirkung durch nativ integrierten Blog auf eigener Domain
- Deployment auf Vercel oder Netlify inkl. CI/CD via GitHub

---

## 2. Technologiestack

| Bereich | Empfehlung | Begründung |
|---|---|---|
| Framework | **Astro** (empfohlen) oder Next.js | Astro: statisch, extrem schnell, Markdown/MDX nativ, ideal für Blog + CMS |
| Styling | **Tailwind CSS** | Utility-first, schnell anpassbar, mobile-first |
| Content | Markdown (.md) / MDX (.mdx) | VS-Code-freundlich, Git-basiert, kein separates CMS nötig |
| Blog | Nativ in Astro/Next.js | Alle Blog-Posts als .md in /content/blog/, volle SEO-Kontrolle |
| Hosting | **Vercel** oder **Netlify** | Kostenloser Tier für Start, CDN, SSL, automatisches Deploy via GitHub |
| Versionskontrolle | **GitHub** | Standard, CI/CD-Integration, kostenlos |
| Digitale Produkte (Checkout) | **Digistore24** oder **Gumroad** | Fertige Zahlungsabwicklung, Download-Auslieferung, MwSt-konform |
| Analytics | **Plausible** (empfohlen) oder Matomo | DSGVO-konform, cookieless, leichtgewichtig |
| Kontaktformular | **Formspree** oder **Netlify Forms** | Einfach, kein Backend nötig |
| E-Mail-Marketing | **Brevo** (ehemals Sendinblue) | DSGVO-konform, kostengünstiger Start, Newsletter für Blog-Abonnenten |

---

## 3. Ordnerstruktur

```
techsulting/
├── public/                     # Statische Assets (Logo, Bilder, Fonts)
│   └── images/
├── src/
│   ├── components/             # Wiederverwendbare UI-Komponenten
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   ├── CTAButton.astro
│   │   ├── ReferenzCard.astro
│   │   ├── BlogTeaser.astro
│   │   └── ProduktCard.astro
│   ├── layouts/
│   │   ├── LayoutMain.astro    # Standard-Layout
│   │   └── LayoutBlog.astro    # Blog-spezifisches Layout
│   └── pages/
│       ├── index.astro         # Startseite
│       ├── leistungen/
│       │   ├── index.astro
│       │   ├── it-consulting.astro
│       │   ├── ki-beratung.astro
│       │   ├── websites.astro
│       │   ├── schulungen.astro
│       │   └── methoden.astro
│       ├── digitale-produkte/
│       │   ├── index.astro
│       │   ├── prompts.astro
│       │   └── templates.astro
│       ├── referenzen.astro
│       ├── ueber-uns.astro
│       ├── blog/
│       │   ├── index.astro     # Blog-Übersicht
│       │   └── [...slug].astro # Dynamische Blog-Post-Routes
│       └── kontakt.astro
├── content/
│   ├── blog/                   # Alle Blogposts als .md
│   │   └── 2026-03-12-prompt-engineering.md
│   └── referenzen/             # Referenz-Detailseiten als .md (optional)
├── astro.config.mjs
├── tailwind.config.mjs
├── package.json
└── README.md
```

---

## 4. Routing & Seitenstruktur

| URL | Inhalt |
|---|---|
| `/` | Startseite |
| `/leistungen` | Leistungsübersicht |
| `/leistungen/it-consulting` | IT-Consulting & Business Analyse |
| `/leistungen/ki-beratung` | KI-Beratung & KI-Schulungen |
| `/leistungen/websites` | Webseiten & CMS |
| `/leistungen/schulungen` | Schulungen |
| `/leistungen/methoden` | Projektmethoden |
| `/digitale-produkte` | Digitale Produkte Übersicht |
| `/digitale-produkte/prompts` | Prompt-Sets |
| `/digitale-produkte/templates` | Templates & Checklisten |
| `/referenzen` | Alle Referenzen |
| `/ueber-uns` | Über TechSulting |
| `/blog` | Blog-Übersicht |
| `/blog/[slug]` | Einzelner Blogpost |
| `/kontakt` | Kontaktseite |

---

## 5. SEO-Strategie

### Keyword-Cluster

| Seite | Haupt-Keyword | Neben-Keywords |
|---|---|---|
| Startseite | IT-Consulting Köln | IT-Beratung Freiberufler NRW, KI-Berater Köln |
| IT-Consulting | Business Analyse Finanzdienstleister | Anforderungsmanagement PRINCE2, IT-Projektmanagement |
| KI-Beratung | KI-Beratung Unternehmen | Prompt Engineering, KI-Schulung, zertifizierter KI-Berater |
| Digitale Produkte | Prompts kaufen Download | ChatGPT Prompts Business, Prompt-Set kaufen |
| Blog | IT-Consulting Blog | KI im Unternehmen, Business Analyse Praxis |

### Onpage-Maßnahmen
- H1 per Seite (klar, keyword-stark)
- Meta-Title max. 60 Zeichen, Meta-Description max. 160 Zeichen
- Open Graph Tags für alle Seiten (Social Sharing)
- Strukturierte Daten (JSON-LD): Organization, BlogPosting, Product
- Interne Verlinkung: Leistungsseiten ↔ Referenzen ↔ Blog ↔ Produkte
- Sitemap.xml automatisch generiert
- robots.txt konfiguriert

---

## 6. Blog-Workflow (Content-Pipeline)

```
1. Neuer Blogpost als .md in /content/blog/ anlegen
2. Frontmatter: title, description, date, slug, tags
3. Artikel in VS Code schreiben
4. Git commit & push → automatisches Deploy via GitHub Actions
5. Aus Artikel ableiten:
   a. 3-5 Social-Media-Posts (LinkedIn, Xing)
   b. Video-Skript (YouTube Short oder Langvideo)
   c. Newsletter-Teaser
```

---

## 7. Digitale Produkte & Checkout-Integration

- Produktseiten auf techsulting.de mit Beschreibung, Vorschau und Benefits
- Kauf-Button verlinkt auf **Digistore24** oder **Gumroad** (externer Checkout)
- Nach Kauf: automatischer Download-Link per E-Mail (über Plattform)
- Keine eigene Payment-Infrastruktur nötig → geringer Aufwand, DSGVO-konform

---

## 8. Externe KI-Produkt-Webseiten

- Eigene Domains für Micro-Tools (z. B. `ki-dateiumbenenner.de`)
- Technologie: Astro oder einfaches Vite-Projekt
- Verlinkung: Jede Produktseite → zurück zu techsulting.de (Backlink-Netzwerk)
- Einstieg: Tool auf subdomain oder eigenem Produkt-Teaser auf techsulting.de

---

## 9. Umsetzungsschritte (Phasenplan)

### Phase 1: Setup (Woche 1)
- [ ] GitHub-Repository anlegen
- [ ] Astro-Projekt initialisieren (`npm create astro@latest`)
- [ ] Tailwind CSS integrieren
- [ ] Basis-Layout (Header, Footer, Navigation) implementieren

### Phase 2: Statische Seiten (Woche 2-3)
- [ ] Startseite
- [ ] Alle Leistungsseiten
- [ ] Referenzen-Seite
- [ ] Über uns, Kontakt
- [ ] Formspree/Netlify Forms für Kontaktformular

### Phase 3: Blog-System (Woche 3-4)
- [ ] Blog-Übersichtsseite mit Teaser-Karten
- [ ] Dynamische Blog-Post-Routes (`[...slug].astro`)
- [ ] Erste 3 Blogartikel verfassen und einstellen
- [ ] RSS-Feed generieren

### Phase 4: Digitale Produkte (Woche 4-5)
- [ ] Produktseiten anlegen
- [ ] Kauf-Buttons mit Digistore24/Gumroad verknüpfen
- [ ] Erste 2 Prompt-Sets fertigstellen und hochladen

### Phase 5: SEO & Finalisierung (Woche 5-6)
- [ ] Meta-Tags, OG-Tags auf allen Seiten prüfen
- [ ] Sitemap & robots.txt konfigurieren
- [ ] Lighthouse-Test (Ziel: Score > 90 in allen Kategorien)
- [ ] Plausible Analytics einbinden
- [ ] Accessibility-Check

### Phase 6: Launch (Woche 6-7)
- [ ] DNS-Umstellung auf neue Seite
- [ ] Redirects von alten URLs einrichten
- [ ] LinkedIn/XING Launch-Beitrag
- [ ] Erster Blog-Artikel veröffentlichen
- [ ] Google Search Console einrichten und Sitemap einreichen

---

## 10. Launch-Checkliste

- [ ] DSGVO-konformes Impressum und Datenschutzerklärung (z. B. per eRecht24)
- [ ] SSL-Zertifikat aktiv
- [ ] Alle internen Links funktionieren
- [ ] Mobilansicht auf iPhone und Android getestet
- [ ] Ladezeiten < 2 Sekunden (Core Web Vitals)
- [ ] Kontaktformular funktioniert und sendet E-Mails
- [ ] Google Search Console eingerichtet
- [ ] Plausible/Matomo Analytics aktiv
