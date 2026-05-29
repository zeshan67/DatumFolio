# DatumFolio (DF)

## Product Requirements Document

**Version:** 1.0
**Date:** June 2025
**Author:** Asif Hasan Zeshan
**Status:** Pre-Development

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Platform Identity](#2-platform-identity)
3. [Problem Statement](#3-problem-statement)
4. [Target Audience](#4-target-audience)
5. [Platform Scope and Features](#5-platform-scope-and-features)
6. [Tiered Access Model](#6-tiered-access-model)
7. [Content Verticals](#7-content-verticals)
8. [Database and Data Strategy](#8-database-and-data-strategy)
9. [Technical Architecture](#9-technical-architecture)
10. [Design System](#10-design-system)
11. [User Flows](#11-user-flows)
12. [Security Model](#12-security-model)
13. [Business Model](#13-business-model)
14. [Traffic and Growth Strategy](#14-traffic-and-growth-strategy)
15. [Community Strategy](#15-community-strategy)
16. [Development Phases](#16-development-phases)
17. [Success Metrics](#17-success-metrics)
18. [Risk Analysis](#18-risk-analysis)
19. [Competitive Landscape](#19-competitive-landscape)
20. [Long-Term Vision](#20-long-term-vision)

---

## 1. Executive Summary

### 1.1 What Is DatumFolio

DatumFolio is a comprehensive information hub for design researchers across the domains of Architecture, Urban Design, Interactive Design, Digital Fine Arts, Human-Centered Design, Product Design, and Participatory Design. It serves as a single destination for pursuing higher education, discovering research opportunities (fellowships, conference publications, journal CFPs, residencies), initiating research, and collaborating globally.

### 1.2 One-Sentence Pitch

DatumFolio is the definitive research and education network for the design disciplines — where prospective students find programs, active researchers find opportunities, and the global design research community connects.

### 1.3 Why Now

- No single platform aggregates graduate programs, research opportunities, and professional connections for design researchers
- Existing tools are fragmented, US-centric, ranking-obsessed, or profession-focused rather than research-focused
- The design research community lacks a dedicated network equivalent to what IEEE provides for engineering
- Growing global demand for structured, research-driven design education resources
- The intersection of design + technology + research is expanding rapidly but has no central hub

### 1.4 Ambition

Within 3 years, DatumFolio aims to be the largest curated database of design research graduate programs and opportunities globally, with a thriving community of 50,000+ design researchers, and a sustainable privately-owned business model that nurtures inquiry and collaboration without being corporate.

---

## 2. Platform Identity

### 2.1 Name

**DatumFolio**
- Short form: **DF**
- "Datum" — an architectural reference point; a fixed starting position
- "Folio" — a curated collection of knowledge and work
- Together: a reference point for design research knowledge

### 2.2 Tagline Options

- "Where design research begins"
- "The reference point for design researchers"
- "Research. Discover. Connect."

### 2.3 Brand Character

| Attribute | Description |
|---|---|
| Tone | Intellectual but accessible. Confident but not arrogant. |
| Visual | Editorial, bold, typographically driven. Venetian red as accent against black/white/grey. |
| Personality | A trusted senior researcher who knows everyone and shares generously. |
| NOT | A corporate portal. A ranking system. A news feed. A social media clone. |

### 2.4 Disciplinary Scope

**Core Focus (research-driven, digitally innovative):**
- Architecture (design research, computation, building science)
- Urban Design
- Interactive Design
- Digital Fine Arts
- Human-Centered Design
- Product Design
- Participatory Design

**Included in graduate database but not primary identity:**
- Landscape Architecture
- Urban Planning
- Construction Management
- Real Estate Development

**Explicitly excluded:**
- Industrial Design
- Fine Arts (non-digital, non-research)
- Chinese institutions (Phase 1)
- Purely profession-focused programs without research component

### 2.5 Philosophical Position

DatumFolio is research and inquiry driven. It does not rank institutions. It does not feature design projects or built work. It does not function as an architecture news agency. It provides structured, transparent, curated information that empowers researchers to make their own decisions.

---

## 3. Problem Statement

### 3.1 For Prospective Graduate Students

- No single resource compares design research programs globally
- Conventional rankings (QS, U.S. News) are reductive and irrelevant to research-focused students
- Information is fragmented across hundreds of university websites
- Every application cycle, students reinvent the wheel building personal spreadsheets
- International students face additional complexity with no structured support

### 3.2 For Active Researchers

- Conference CFPs are scattered across dozens of mailing lists and websites
- Journal call for contributions have no centralized, discipline-specific aggregator
- Fellowship and grant opportunities require monitoring multiple disconnected sources
- Finding collaborators in adjacent design research fields is purely serendipitous
- PhD candidates have no dedicated platform connecting them to research positions

### 3.3 For the Community

- Design research lacks a unifying professional network (unlike IEEE for engineering)
- Knowledge about programs, opportunities, and funding is gatekept by institutional access
- There is no structured way to discover who is doing what research, where

---

## 4. Target Audience

### 4.1 Primary Users (Ranked)

| Priority | Segment | Core Need |
|---|---|---|
| 1 | Prospective graduate students | Find and compare programs globally |
| 2 | Current graduate students | Find conferences, publishing opportunities, grants |
| 3 | PhD candidates | Research positions, collaborations, conferences |
| 4 | Early-career researchers / postdocs | Opportunities, visibility, connections |
| 5 | Faculty | Collaborators, students, conference partners |
| 6 | Practicing professionals | Research engagement, career transition |

### 4.2 Geographic Scope

**Global from Day 1** across all content verticals.

Graduate program database launch coverage:
- United States: 150 institutions
- United Kingdom: 30 institutions
- European Union: 70 institutions
- Australia: 15 institutions
- East/Southeast Asia (Japan, Korea, Singapore, Malaysia, Indonesia): 35 institutions
- **Total launch target: 300 institutions, each with multiple programs**

Not included in Phase 1: China, Africa, South America, Middle East (future expansion).

### 4.3 User Personas

**Persona 1: Aisha (Prospective Student)**
- 24, B.Arch from Bangladesh, wants an MS in Computational Design
- Needs: compare programs across US and Europe, understand costs, deadlines, research fit
- Pain: spending 100+ hours on fragmented university websites

**Persona 2: Marcus (Current PhD Student)**
- 28, PhD candidate in Urban Analytics at TU Delft
- Needs: find relevant conferences with upcoming CFPs, identify journals for his paper, discover fellowships
- Pain: missing opportunities because information is scattered

**Persona 3: Dr. Yuki (Early-Career Faculty)**
- 34, Assistant Professor in Interactive Design at a Japanese university
- Needs: find collaborators working on participatory design + AI, discover students for her lab
- Pain: isolated in her institution, no network beyond her immediate academic circle

---

## 5. Platform Scope and Features

### 5.1 Feature Map
DATUMFOLIO
```mermaid
flowchart TD
    DF[DATUMFOLIO]

    DF --> GPS[Graduate Program Search — Global]
    DF --> RO[Research Opportunities]
    DF --> COM[Community]
    DF --> UA[User Accounts]
    DF --> FUT[Future — Tier 3]

    %% Graduate Program Search
    GPS --> GPS1[Browse and filter programs]
    GPS --> GPS2[Interactive map view]
    GPS --> GPS3[Program detail pages]
    GPS --> GPS4[Side-by-side comparison — up to 5]
    GPS --> GPS5[Shortlist / save programs]
    GPS --> GPS6[Data completeness transparency]

    %% Research Opportunities
    RO --> RO1[Conferences & CFPs — deadlines, themes, locations]
    RO --> RO2[Journal Call for Contributions — live calls only]
    RO --> RO3[Fellowships & Grants]
    RO --> RO4[Innovation Opportunities — PhD, postdocs, research roles]

    %% Community
    COM --> COM1[Discord server — linked]
    COM --> COM2[Newsletter / Weekly Digest]
    COM --> COM3[Community submission portal — moderated]
    COM --> COM4[Future: Collaboration matchmaking]

    %% User Accounts
    UA --> UA1[Email + password authentication]
    UA --> UA2[Profile — disciplines, interests, career stage]
    UA --> UA3[Saved items — programs, opportunities]
    UA --> UA4[Future: Public researcher profile]

    %% Future Tier 3
    FUT --> FUT1[University research lab profiles]
    FUT --> FUT2[Mentoring marketplace]
    FUT --> FUT3[Portfolio review service]
    FUT --> FUT4[Grant writing assistance]
    FUT --> FUT5[Premium institutional features]

### 5.2 Feature Prioritization

| Feature | Tier | Launch Priority | Complexity |
|---|---|---|---|
| Program search and browse | Tier 1 (free) | Critical | Medium |
| Program comparison | Tier 2 (free account) | Critical | Medium |
| Program shortlist | Tier 2 (free account) | Critical | Low |
| Interactive map | Tier 1 (free) | Critical | Medium |
| Conference / CFP board | Tier 2 (free account) | High | Low |
| Journal CFP board | Tier 2 (free account) | High | Low |
| Fellowship board | Tier 2 (free account) | High | Low |
| Innovation Opportunity board | Tier 2 (free account) | High | Low |
| User authentication | — | Critical | Medium |
| Newsletter / digest | Tier 1 (free) | High | Low |
| Community submission form | — | Medium | Low |
| Discord integration | — | High | None (external) |
| Admin moderation panel | Internal | Medium | Medium |
| Lab profiles | Tier 3 (paid) | Future | High |
| Collaboration matching | Tier 3 (paid) | Future | High |
| Mentoring / portfolio review | Tier 3 (paid) | Future | High |

---

## 6. Tiered Access Model

### 6.1 Tier 1 — Open (No Account Required)

| Access | Details |
|---|---|
| Browse graduate programs | Full search, filter, map view |
| View program detail pages | All public program data |
| Read newsletter/digest | Weekly research news |
| View About, FAQ, general content | — |

### 6.2 Tier 2 — Free Account (Email + Password)

| Access | Details |
|---|---|
| Everything in Tier 1 | — |
| Save / shortlist programs | Persisted to account |
| Compare programs side by side | Up to 5 programs |
| Access Conferences & CFPs | Full board with deadlines |
| Access Journal CFPs | Live calls for contributions |
| Access Fellowships & Grants | Full listings |
| Access Innovation Opportunities | PhD, postdoc, research roles |
| Submit opportunities (moderated) | Community contributions |
| Discord community access | Linked from platform |

### 6.3 Tier 3 — Premium (Paid, Future)

| Access | Details |
|---|---|
| Everything in Tier 2 | — |
| University research lab profiles | Detailed lab + faculty info |
| Collaboration matchmaking | AI-suggested connections |
| Mentoring access | 1:1 with established researchers |
| Portfolio review service | Expert feedback on applications |
| Grant writing assistance | Guided support |
| Specialized job coaching | Application strategy |
| Priority community features | — |

### 6.4 Year 1 Strategy

- Tier 1 and Tier 2 only
- No paid features deployed in Year 1
- Focus: database strength, user acquisition, community building
- Tier 3 features designed and documented but not built until funding secured

---

## 7. Content Verticals

### 7.1 Graduate Programs

**Schema per program:**
- Institution (name, location, type, coordinates)
- Program (name, degree, discipline, duration, credits, accreditation)
- Admissions (deadlines, requirements, acceptance rate, portfolio, GRE, language tests)
- Costs (tuition, fees, aid, scholarships, total estimate)
- Research focus areas
- Data completeness score
- Data provenance (source, verification date, quality)

**Launch target:** 300 institutions globally, each with 1-5 programs = ~600-1000 program entries.

### 7.2 Conferences & CFPs

**Schema per conference:**
- Name
- Organizing body
- Theme / focus area
- Disciplines relevant
- CFP deadline
- Conference dates
- Location (city, country) or Virtual
- Website URL
- Submission format (paper, poster, workshop, etc.)
- Registration cost range
- Status (CFP Open, CFP Closed, Upcoming, Past)

**Launch target:** 5 curated, high-quality entries. Grow to 50+ within 6 months.

**Update frequency:** Weekly curation.

### 7.3 Journal Call for Contributions

**Schema per call:**
- Journal name
- Publisher
- Special issue theme
- Disciplines relevant
- Submission deadline
- Expected publication date
- Peer review type (blind, double-blind, open)
- Open access status
- Website URL
- Status (Open, Closing Soon, Closed)

**Launch target:** 5 curated, live calls. Grow as calls cycle.

**Update frequency:** Weekly curation.

### 7.4 Fellowships & Grants

**Schema per opportunity:**
- Name
- Granting organization
- Amount / range
- Disciplines eligible
- Career stage (student, early-career, faculty)
- Geographic eligibility
- Application deadline
- Duration
- Website URL
- Status (Open, Closing Soon, Closed)

**Launch target:** 3-5 curated entries.

**Update frequency:** Monthly curation (annual cycles).

### 7.5 Innovation Opportunities

**Schema per opportunity:**
- Title
- Institution / organization
- Type (PhD position, Postdoc, Research Assistant, Faculty, Industry Research)
- Disciplines relevant
- Location or Remote
- Duration
- Funding status (Funded, Partially Funded, Unfunded)
- Application deadline
- Website URL
- Contact
- Status (Open, Closed)

**Launch target:** 5-10 curated entries.

**Update frequency:** Weekly curation.

### 7.6 Newsletter / Digest

- Weekly email digest
- Curated highlights: new programs added, upcoming deadlines, featured opportunities
- Tier 1 (free, no account needed — just email signup)
- Sent via email service (Resend, Mailchimp free tier, or Supabase edge function)

---

## 8. Database and Data Strategy

### 8.1 Data Architecture
CORE TABLES
├── institutions
├── programs
├── admissions
├── costs
├── metadata
│
OPPORTUNITY TABLES
├── conferences
├── journal_calls
├── fellowships
├── innovation_opportunities
│
USER TABLES
├── users (auth)
├── profiles
├── saved_programs
├── saved_opportunities
│
COMMUNITY TABLES
├── submissions (moderated queue)
├── newsletter_signups


### 8.2 Data Collection Strategy

**Primary method:** Manual curation by founder.

**Sources for graduate programs:**
- NAAB (US Architecture accreditation)
- RIBA (UK)
- ECTS program databases (EU)
- Individual university websites
- Government education databases per country

**Sources for opportunities:**
- Academic mailing lists
- Conference websites
- Publisher portals (Elsevier, Taylor & Francis, MDPI)
- Fellowship databases (Fulbright, DAAD, etc.)
- University job boards

**Community contributions:**
- Open submission form on website
- All submissions require moderation before going live
- Submitters must provide source URL for verification

### 8.3 Data Quality Framework

| Quality Level | Definition | Display |
|---|---|---|
| Verified | Data confirmed from official source within 6 months | Green indicator |
| Current | Data from official source, 6-12 months old | No special indicator |
| Aging | Data over 12 months old, may have changed | Yellow indicator |
| Unverified | Community-submitted, not yet confirmed | Grey indicator |

### 8.4 Data Completeness

Every program profile shows a completeness percentage:
- What percentage of fields are filled
- Transparently displayed to users
- NOT a quality rating of the program

### 8.5 Update Cadence

| Content | Update Frequency | Responsibility |
|---|---|---|
| Graduate programs | Annually (tuition, deadlines) | Founder |
| Conferences / CFPs | Weekly | Founder + community |
| Journal calls | Weekly | Founder + community |
| Fellowships | Monthly | Founder |
| Innovation opportunities | Weekly | Founder + community |

---

## 9. Technical Architecture

### 9.1 Stack

| Layer | Technology | Rationale |
|---|---|---|
| Framework | Next.js 15 (App Router, TypeScript) | Full-stack, SSR, API routes, free deployment |
| Styling | Tailwind CSS 4 + CSS Custom Properties | Utility-first, design tokens |
| Database | Supabase (PostgreSQL + Auth) | Free tier, built-in auth, RLS, real-time |
| Authentication | Supabase Auth (Email + Password) | Simple, secure, free |
| Map | Mapbox GL JS | Beautiful, customizable, free tier generous |
| Hosting | Vercel | Free, auto-deploy from GitHub |
| Email | Resend (free tier) or Supabase Edge Functions | Transactional + newsletter |
| Community | Discord (external) | Free, established, design-friendly |
| Analytics | Vercel Analytics (free) or Plausible | Privacy-respecting |
| Font | Custom selection (TBD) | Brand-specific |

### 9.2 Architecture Principles

- **Modular:** Every feature is a self-contained module
- **Server-first:** Data fetching on server, interactivity on client only where needed
- **API-mediated:** Frontend never talks to database directly
- **Documented:** Every file has clear comments, every pattern is replicable
- **Correctable:** Small, focused files that can be replaced without cascading breaks

### 9.3 Folder Structure (Planned)
datumfolio/
├── src/
│ ├── app/
│ │ ├── (auth)/ # Login, signup, password reset
│ │ ├── (marketing)/ # Home, about, pricing
│ │ ├── programs/ # Graduate program search
│ │ ├── explore/ # Map view
│ │ ├── opportunities/ # Conferences, CFPs, fellowships
│ │ ├── community/ # Submission form, Discord link
│ │ ├── dashboard/ # User dashboard (Tier 2)
│ │ ├── api/ # API routes
│ │ └── admin/ # Admin/moderation (future)
│ ├── components/
│ │ ├── ui/ # Design system primitives
│ │ ├── programs/ # Program-specific components
│ │ ├── opportunities/ # Opportunity-specific components
│ │ ├── map/ # Map components
│ │ └── layout/ # Header, footer, navigation
│ ├── lib/
│ │ ├── supabase/ # Database clients
│ │ ├── api/ # Response helpers, client functions
│ │ └── hooks/ # Custom React hooks
│ └── types/ # TypeScript definitions
├── public/ # Static assets
└── docs/ # Documentation

### 9.4 Environment Variables

| Variable | Purpose |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase public key |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase admin key (server only) |
| `NEXT_PUBLIC_MAPBOX_TOKEN` | Mapbox access token |
| `RESEND_API_KEY` | Email sending (future) |
| `ADMIN_SECRET` | Admin route protection |

---

## 10. Design System

### 10.1 Design Philosophy

DatumFolio's interface should feel like a well-designed research publication — bold typography, generous whitespace, structured grids, and moments of vibrant color that create energy without overwhelming. It is editorial, not decorative. It is typographically driven, not illustration-heavy.

**Inspiration direction:** Swiss-modern editorial design with a single bold accent color (Venetian Red) used sparingly for emphasis, interaction states, and brand moments.

### 10.2 Color Palette

| Token | Value | Usage |
|---|---|---|
| **Background** | `#FAFAF9` | Page background |
| **Surface** | `#FFFFFF` | Cards, modals |
| **Surface Muted** | `#F5F5F4` | Section backgrounds |
| **Ink Primary** | `#1C1917` | Headings, primary text, buttons |
| **Ink Secondary** | `#57534E` | Body text |
| **Ink Tertiary** | `#A8A29E` | Labels, captions, metadata |
| **Line** | `#E7E5E4` | Borders, dividers |
| **Line Strong** | `#D6D3D1` | Active borders |
| **Accent (Venetian Red)** | `#C23B22` | CTAs, active states, brand moments |
| **Accent Hover** | `#A83220` | Hover state |
| **Accent Light** | `#FDF2F0` | Accent background tint |
| **Success** | `#15803D` | Verified, confirmed |
| **Warning** | `#B45309` | Closing soon, attention |
| **Info** | `#1D4ED8` | Informational |

### 10.3 Accent Color Usage Rules

The Venetian Red (`#C23B22`) is used **sparingly** for maximum impact:

**DO use for:**
- Primary CTA buttons (one per page section maximum)
- Active navigation indicator (thin underline or dot)
- Key deadlines that are closing soon
- The DatumFolio logo mark
- Selected/active filter states
- "New" or "Featured" badges
- Link hover states (subtle)

**DO NOT use for:**
- Body text
- Backgrounds (except very light tint `#FDF2F0`)
- Multiple elements competing on the same screen
- Decorative purposes without meaning

### 10.4 Typography

| Role | Font | Size | Weight |
|---|---|---|---|
| Display | TBD (considering: Instrument Serif, Fraunces, or custom) | 3-4rem | 500-600 |
| Headings | Inter Variable or similar sans | 1.3-2.25rem | 600 |
| Body | Inter Variable | 0.875-1rem | 400 |
| Labels | Inter Variable | 0.6875rem | 500, uppercase, tracked |
| Data/Numbers | JetBrains Mono | varies | 600 |
| Code/Technical | JetBrains Mono | 0.8125rem | 400 |

### 10.5 Layout Grid

| Container | Max Width | Usage |
|---|---|---|
| Full | 1440px | Page container |
| Content | 1200px | Main content area |
| Prose | 720px | Long-form text |
| Narrow | 540px | Forms, focused content |
| Detail | 860px | Program detail pages |

### 10.6 Component Library (Planned)

Core primitives:
- Button (primary, secondary, ghost, danger)
- Input (text, email, password, select, textarea)
- Card (standard, hoverable, featured)
- Badge (discipline, status, tier)
- DataPoint (label + value, mono style)
- DataCompletenessBadge
- Tag / Chip
- Modal
- Dropdown
- Toast / Notification
- Skeleton loader
- Section / Container / Stack / Grid

---

## 11. User Flows

### 11.1 Prospective Student Flow
Landing Page
→ Browse Programs (Tier 1, no login)
→ Filter by country, discipline, degree type
→ View map with markers
→ Click program card → Detail page
→ Wants to save / compare
→ Prompt: "Create free account to save programs"
→ Sign up (email + password)
→ Save to shortlist
→ Compare up to 5 programs
→ Receives newsletter with deadline reminders


### 11.2 Active Researcher Flow
Landing Page
→ Sign up / Log in
→ Dashboard with saved items
→ Browse Opportunities
→ Conferences & CFPs tab
→ Journal Calls tab
→ Fellowships tab
→ Innovation Opportunities tab
→ Save opportunities to dashboard
→ Submit an opportunity (moderated)
→ Join Discord for community discussion


### 11.3 Community Contributor Flow

---

## 12. Security Model

### 12.1 Authentication

- Supabase Auth with email + password
- Email verification required
- Password reset via email
- Session management via Supabase (JWT tokens)
- No social login in Year 1

### 12.2 Authorization (Row Level Security)

| Table | Anonymous | Authenticated (Tier 2) | Admin |
|---|---|---|---|
| institutions | SELECT | SELECT | ALL |
| programs | SELECT | SELECT | ALL |
| admissions | SELECT | SELECT | ALL |
| costs | SELECT | SELECT | ALL |
| metadata | SELECT | SELECT | ALL |
| conferences | — | SELECT | ALL |
| journal_calls | — | SELECT | ALL |
| fellowships | — | SELECT | ALL |
| opportunities | — | SELECT | ALL |
| saved_programs | — | Own rows only | ALL |
| saved_opportunities | — | Own rows only | ALL |
| submissions | — | INSERT (own) | ALL |
| profiles | — | Own row only | ALL |

### 12.3 Data Protection

- Private GitHub repository
- Environment variables in Vercel vault (never in code)
- No PII stored beyond email and profile info
- GDPR-conscious: users can delete their account and data
- No tracking pixels, no third-party analytics cookies

---

## 13. Business Model

### 13.1 Year 1: Growth Phase (No Revenue)

- All features free (Tier 1 + Tier 2)
- Focus: database strength, user acquisition, community building
- Metric: 5,000 signups, largest design research program database globally
- Fundraising: pitch to design education organizations, university innovation grants, angel investors in ed-tech

### 13.2 Year 2-3: Monetization Phase

**Revenue Stream A: Premium User Subscriptions (Tier 3)**

| Feature | Price (est.) |
|---|---|
| Lab profiles + collaboration matching | $9/month |
| Mentoring marketplace access | $19/month |
| Portfolio review (per session) | $49-99 |
| Grant writing assistance (per project) | $149-299 |
| Bundle: All premium | $29/month |

**Revenue Stream B: Institutional Subscriptions**

| Feature | Price (est.) |
|---|---|
| Verified program badge | $500/year |
| Featured placement in search results | $1,000/year |
| Analytics dashboard (who views their program) | $2,000/year |
| Recruitment tools (contact interested students) | $3,000/year |

**Revenue Stream C: Opportunity Promotion**

| Feature | Price (est.) |
|---|---|
| Featured conference listing | $200-500 |
| Sponsored opportunity placement | $100-300 |
| Newsletter sponsored mention | $150-400 |

**Revenue Stream D: Events and Services**

| Feature | Price (est.) |
|---|---|
| Virtual research showcases | Sponsorship-funded |
| Annual design research summit | Ticket + sponsor revenue |
| Curated researcher-in-residence programs | Partnership-funded |

### 13.3 Long-Term Sustainability Philosophy

DatumFolio generates revenue primarily from institutions and premium services while keeping the core experience free for researchers. The platform grows its value by growing its community — the larger the network, the more institutions want to be visible on it, the more premium services have demand.

**Principle:** Never paywall information that helps a student make a decision. Charge for convenience, acceleration, and institutional visibility.

---

## 14. Traffic and Growth Strategy

### 14.1 Launch Strategy (Month 1)

| Channel | Action | Expected Impact |
|---|---|---|
| Product Hunt | Launch day with compelling visuals | 500-1000 visits |
| Reddit | Posts on r/architecture, r/urbanplanning, r/gradschool, r/gradadmissions | 200-500 visits per post |
| LinkedIn | Personal post + outreach to design educators | 500-1000 impressions |
| Instagram | Brand account with visual teasers, reels | Brand awareness |
| Design school communities | Direct outreach to student groups | 100-300 signups |
| X/Twitter | Threads on "how to choose a grad program" | Variable |

### 14.2 Content Marketing (Months 1-6)

| Content Type | Frequency | Channel | SEO Value |
|---|---|---|---|
| "Best programs for [topic]" guides | 2x/month | Blog on site | High (long-tail keywords) |
| "How to apply to [country] programs" | 1x/month | Blog + social | High |
| Conference deadline roundups | Weekly | Newsletter + social | Medium |
| "Researcher spotlight" interviews | 2x/month | Blog + social | Community building |
| Comparison content ("Program A vs B") | 2x/month | Blog on site | High (search intent) |
| Infographics (tuition comparison, geographic distribution) | 1x/month | Social + Pinterest | Shareability |

### 14.3 SEO Strategy

**Target keywords (long-tail):**
- "best architecture graduate programs in Europe"
- "urban design masters programs compared"
- "computational design PhD programs"
- "architecture conferences 2025 2026"
- "design research fellowships"
- "how to choose an architecture graduate school"
- "[University name] architecture program review"

**Technical SEO:**
- Every program has its own indexable page with structured data
- Every opportunity has its own page
- Blog content targets comparison and "how to" queries
- Sitemap auto-generated
- Meta descriptions for all pages

### 14.4 Community-Driven Growth (Months 3-12)

| Mechanism | Description |
|---|---|
| Discord community | Active discussions, resource sharing, peer support |
| User submissions | Community-contributed opportunities (moderated) |
| Referral program (future) | "Invite a colleague" with benefits |
| University partnerships | Programs link to their DatumFolio page from their own site |
| Student ambassador program | Reps at major design schools share the platform |

### 14.5 Partnership Strategy

| Partner Type | Value Exchange |
|---|---|
| Design schools | We feature them accurately → they link to us |
| Conference organizers | We promote their CFP → they mention us to attendees |
| Design publications (Dezeen, ArchDaily, etc.) | Cross-promotion, guest content |
| Professional bodies (RIBA, AIA, ACSA) | Data validation → credibility |
| Discord design communities | Cross-community engagement |

### 14.6 Growth Targets

| Milestone | Timeline | Target |
|---|---|---|
| Launch | Month 1 | Platform live, 300 institutions, 5 opportunities |
| Early traction | Month 3 | 1,000 signups, 500 weekly active users |
| Community established | Month 6 | 5,000 signups, active Discord, 50+ opportunities |
| Authority | Month 12 | 15,000 signups, 500+ institutions, recognized brand |
| Monetization ready | Month 18 | 30,000 signups, Tier 3 features launched |

---

## 15. Community Strategy

### 15.1 Discord Server Structure
DATUMFOLIO DISCORD
│
├── #announcements (read-only)
├── #introductions
├── #general
│
├── PROGRAMS
│ ├── #program-recommendations
│ ├── #application-tips
│ ├── #portfolio-feedback
│ └── #funding-scholarships
│
├── RESEARCH
│ ├── #conferences-cfps
│ ├── #publishing-journals
│ ├── #collaboration-wanted
│ └── #research-methods
│
├── DISCIPLINES
│ ├── #architecture
│ ├── #urban-design
│ ├── #interactive-design
│ ├── #digital-arts
│ └── #participatory-design
│
├── CAREER
│ ├── #phd-positions
│ ├── #postdoc-opportunities
│ ├── #industry-research
│ └── #career-advice
│
└── META
├── #feedback-suggestions
├── #bug-reports
└── #off-topic

### 15.2 Moderation

- Founder as primary moderator (Year 1)
- Community guidelines document
- Volunteer moderators from active members (Month 6+)
- Zero tolerance for spam, self-promotion without value

### 15.3 Community-to-Platform Loop
Discord discussion about a conference
→ Member submits it to DatumFolio via form
→ Founder approves
→ Goes live on platform
→ Shared back to Discord with link
→ More users discover platform

---

## 16. Development Phases

### 16.1 Phase 0: Foundation (Week 1-2)

- [ ] Set up new GitHub repository (private)
- [ ] Initialize Next.js 15 + TypeScript + Tailwind 4
- [ ] Set up Supabase project with Auth enabled
- [ ] Configure Vercel deployment
- [ ] Create design system tokens (colors, typography, spacing)
- [ ] Build core UI primitives
- [ ] Build root layout (header, footer, navigation)
- [ ] Build landing page

### 16.2 Phase 1: Graduate Programs (Week 2-5)

- [ ] Database schema (institutions, programs, admissions, costs, metadata)
- [ ] Seed data (50 programs across regions)
- [ ] API layer (list, filter, detail, search)
- [ ] Program directory page with filters
- [ ] Program detail page (editorial layout)
- [ ] Interactive map (Mapbox)
- [ ] Data completeness indicators

### 16.3 Phase 2: Authentication + User Features (Week 5-7)

- [ ] Supabase Auth setup (email + password)
- [ ] Sign up / Login / Password reset pages
- [ ] User profile creation
- [ ] Protected routes (Tier 2 content)
- [ ] Save / shortlist programs (persisted to account)
- [ ] Compare programs (up to 5)
- [ ] User dashboard

### 16.4 Phase 3: Research Opportunities (Week 7-9)

- [ ] Database tables (conferences, journal_calls, fellowships, opportunities)
- [ ] Seed data (5 per category)
- [ ] Opportunities browse page with tabs
- [ ] Individual opportunity detail pages
- [ ] Save opportunities to dashboard
- [ ] "Closing soon" indicators and deadline logic

### 16.5 Phase 4: Community and Growth (Week 9-11)

- [ ] Discord server setup and structure
- [ ] Community submission form (website)
- [ ] Moderation workflow (email notification to founder)
- [ ] Newsletter signup (email capture)
- [ ] SEO optimization (meta, sitemap, structured data)
- [ ] Blog/content section (for SEO articles)
- [ ] About page, FAQ, legal pages

### 16.6 Phase 5: Data Enrichment (Week 11-13+)

- [ ] Scale graduate program database to 300 institutions
- [ ] Add 50+ research opportunities
- [ ] Refine data quality and completeness
- [ ] Launch community contribution drive
- [ ] Product Hunt launch preparation

### 16.7 Ongoing (Post-Launch)

- [ ] Weekly content curation (opportunities)
- [ ] Monthly database updates (programs)
- [ ] Community management
- [ ] Growth experiments
- [ ] User feedback integration
- [ ] Performance optimization

---

## 17. Success Metrics

### 17.1 Primary KPIs

| Metric | 3 Month Target | 6 Month Target | 12 Month Target |
|---|---|---|---|
| Registered users | 1,000 | 5,000 | 15,000 |
| Monthly active users | 400 | 2,000 | 7,000 |
| Programs in database | 300 institutions | 400 institutions | 600 institutions |
| Opportunities listed | 20 | 50+ | 200+ |
| Newsletter subscribers | 500 | 2,500 | 8,000 |
| Discord members | 100 | 500 | 2,000 |

### 17.2 Secondary KPIs

| Metric | Target |
|---|---|
| Average session duration | > 3 minutes |
| Programs compared per user | > 2 |
| Opportunities saved per user | > 3 |
| Community submissions per month | 10+ |
| Newsletter open rate | > 35% |
| Organic search traffic (% of total) | > 40% by month 6 |

### 17.3 Quality Metrics

| Metric | Target |
|---|---|
| Data completeness (avg across programs) | > 65% |
| Page load time (LCP) | < 2.5 seconds |
| Mobile usability score | > 95 |
| Uptime | 99.5%+ |
| Zero critical bugs in production | Always |

---

## 18. Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Data collection takes longer than expected | High | Medium | Prioritize breadth over depth. Launch with 40% completeness and improve. |
| Low initial traffic | Medium | High | Invest in SEO content from day one. Product Hunt launch. Reddit strategy. |
| Supabase free tier limits exceeded | Low (Year 1) | Medium | Monitor usage. Upgrade when revenue justifies it. |
| Solo developer burnout | Medium | Critical | Modular architecture. Automate what possible. Accept imperfection. |
| Competitor launches similar product | Low | Medium | Speed + community + data depth = defensibility. |
| Users don't convert to Tier 2 | Medium | Medium | Make Tier 2 genuinely valuable. Generous free tier builds trust. |
| Community becomes inactive | Medium | Medium | Regular engagement. Weekly content. Genuine value in Discord. |
| Legal issues (data accuracy claims) | Low | High | Clear disclaimers. "Verify with institution" on every page. |

---

## 19. Competitive Landscape

### 19.1 Direct Competitors

| Platform | What They Do | DatumFolio Advantage |
|---|---|---|
| U.S. News / QS | Rank programs by composite score | We don't rank. We structure and compare. Global. Research-focused. |
| Archinect | Architecture jobs + schools | We are research-driven, not profession-driven. Broader disciplines. |
| WikiCFP | Conference CFP aggregator | We curate for design research specifically. Better UX. Integrated with programs. |
| ResearchGate | Researcher networking | We are discipline-specific. Education-first. Not a paper repository. |
| GradCafe | Forum-based grad school discussion | We provide structured data, not forum noise. |

### 19.2 Indirect Competitors

| Platform | Overlap | Differentiation |
|---|---|---|
| LinkedIn | Professional networking | We are academic/research-focused, not corporate. |
| ArchDaily | Architecture media | We don't feature projects or news. Pure research + education. |
| DAAD / Fulbright sites | Fellowship databases | We aggregate across many sources. Design-specific. |

### 19.3 Defensibility (Moat)

1. **Data depth** — 300+ institutions with structured, comparable data. Hard to replicate quickly.
2. **Community** — Discord + newsletter creates switching cost. People don't leave communities.
3. **Curation quality** — Hand-curated by someone who understands the field. Not scraped noise.
4. **Global from day one** — Most competitors are US-only or architecture-only.
5. **Research focus** — Not profession-focused. A distinct positioning nobody occupies.
6. **Network effects** — More users → more submissions → more data → more users.

---

## 20. Long-Term Vision (3-5 Years)

### Year 1: Foundation
- Largest curated design research program database globally
- Active community of 15,000+ researchers and students
- Recognized brand in design education circles
- Sustainable through community trust and data depth

### Year 2: Monetization
- Tier 3 premium features launched
- Institutional subscriptions generating recurring revenue
- 50,000 registered users
- Full-time operation (founder + 1-2 team members)

### Year 3: Authority
- The default starting point for anyone entering design research
- University programs actively maintain their profiles
- Conference organizers submit their own CFPs
- 100,000+ users globally
- Revenue supporting a small team

### Year 5: Network
- The "IEEE of design research" — privately owned, community-driven
- PhD hiring boards connecting institutions with candidates
- Collaboration matchmaking across continents
- Research lab profiles with live project data
- Annual DatumFolio Research Summit (virtual + physical)
- Open data API for researchers
- Expansion into professional development and continuing education
- Self-sustaining flywheel of content, community, and commerce

---

## Appendix A: Technical Decisions Log

| Decision | Choice | Rationale |
|---|---|---|
| Framework | Next.js 15 (App Router) | Full-stack, SSR, API routes, free Vercel deployment |
| Database | Supabase | Free tier, built-in auth, PostgreSQL, RLS |
| Auth | Supabase Auth (email + password) | Simplest, free, secure |
| Map | Mapbox GL JS | Beautiful, customizable, generous free tier |
| Hosting | Vercel | Auto-deploy, free, fast |
| Styling | Tailwind CSS 4 | Utility-first, fast iteration |
| Community | Discord (external) | Free, established, doesn't require building |
| Email | TBD (Resend or Supabase) | Transactional + newsletter needs |
| Analytics | Vercel Analytics or Plausible | Privacy-respecting, free/cheap |
| Repository | Private GitHub | Security, version control |

## Appendix B: Legal Requirements

- Terms of Service
- Privacy Policy (GDPR-conscious)
- Cookie notice (minimal — no tracking cookies)
- Data disclaimer on every program page ("Verify with institution")
- Copyright notice
- Community guidelines (Discord)
- DMCA/takedown process for incorrect data

## Appendix C: Content Calendar Template (Monthly)

| Week | Blog Post | Social | Newsletter | Curation |
|---|---|---|---|---|
| 1 | "How to" guide | 3 posts | Digest | 5 new opportunities |
| 2 | Program comparison | 3 posts | — | Data updates |
| 3 | Researcher spotlight | 3 posts | Digest | 5 new opportunities |
| 4 | Discipline deep-dive | 3 posts | — | Quality review |

---

*Document Version 1.0 — June 2025*
*Author: Asif Hasan Zeshan*
*Status: Startinng Development*

