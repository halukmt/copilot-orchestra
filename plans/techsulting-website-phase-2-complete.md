## Phase 2 Complete: Alle Content-Seiten (DE + EN)

Alle Content-Seiten für DE und EN wurden vollständig implementiert. SEOHead-Komponente, LegalLayout und PlaceholderImage wurden erstellt. Der Build läuft erfolgreich mit dem Netlify-Adapter. Alle 60 Playwright-Tests sind grün.

**Files created/changed:**
- `src/components/shared/SEOHead.astro` (neu)
- `src/components/shared/PlaceholderImage.astro` (neu)
- `src/layouts/LegalLayout.astro` (neu)
- `src/layouts/BaseLayout.astro` (geändert: SEOHead integriert)
- `astro.config.mjs` (geändert: output static + netlify adapter)
- `src/pages/de/index.astro` (ersetzt: voller Homepage-Content)
- `src/pages/de/ueber-uns.astro` (neu)
- `src/pages/de/referenzen.astro` (neu, 18 Referenzen als collapse cards)
- `src/pages/de/leistungen/index.astro` (neu)
- `src/pages/de/leistungen/it-consulting.astro` (neu)
- `src/pages/de/leistungen/ki-beratung.astro` (neu)
- `src/pages/de/leistungen/schulungen.astro` (neu)
- `src/pages/de/leistungen/methoden.astro` (neu)
- `src/pages/de/leistungen/websites.astro` (neu)
- `src/pages/de/digitale-produkte/index.astro` (neu)
- `src/pages/de/digitale-produkte/prompts.astro` (neu)
- `src/pages/de/kontakt.astro` (neu, Platzhalter für Phase 4)
- `src/pages/de/impressum.astro` (neu)
- `src/pages/de/datenschutz.astro` (neu)
- `src/pages/de/agb.astro` (neu)
- `src/pages/de/blog/index.astro` (neu, Platzhalter für Phase 3)
- `src/pages/de/admin/login.astro` (neu, Platzhalter für Phase 4)
- `src/pages/de/admin/blog-editor.astro` (neu, Platzhalter für Phase 4)
- `src/pages/en/index.astro` (ersetzt: voller EN-Content)
- `src/pages/en/ueber-uns.astro` (neu)
- `src/pages/en/referenzen.astro` (neu)
- `src/pages/en/leistungen/index.astro` (neu)
- `src/pages/en/leistungen/it-consulting.astro` (neu)
- `src/pages/en/leistungen/ki-beratung.astro` (neu)
- `src/pages/en/leistungen/schulungen.astro` (neu)
- `src/pages/en/leistungen/methoden.astro` (neu)
- `src/pages/en/leistungen/websites.astro` (neu)
- `src/pages/en/digitale-produkte/index.astro` (neu)
- `src/pages/en/digitale-produkte/prompts.astro` (neu)
- `src/pages/en/kontakt.astro` (neu)
- `src/pages/en/impressum.astro` (neu)
- `src/pages/en/datenschutz.astro` (neu)
- `src/pages/en/blog/index.astro` (neu)
- `tests/seo.spec.ts` (neu)
- `tests/content.spec.ts` (neu)

**Functions created/changed:**
- `SEOHead` – title/description/canonical/hreflang(de/en/x-default)/OG/Twitter/JSON-LD
- `PlaceholderImage` – Bildplatzhalter mit Dimension-Label
- `LegalLayout` – Wrapper für Impressum/Datenschutz/AGB
- `BaseLayout` – integriert SEOHead, akzeptiert jsonLd prop

**Tests created/changed:**
- `tests/seo.spec.ts`: 42 Tests (title+description, canonical, hreflang für alle DE-Seiten; JSON-LD für Homepage/Schulungen/Produkte)
- `tests/content.spec.ts`: 8 Tests (Homepage DE/EN, Referenzen collapse, Legal-Seiten, Leistungen, Digitaleprodukte)

**Review Status:** APPROVED

**Fix Applied:**
- `astro.config.mjs`: `output: 'hybrid'` → `output: 'static'` (Astro 5 entfernte hybrid-Mode), `adapter: netlify()` hinzugefügt
- AGB description verlängert von 19→51 Zeichen (Test erwartete >20)
