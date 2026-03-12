## Phase 4 Complete: Contact Form + Admin Security

Implemented a fully secured contact form with CSRF protection, honeypot, rate limiting, and Mailjet integration, plus an admin login protected by bcrypt + JWT with Astro middleware guarding all /admin/* routes.

**Files created/changed:**
- src/lib/security/input-sanitizer.ts
- src/lib/security/rate-limiter.ts
- src/lib/security/csrf.ts
- src/lib/services/auth.ts
- src/lib/services/mailjet.ts
- src/components/forms/ContactForm.astro
- src/pages/api/contact.ts
- src/pages/api/admin-auth.ts
- src/pages/api/admin-logout.ts
- src/middleware.ts
- src/pages/de/kontakt.astro (updated)
- src/pages/en/kontakt.astro (updated)
- src/pages/de/admin/login.astro (updated)
- src/pages/de/admin/blog-editor.astro (updated)
- tests/contact-form.spec.ts
- tests/admin.spec.ts

**Functions created/changed:**
- sanitizeText(), sanitizeEmail(), validateContactForm() – input-sanitizer.ts
- checkRateLimit() – rate-limiter.ts
- generateCsrfToken(), validateCsrfToken() – csrf.ts
- createJwt(), verifyJwt(), setAuthCookie(), clearAuthCookie() – auth.ts
- sendContactEmail() – mailjet.ts
- POST /api/contact – contact.ts
- POST /api/admin-auth – admin-auth.ts
- GET /api/admin-logout – admin-logout.ts
- onRequest middleware – middleware.ts

**Tests created/changed:**
- tests/contact-form.spec.ts – 5 tests (form visibility, honeypot, CSRF, required fields, EN page)
- tests/admin.spec.ts – 3 tests (login page renders, wrong password stays on login, unauthenticated redirect)

**Review Status:** APPROVED

**Git Commit Message:**
feat: add contact form with security + admin login

- CSRF token (httpOnly cookie + constant-time compare) on contact form
- Honeypot field and in-memory rate limiting (5 req/h per IP)
- Input sanitization (HTML stripping, max-length, email validation)
- Mailjet REST API integration for contact email delivery
- JWT HS256 (8h, httpOnly Secure SameSite=Strict) for admin auth
- bcryptjs password comparison for admin login
- Astro middleware protecting all /admin/* routes
- 8 new Playwright tests (contact-form + admin)
