# DatumFolio
<div align="center">

# DatumFolio

**The reference point for design researchers.**

[![Status](https://img.shields.io/badge/Status-In_Development-C23B22)]()
[![Version](https://img.shields.io/badge/Version-1.0_Pre--Release-1C1917)]()
[![Next.js](https://img.shields.io/badge/Next.js-15-000000?logo=nextdotjs)]()
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript)]()
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-3FCF8E?logo=supabase)]()
[![Mapbox](https://img.shields.io/badge/Mapbox-GL_JS-000000?logo=mapbox)]()

</div>

---

## What is DatumFolio

DatumFolio is a comprehensive information hub for design researchers across the domains of Architecture, Urban Design, Interactive Design, Digital Fine Arts, Human-Centered Design, Product Design, and Participatory Design. It serves as a single destination for pursuing higher education, discovering research opportunities, initiating research, and collaborating globally [1].

The platform aggregates graduate programs, conference CFPs, journal calls for contributions, fellowships, and research positions into one structured, searchable, and comparable interface. It is global from day one, research-driven by design, and does not rank institutions [1].

---

## Who It's For

| Audience | Use Case |
|----------|----------|
| Prospective graduate students | Find and compare programs globally |
| Current graduate students | Discover conferences, publishing opportunities, grants |
| PhD candidates | Research positions, collaborations, conferences |
| Early-career researchers | Opportunities, visibility, connections |
| Faculty | Collaborators, students, conference partners |
| Practicing professionals | Research engagement, career transition |

---

## Key Features

- **Graduate Program Search** — Browse, filter, and compare programs across 300+ institutions globally with an interactive map view [1]
- **Side-by-Side Comparison** — Compare up to 5 programs with highlighted differences across admissions, cost, and structure [1]
- **Research Opportunities** — Curated conferences, journal CFPs, fellowships, and innovation opportunities with deadline tracking. Public teaser listings (title + deadline) are discoverable by anyone; full details require a free account [1]
- **Interactive Map** — Programs visualized geographically with discipline-coded markers and clustered views [1]
- **Data Transparency** — Every program profile shows a completeness indicator showing how much data has been verified [1]
- **Community** — Discord-linked community for discussion, peer support, and moderated opportunity submissions [1]
- **Newsletter** — Opt-in weekly digest of new programs, upcoming deadlines, and featured opportunities. Explicit consent required at signup; off by default [1]

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 15 (App Router, TypeScript) |
| Styling | Tailwind CSS 4 + CSS Custom Properties |
| Database | Supabase (PostgreSQL + Auth + RLS) |
| Authentication | Supabase Auth (Email + Password) |
| Map | Mapbox GL JS via react-map-gl |
| Hosting | Vercel |
| Email | Resend |
| Community | Discord (external) |
| Analytics | Vercel Analytics |

---

## Current Status

| Item | Status |
|------|--------|
| Version | 1.0 Pre-Release |
| Database Schema | Defined |
| Design System | Defined |
| Graduate Program Data | In collection (target: 300 institutions) |
| Opportunity Data | In curation |
| Platform Build | In development |

---

## Roadmap

| Phase | Timeline | Scope |
|-------|----------|-------|
| Foundation + Programs | Week 1–5 | Core platform, program search, map, filters, detail pages |
| Auth + User Features | Week 5–7 | Accounts, shortlist, comparison, dashboard |
| Research Opportunities | Week 7–9 | Conferences, journals, fellowships, innovation board |
| Community + Growth | Week 9–11 | Discord, submissions, newsletter, SEO, blog |
| Data Enrichment | Week 11–13+ | Scale to 300 institutions, 50+ opportunities |

---

## Documentation

| Document | Purpose |
|----------|---------|
| [PRD.md](./PRD.md) | Product Requirements Document — full scope, features, audience, strategy |
| [docs/TECHNICAL-SPEC.md](./docs/TECHNICAL-SPEC.md) | Technical Specification — database schema, API contracts, components, tokens |
| [docs/FEATURE-MAP.md](./docs/FEATURE-MAP.md) | Visual feature hierarchy |
| [docs/DATA-ARCHITECTURE.md](./docs/DATA-ARCHITECTURE.md) | Database structure flowchart |
| [docs/FOLDER-STRUCTURE.md](./docs/FOLDER-STRUCTURE.md) | Codebase organization |
| [docs/DISCORD-STRUCTURE.md](./docs/DISCORD-STRUCTURE.md) | Community server layout |

---

## Setup

### Prerequisites

- GitHub account
- Vercel account (linked to GitHub)
- Supabase account
- Mapbox account

### Environment Variables

| Variable | Source |
|----------|--------|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase → Settings → API |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase → Settings → API |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase → Settings → API |
| `NEXT_PUBLIC_MAPBOX_TOKEN` | Mapbox → Account → Tokens |
| `RESEND_API_KEY` | Resend → Dashboard |
| `ADMIN_SECRET` | Self-defined |

### Deployment

1. Fork/clone this private repository
2. Connect to Vercel (Framework Preset: Next.js)
3. Add environment variables in Vercel project settings
4. Run database schema SQL in Supabase SQL Editor
5. Every commit to `main` auto-deploys

---

## Contributing

This is a private repository in active development. Contributions are by invitation only.

If you would like to contribute data corrections, opportunity submissions, or feature suggestions:

1. Join the DatumFolio Discord community
2. Use the community submission form on the platform
3. Or reach out directly via the contact below

All community-submitted data is moderated before publication.

---

## License

All rights reserved. This is proprietary software.

Unauthorized copying, distribution, or modification of this codebase is strictly prohibited without explicit written permission from the author.

---

## Contact

**Creator:** Asif Hasan Zeshan
**Email:** asifzeshan@arizona.edu
**Portfolio:** [asifhasanzeshan.xyz](https://www.asifhasanzeshan.xyz)
**Institution:** University of Arizona

---

<div align="center">

*DatumFolio — Where design research begins.*

</div>
