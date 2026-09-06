# Hermes Landing Builder — Drag-and-Drop Landing Page Builder

> Build beautiful marketing landing pages with a visual section-based editor, live preview, SEO management, and analytics dashboard.

## Stack

- **Frontend:** Angular 22 + Signals + Zoneless + Tailwind CSS v4 + dark mode
- **Backend:** NestJS + TypeORM + JWT Auth (Passport.js)
- **Database:** SQLite (dev) / Neon PostgreSQL (prod-ready schema)
- **CI/CD:** GitHub Actions workflow YAML
- **AI:** None — counter=2 non-AI build

## Project Decision Table

| Factor | Decisión |
|--------|----------|
| **Cycle** | Counter=2 — non-AI build (1-3 = full-stack) |
| **Weak skill #1** | Neon/Postgres (2→3) — JSONB for component data, PostgreSQL-compatible schema, FTS for SEO analytics, environment-aware migrations |
| **Weak skill #2** | Testing (3→4) — Section ordering logic, component validation, publishing status machine, analytics aggregation, preview rendering |
| **Domain** | Marketing / Dev Tools — Landing page builder with visual editor |
| **Diferencia** | First project in marketing/landing-page domain; first section-based visual editor with JSONB flexible schema; first analytics tracking with PostgreSQL aggregation |
| **Últimos 10** | Survey Engine (data collection), Job Tracker (career), Team Wiki (content), Billing Portal (fintech), Time Tracker (time tracking), Help Desk (support), Code Review Assistant (dev tools), Social Scheduler (social media), Workout Tracker (health), Inventory Manager (inventory) — none in marketing domain |

## Público objetivo

- **Marketers** creating campaign landing pages without developers
- **Small business owners** building their online presence
- **Freelancers** launching simple sites for clients
- **Developers** needing rapid landing page prototypes

## Features Priorizadas

### Must Have (MVP)

- [ ] **User auth** — register, login, JWT-protected routes
- [ ] **Project CRUD** — create, list, edit, delete landing pages
- [ ] **Section-based editor** — add/reorder/remove sections (hero, features, pricing, testimonials, FAQ, CTA, footer)
- [ ] **Component customization** — edit text, colors, layout per section (JSONB flexible schema)
- [ ] **Live preview** — see changes in real-time while editing
- [ ] **Publish workflow** — draft → published with public URL
- [ ] **Template library** — start from pre-built templates (3+ templates)
- [ ] **SEO settings** — meta title, description, OG tags per project
- [ ] **Dashboard** — list of projects with status, view count, last edited
- [ ] **Analytics** — page views per project (simple counter + daily breakdown)
- [ ] **Responsive design** — preview desktop + mobile views
- [ ] **Dark mode** — full dark mode support

### Should Have

- [ ] **Section presets** — multiple layout variants per section type (e.g., 2-column features, 3-column features)
- [ ] **Custom CSS** — inject custom styles per project
- [ ] **Favicon upload** — custom favicon per landing page
- [ ] **Custom domain** — domain/subdomain binding (UI only — DNS setup manual)
- [ ] **Export to HTML** — download static HTML of published page

### Could Have

- [ ] **Collaboration** — invite team members to edit
- [ ] **A/B testing** — create variants and track conversion
- [ ] **Asset library** — upload and reuse images across sections
- [ ] **Form builder** — add contact forms with email notifications
- [ ] **Lead capture** — collect emails from visitor forms
- [ ] **Version history** — roll back to previous versions

## Limitaciones Conocidas

- **GITHUB_TOKEN**: Returns 401 (Bad credentials) — git commits prepared locally. Has occasionally worked after sourcing `export-cron-env.sh`.
- **NEON_API_KEY**: Empty or invalid — PostgreSQL/Neon integration cannot be tested. Schema must be PostgreSQL-compatible for eventual migration.
- **OPENAI_API_KEY**: Empty — not needed (non-AI build).

## Improvement Goals from Previous Projects

1. ✅ First real PostgreSQL-compatible schema with JSONB columns (exercises Neon/Postgres weakest skill)
2. ✅ Section ordering logic with position recalculation (exercises Testing weakest skill)
3. ✅ Publishing status machine with valid transitions (exercises Testing)
4. ✅ Analytics aggregation queries using PostgreSQL patterns (exercises Neon/Postgres)
5. ✅ Meaningful test suite (15+ tests across auth, sections, publishing, analytics)
