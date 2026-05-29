# DatumFolio — Technical Specification Document

**Version:** 1.0
**Date:** June 2025
**Author:** Asif Hasan Zeshan
**Status:** Ready for Development
**Reference:** This document implements the requirements defined in [PRD.md](../PRD.md)

---

## Document Purpose and Usage

**This document is:**

- A contract defining what each part of the system does
- A reference to be read at the start of every development session
- Modular enough that changing one section does not invalidate others
- Valid regardless of build order or modifications made during development

**This document is NOT:**

- Implementation code (that comes during build)
- Immutable (you can modify sections as you develop)
- Sequential (you can build features in any order)

**How to use with Claude:**

1. Feed this entire document at the start of every session
2. Additionally feed the per-feature spec relevant to that day's work
3. When you modify something during build, update the relevant section here so future sessions stay accurate

---

## Table of Contents

1. [System Overview](#1-system-overview)
2. [Database Schema](#2-database-schema)
3. [API Contracts](#3-api-contracts)
4. [Component Specifications](#4-component-specifications)
5. [File Responsibility Map](#5-file-responsibility-map)
6. [Design Tokens](#6-design-tokens)
7. [Error Handling Patterns](#7-error-handling-patterns)
8. [Testing Strategy](#8-testing-strategy)
9. [Deployment Checklist](#9-deployment-checklist)
10. [Database Connection Stability](#10-database-connection-stability)
11. [Modification Strategy](#11-modification-strategy)
12. [Changelog](#12-changelog)

---

## 1. System Overview

### 1.1 Platform Summary

DatumFolio is a comprehensive information hub for design researchers providing graduate program search, research opportunity discovery, and community connection across Architecture, Urban Design, Interactive Design, Digital Fine Arts, Human-Centered Design, Product Design, and Participatory Design [1].

### 1.2 Technical Stack

| Layer | Technology | Version | Free Tier Limits |
|-------|-----------|---------|-----------------|
| Framework | Next.js (App Router, TypeScript) | 15.x | Unlimited |
| Styling | Tailwind CSS + CSS Custom Properties | 4.x | Unlimited |
| Database | Supabase (PostgreSQL) | Latest | 500MB, 50k rows, 2GB bandwidth/month |
| Auth | Supabase Auth (Email + Password) | Latest | 50k monthly active users |
| Map | Mapbox GL JS via react-map-gl | 3.x / 7.x | 50k map loads/month |
| Hosting | Vercel | Latest | 100GB bandwidth, serverless functions |
| Email | Resend | Latest | 100 emails/day free |
| Community | Discord (external) | — | Unlimited |
| Analytics | Vercel Analytics | Latest | Free tier included |
| Repository | GitHub (Private) | — | Unlimited private repos |

### 1.3 Architecture Principles

| Principle | Implementation |
|-----------|---------------|
| Modular | Every feature is a self-contained folder with its own components, types, and API routes |
| API-mediated | Frontend NEVER imports Supabase directly. All data flows through /api/ routes |
| Server-first | Pages fetch data on server. Client interactivity only where user action requires it |
| Documented | Every file has a top comment explaining its purpose and dependencies |
| Correctable | Files are small and focused. Replacing one file never breaks another feature |
| Always connected | Database connection uses persistent client with retry logic. No timeout disconnects |

### 1.4 Development Environment

| Tool | Purpose |
|------|---------|
| GitHub Web Editor | All code editing (no local dev) |
| Vercel | Auto-deploy on every commit to main |
| Supabase Dashboard | Database management, auth config, SQL editor |
| Claude | AI-assisted development (fed this spec + feature specs) |

---

## 2. Database Schema

### 2.1 Complete SQL

The following SQL is ready to paste into Supabase SQL Editor as a single execution block. It creates all tables, indexes, triggers, and Row Level Security policies [1].

```sql
-- ============================================
-- DATUMFOLIO DATABASE SCHEMA
-- Version: 1.0
-- Paste this entire block into Supabase SQL Editor
-- ============================================


-- ============================================
-- EXTENSIONS
-- ============================================

CREATE EXTENSION IF NOT EXISTS "uuid-ossp";


-- ============================================
-- TABLE: institutions
-- ============================================

CREATE TABLE institutions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  name TEXT NOT NULL UNIQUE,
  short_name TEXT,
  school_unit TEXT,

  city TEXT NOT NULL,
  state_province TEXT,
  country TEXT NOT NULL,
region TEXT NOT NULL CHECK (
    region IN ('US', 'UK', 'Europe', 'Asia', 'Oceania', 'Other')
  ),
  subregion TEXT CHECK (
    subregion IN ('North America (ex-US)', 'South America', 'Africa', 'Russia')
  ),
  latitude DOUBLE PRECISION NOT NULL,
  longitude DOUBLE PRECISION NOT NULL,

  institution_type TEXT NOT NULL CHECK (
    institution_type IN ('Private', 'Public', 'For-Profit')
  ),

  website_url TEXT,
  logo_url TEXT
);


-- ============================================
-- TABLE: programs
-- ============================================

CREATE TABLE programs (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  institution_id UUID NOT NULL REFERENCES institutions(id) ON DELETE CASCADE,

  program_name TEXT NOT NULL,
  degree_type TEXT NOT NULL CHECK (
    degree_type IN (
      'Master of Architecture',
      'Master of Science',
      'Master of Arts',
      'Master of Fine Arts',
      'Master of Landscape Architecture',
      'Master of Urban Design',
      'Master of Urban Planning',
      'Master of City Planning',
      'Master of Real Estate Development',
      'Master of Construction Management',
      'Master of Design',
      'Post-Professional',
      'PhD',
      'Other'
    )
  ),
  discipline TEXT NOT NULL CHECK (
    discipline IN (
      'Architecture',
      'Urban Design',
      'Interactive Design',
      'Digital Fine Arts',
      'Human-Centered Design',
      'Product Design',
      'Participatory Design',
      'Landscape Architecture',
      'Urban Planning',
      'Construction Management',
      'Real Estate Development',
      'Design Research'
    )
  ),

  duration_months INTEGER,
  credits_required INTEGER,
  thesis_required BOOLEAN,
  accredited BOOLEAN,
  accreditation_body TEXT,
  stem_designated BOOLEAN,
  research_areas TEXT[],
  program_size_approx INTEGER,

  website_url TEXT,
  application_url TEXT,

  data_completeness NUMERIC(3,2) DEFAULT 0.00 NOT NULL CHECK (
    data_completeness >= 0 AND data_completeness <= 1
  ),
  notes TEXT,

  UNIQUE(institution_id, program_name)
);


-- ============================================
-- TABLE: admissions
-- ============================================

CREATE TABLE admissions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  program_id UUID NOT NULL UNIQUE REFERENCES programs(id) ON DELETE CASCADE,

  gre_required BOOLEAN,
  gre_minimum_verbal INTEGER,
  gre_minimum_quant INTEGER,

  portfolio_required BOOLEAN,
  portfolio_format TEXT,

  toefl_minimum INTEGER,
  ielts_minimum NUMERIC(2,1),
  duolingo_minimum INTEGER,

  letters_required INTEGER,
  statement_required BOOLEAN,
  resume_required BOOLEAN,
  interview_required BOOLEAN,

  prior_degree_requirement TEXT,

  application_deadline_fall TEXT,
  application_deadline_spring TEXT,
  early_deadline TEXT,

  acceptance_rate NUMERIC(4,3),
  avg_cohort_size INTEGER
);


-- ============================================
-- TABLE: costs
-- ============================================

CREATE TABLE costs (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  program_id UUID NOT NULL UNIQUE REFERENCES programs(id) ON DELETE CASCADE,

  tuition_resident INTEGER,
  tuition_nonresident INTEGER,
  tuition_per_credit INTEGER,
  tuition_currency TEXT DEFAULT 'USD',
  tuition_notes TEXT,

  fees_annual INTEGER,
  estimated_living_cost INTEGER,
  estimated_total_cost INTEGER,

  financial_aid_available BOOLEAN,
  assistantships_available BOOLEAN,
  fellowships_available BOOLEAN,
  avg_scholarship_amount INTEGER,
  funding_notes TEXT,

  cost_year TEXT
);


-- ============================================
-- TABLE: metadata
-- ============================================

CREATE TABLE metadata (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  program_id UUID NOT NULL UNIQUE REFERENCES programs(id) ON DELETE CASCADE,

  source_url TEXT,
  source_type TEXT CHECK (
    source_type IN (
      'Institution Website', 'NAAB', 'RIBA', 'ACSP',
      'LAAB', 'Manual Entry', 'Community Submission', 'Other'
    )
  ),

  last_verified DATE,
  verified_by TEXT,

  data_quality TEXT CHECK (
    data_quality IN ('Verified', 'Current', 'Aging', 'Unverified')
  ) DEFAULT 'Unverified',

  missing_fields TEXT[],
  editor_notes TEXT
);


-- ============================================
-- TABLE: conferences
-- ============================================

CREATE TABLE conferences (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  name TEXT NOT NULL,
  organizing_body TEXT,
  theme TEXT,
  disciplines TEXT[],

  cfp_deadline DATE,
  conference_start_date DATE,
  conference_end_date DATE,

  city TEXT,
  country TEXT,
  is_virtual BOOLEAN DEFAULT FALSE,

  website_url TEXT NOT NULL,
  submission_format TEXT[],
  registration_cost TEXT,

  status TEXT DEFAULT 'CFP Open' CHECK (
    status IN ('CFP Open', 'CFP Closed', 'Upcoming', 'Past')
  ),

  featured BOOLEAN DEFAULT FALSE,
  submitted_by UUID,
  approved BOOLEAN DEFAULT FALSE
);


-- ============================================
-- TABLE: journal_calls
-- ============================================

CREATE TABLE journal_calls (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  journal_name TEXT NOT NULL,
  publisher TEXT,
  special_issue_theme TEXT,
  disciplines TEXT[],

  submission_deadline DATE NOT NULL,
  expected_publication TEXT,

  peer_review_type TEXT CHECK (
    peer_review_type IN ('blind', 'double-blind', 'open', 'other')
  ),
  open_access BOOLEAN,

  website_url TEXT NOT NULL,

  status TEXT DEFAULT 'Open' CHECK (
    status IN ('Open', 'Closing Soon', 'Closed')
  ),

  featured BOOLEAN DEFAULT FALSE,
  submitted_by UUID,
  approved BOOLEAN DEFAULT FALSE
);


-- ============================================
-- TABLE: fellowships
-- ============================================

CREATE TABLE fellowships (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  name TEXT NOT NULL,
  granting_organization TEXT NOT NULL,
  amount_description TEXT,
  amount_min INTEGER,
  amount_max INTEGER,
  currency TEXT DEFAULT 'USD',

  disciplines TEXT[],
  career_stage TEXT[],
  geographic_eligibility TEXT,

  application_deadline DATE,
  duration TEXT,

  website_url TEXT NOT NULL,

  status TEXT DEFAULT 'Open' CHECK (
    status IN ('Open', 'Closing Soon', 'Closed')
  ),

  featured BOOLEAN DEFAULT FALSE,
  submitted_by UUID,
  approved BOOLEAN DEFAULT FALSE
);


-- ============================================
-- TABLE: innovation_opportunities
-- ============================================

CREATE TABLE innovation_opportunities (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  title TEXT NOT NULL,
  institution TEXT,
  opportunity_type TEXT NOT NULL CHECK (
    opportunity_type IN (
      'PhD', 'Postdoc', 'Research Assistant', 'Faculty', 'Industry Research'
    )
  ),
  disciplines TEXT[],

  city TEXT,
  country TEXT,
  is_remote BOOLEAN DEFAULT FALSE,

  duration TEXT,
  funding_status TEXT CHECK (
    funding_status IN ('Funded', 'Partially Funded', 'Unfunded')
  ),

  application_deadline DATE,
  website_url TEXT NOT NULL,
  contact_info TEXT,
  description TEXT,

  status TEXT DEFAULT 'Open' CHECK (
    status IN ('Open', 'Closed')
  ),

  featured BOOLEAN DEFAULT FALSE,
  submitted_by UUID,
  approved BOOLEAN DEFAULT FALSE
);


-- ============================================
-- TABLE: profiles
-- ============================================

CREATE TABLE profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  full_name TEXT,
  disciplines TEXT[],
  career_stage TEXT CHECK (
    career_stage IN ('student', 'phd', 'postdoc', 'faculty', 'professional')
  ),
  institution TEXT,
  country TEXT,
 newsletter_subscribed BOOLEAN DEFAULT FALSE NOT NULL
);


-- ============================================
-- TABLE: saved_programs
-- ============================================

CREATE TABLE saved_programs (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  program_id UUID NOT NULL REFERENCES programs(id) ON DELETE CASCADE,
  notes TEXT,

  UNIQUE(user_id, program_id)
);


-- ============================================
-- TABLE: saved_opportunities
-- ============================================

CREATE TABLE saved_opportunities (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  opportunity_type TEXT NOT NULL CHECK (
    opportunity_type IN ('conference', 'journal', 'fellowship', 'innovation')
  ),
  opportunity_id UUID NOT NULL,

  UNIQUE(user_id, opportunity_type, opportunity_id)
);


-- ============================================
-- TABLE: submissions
-- ============================================

CREATE TABLE submissions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  user_id UUID NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
submission_type TEXT NOT NULL CHECK (
    submission_type IN ('conference', 'journal', 'fellowship', 'innovation')
  ),

  title TEXT NOT NULL,
  description TEXT,
  deadline DATE,
  url TEXT NOT NULL,
  additional_data JSONB,

  status TEXT DEFAULT 'pending' CHECK (
    status IN ('pending', 'approved', 'rejected', 'needs_info')
  ),
  reviewer_notes TEXT,
  reviewed_at TIMESTAMPTZ
);


-- ============================================
-- TABLE: newsletter_signups
-- ============================================

CREATE TABLE newsletter_signups (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL,

  email TEXT NOT NULL UNIQUE,
  consent BOOLEAN NOT NULL DEFAULT FALSE,
  source TEXT,
  disciplines_interest TEXT[]
);

-- ============================================
-- UNIFIED NEWSLETTER RECIPIENT VIEW
-- Single source of truth for who receives the newsletter.
-- Combines opted-in account holders (via auth email) and
-- standalone subscribers. Send job reads from this view only.
-- ============================================

CREATE OR REPLACE VIEW newsletter_recipients AS
  SELECT u.email, 'account' AS origin
  FROM auth.users u
  JOIN profiles p ON p.id = u.id
  WHERE p.newsletter_subscribed = TRUE
  UNION
  SELECT n.email, 'standalone' AS origin
  FROM newsletter_signups n
  WHERE n.consent = TRUE;


-- ============================================
-- INDEXES
-- ============================================

CREATE INDEX idx_institutions_geo ON institutions (latitude, longitude);
CREATE INDEX idx_institutions_country ON institutions (country);
CREATE INDEX idx_institutions_region ON institutions (region);

CREATE INDEX idx_programs_institution ON programs (institution_id);
CREATE INDEX idx_programs_discipline ON programs (discipline);
CREATE INDEX idx_programs_degree ON programs (degree_type);
CREATE INDEX idx_programs_completeness ON programs (data_completeness);

CREATE INDEX idx_admissions_program ON admissions (program_id);
CREATE INDEX idx_costs_program ON costs (program_id);
CREATE INDEX idx_metadata_program ON metadata (program_id);

CREATE INDEX idx_conferences_status ON conferences (status);
CREATE INDEX idx_conferences_deadline ON conferences (cfp_deadline);
CREATE INDEX idx_journal_calls_status ON journal_calls (status);
CREATE INDEX idx_journal_calls_deadline ON journal_calls (submission_deadline);
CREATE INDEX idx_fellowships_status ON fellowships (status);
CREATE INDEX idx_fellowships_deadline ON fellowships (application_deadline);
CREATE INDEX idx_opportunities_status ON innovation_opportunities (status);
CREATE INDEX idx_opportunities_type ON innovation_opportunities (opportunity_type);

CREATE INDEX idx_saved_programs_user ON saved_programs (user_id);
CREATE INDEX idx_saved_opportunities_user ON saved_opportunities (user_id);
CREATE INDEX idx_submissions_status ON submissions (status);
CREATE INDEX idx_submissions_user ON submissions (user_id);


-- ============================================
-- AUTO-UPDATE TRIGGERS
-- ============================================

CREATE OR REPLACE FUNCTION update_modified_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER set_updated_at_institutions BEFORE UPDATE ON institutions FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_programs BEFORE UPDATE ON programs FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_admissions BEFORE UPDATE ON admissions FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_costs BEFORE UPDATE ON costs FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_metadata BEFORE UPDATE ON metadata FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_conferences BEFORE UPDATE ON conferences FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_journal_calls BEFORE UPDATE ON journal_calls FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_fellowships BEFORE UPDATE ON fellowships FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_opportunities BEFORE UPDATE ON innovation_opportunities FOR EACH ROW EXECUTE FUNCTION update_modified_column();
CREATE TRIGGER set_updated_at_profiles BEFORE UPDATE ON profiles FOR EACH ROW EXECUTE FUNCTION update_modified_column();


-- ============================================
-- ROW LEVEL SECURITY
-- ============================================

ALTER TABLE institutions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read institutions" ON institutions FOR SELECT TO anon, authenticated USING (true);
CREATE POLICY "Admin write institutions" ON institutions FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE programs ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read programs" ON programs FOR SELECT TO anon, authenticated USING (true);
CREATE POLICY "Admin write programs" ON programs FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE admissions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read admissions" ON admissions FOR SELECT TO anon, authenticated USING (true);
CREATE POLICY "Admin write admissions" ON admissions FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE costs ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read costs" ON costs FOR SELECT TO anon, authenticated USING (true);
CREATE POLICY "Admin write costs" ON costs FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE metadata ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read metadata" ON metadata FOR SELECT TO anon, authenticated USING (true);
CREATE POLICY "Admin write metadata" ON metadata FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE conferences ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Auth read conferences" ON conferences FOR SELECT TO authenticated USING (approved = true);
CREATE POLICY "Admin write conferences" ON conferences FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE journal_calls ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Auth read journal_calls" ON journal_calls FOR SELECT TO authenticated USING (approved = true);
CREATE POLICY "Admin write journal_calls" ON journal_calls FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE fellowships ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Auth read fellowships" ON fellowships FOR SELECT TO authenticated USING (approved = true);
CREATE POLICY "Admin write fellowships" ON fellowships FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE innovation_opportunities ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Auth read opportunities" ON innovation_opportunities FOR SELECT TO authenticated USING (approved = true);
CREATE POLICY "Admin write opportunities" ON innovation_opportunities FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users read own profile" ON profiles FOR SELECT TO authenticated USING (auth.uid() = id);
CREATE POLICY "Users update own profile" ON profiles FOR UPDATE TO authenticated USING (auth.uid() = id) WITH CHECK (auth.uid() = id);
CREATE POLICY "Users insert own profile" ON profiles FOR INSERT TO authenticated WITH CHECK (auth.uid() = id);
CREATE POLICY "Admin profiles" ON profiles FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE saved_programs ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users own saved_programs" ON saved_programs FOR ALL TO authenticated USING (auth.uid() = user_id) WITH CHECK (auth.uid() = user_id);
CREATE POLICY "Admin saved_programs" ON saved_programs FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE saved_opportunities ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users own saved_opportunities" ON saved_opportunities FOR ALL TO authenticated USING (auth.uid() = user_id) WITH CHECK (auth.uid() = user_id);
CREATE POLICY "Admin saved_opportunities" ON saved_opportunities FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE submissions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users insert submissions" ON submissions FOR INSERT TO authenticated WITH CHECK (auth.uid() = user_id);
CREATE POLICY "Users read own submissions" ON submissions FOR SELECT TO authenticated USING (auth.uid() = user_id);
CREATE POLICY "Admin submissions" ON submissions FOR ALL TO service_role USING (true) WITH CHECK (true);

ALTER TABLE newsletter_signups ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Anyone can subscribe" ON newsletter_signups FOR INSERT TO anon, authenticated WITH CHECK (true);
CREATE POLICY "Admin newsletter" ON newsletter_signups FOR ALL TO service_role USING (true) WITH CHECK (true);


-- ============================================
-- AUTO-CREATE PROFILE ON SIGNUP
-- ============================================

CREATE OR REPLACE FUNCTION handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO public.profiles (id)
  VALUES (NEW.id);
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION handle_new_user();


-- ============================================
-- PUBLIC TEASER VIEWS (anonymous-readable, SEO)
-- Expose ONLY title + deadline + status for approved items.
-- Full detail remains gated to authenticated users via base tables.
-- ============================================

CREATE OR REPLACE VIEW public_conferences AS
  SELECT id, name AS title, cfp_deadline AS deadline, status
  FROM conferences WHERE approved = TRUE;

CREATE OR REPLACE VIEW public_journal_calls AS
  SELECT id, journal_name AS title, submission_deadline AS deadline, status
  FROM journal_calls WHERE approved = TRUE;

CREATE OR REPLACE VIEW public_fellowships AS
  SELECT id, name AS title, application_deadline AS deadline, status
  FROM fellowships WHERE approved = TRUE;

CREATE OR REPLACE VIEW public_innovation_opportunities AS
  SELECT id, title, application_deadline AS deadline, status
  FROM innovation_opportunities WHERE approved = TRUE;

-- Views inherit RLS from base tables by default. To serve anonymous
-- readers, these views are defined with security_invoker = false
-- (run with definer privileges) so they bypass base-table RLS but
-- expose only the safe columns selected above.
ALTER VIEW public_conferences SET (security_invoker = false);
ALTER VIEW public_journal_calls SET (security_invoker = false);
ALTER VIEW public_fellowships SET (security_invoker = false);
ALTER VIEW public_innovation_opportunities SET (security_invoker = false);

GRANT SELECT ON public_conferences TO anon, authenticated;
GRANT SELECT ON public_journal_calls TO anon, authenticated;
GRANT SELECT ON public_fellowships TO anon, authenticated;
GRANT SELECT ON public_innovation_opportunities TO anon, authenticated;
```

---

## 3. API Contracts

### 3.1 Programs

| Endpoint | Method | Auth Required | Purpose |
|----------|--------|---------------|---------|
| /api/programs | GET | No | List programs with filters |
| /api/programs/[id] | GET | No | Single program with full detail |

**GET /api/programs query parameters:**

| Param | Type | Example |
|-------|------|---------|
| discipline | CSV | Architecture,Urban Design |
| country | CSV | US,UK,DE |
| region | CSV | US,EU,Asia-Pacific |
| degree_type | CSV | Master of Architecture,PhD |
| institution_type | CSV | Private,Public |
| tuition_max | number | 50000 |
| gre_required | boolean | true or false |
| portfolio_required | boolean | true or false |
| stem_only | boolean | true |
| accredited_only | boolean | true |
| search | string | MIT |
| sort | string | name, tuition, duration, completeness |
| order | string | asc or desc |

**Response shape:**

```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "program_name": "string",
      "degree_type": "string",
      "discipline": "string",
      "duration_months": "number or null",
      "stem_designated": "boolean or null",
      "accredited": "boolean or null",
      "accreditation_body": "string or null",
      "research_areas": ["string"],
      "data_completeness": "number",
      "institution": {
        "id": "uuid",
        "name": "string",
        "short_name": "string or null",
        "city": "string",
        "country": "string",
        "region": "string",
        "latitude": "number",
        "longitude": "number",
        "institution_type": "string"
      },
      "admissions": {
        "gre_required": "boolean or null",
        "portfolio_required": "boolean or null",
        "application_deadline_fall": "string or null",
        "acceptance_rate": "number or null"
      },
      "costs": {
        "tuition_nonresident": "number or null",
        "tuition_currency": "string",
        "financial_aid_available": "boolean or null"
      }
    }
  ],
  "meta": {
    "total": "number",
    "returned": "number"
  }
}
```

### 3.2 Opportunities

| Endpoint | Method | Auth Required | Purpose |
|----------|--------|---------------|---------|
| /api/opportunities/conferences | GET | Yes | List approved conferences |
| /api/opportunities/conferences/[id] | GET | Yes | Conference detail |
| /api/opportunities/journals | GET | Yes | List approved journal calls |
| /api/opportunities/journals/[id] | GET | Yes | Journal call detail |
| /api/opportunities/fellowships | GET | Yes | List approved fellowships |
| /api/opportunities/fellowships/[id] | GET | Yes | Fellowship detail |
| /api/opportunities/innovation | GET | Yes | List approved opportunities |
| /api/opportunities/innovation/[id] | GET | Yes | Opportunity detail |
| /api/opportunities/conferences/public | GET | No | Teaser list (title + deadline + status) for SEO |
| /api/opportunities/journals/public | GET | No | Teaser list (title + deadline + status) for SEO |
| /api/opportunities/fellowships/public | GET | No | Teaser list (title + deadline + status) for SEO |
| /api/opportunities/innovation/public | GET | No | Teaser list (title + deadline + status) for SEO |

**Common query params for all opportunity endpoints:**

| Param | Type | Example |
|-------|------|---------|
| discipline | CSV | Architecture,Urban Design |
| status | string | Open or Closing Soon |
| country | string | US |

### 3.3 User Actions

| Endpoint | Method | Auth Required | Purpose |
|----------|--------|---------------|---------|
| /api/user/profile | GET | Yes | Get own profile |
| /api/user/profile | PATCH | Yes | Update own profile |
| /api/user/saved-programs | GET | Yes | Get saved programs |
| /api/user/saved-programs | POST | Yes | Save a program |
| /api/user/saved-programs/[id] | DELETE | Yes | Remove saved program |
| /api/user/saved-opportunities | GET | Yes | Get saved opportunities |
| /api/user/saved-opportunities | POST | Yes | Save an opportunity |
| /api/user/saved-opportunities/[id] | DELETE | Yes | Remove saved opportunity |

### 3.4 Community

| Endpoint | Method | Auth Required | Purpose |
|----------|--------|---------------|---------|
| /api/submit | POST | Yes | Submit opportunity for moderation |
| /api/subscribe | POST | No | Newsletter email signup |

**POST /api/submit body:**

```json
{
"submission_type": "conference or journal or fellowship or innovation",
  "title": "string",
  "description": "string",
  "deadline": "date string",
  "url": "string",
  "additional_data": {}
}
```

### 3.5 Admin

| Endpoint | Method | Auth Required | Purpose |
|----------|--------|---------------|---------|
| /api/admin/export | GET | Admin key | CSV export of signups and submissions |

---

## 4. Component Specifications

### 4.1 UI Primitives

| Component | Props | Behavior |
|-----------|-------|----------|
| Button | variant: primary/secondary/ghost/danger, size: sm/md/lg, disabled, loading, onClick | Primary uses Venetian Red. Ghost has no background. Loading shows spinner. |
| Input | type: text/email/password, label, error, placeholder, value, onChange | Label above. Error below in red. Focus ring on interaction. |
| Select | label, options, value, onChange, placeholder | Custom dropdown arrow. Same styling as Input. |
| Card | variant: standard/hoverable/featured, padding: sm/md/lg | Featured has accent left border. Hoverable lifts on hover. |
| Badge | variant: default/accent/success/warning/info, size: sm/md | Small inline label. Accent uses Venetian Red. |
| DataPoint | label, value, unit, highlight | Label in caps above, value in mono below. |
| DataCompletenessBadge | value: 0-1, size: sm/md, showLabel | Bar + percentage + quality label. Prefixed with "Data". |
| Modal | open, onClose, title, children | Centered overlay. Click backdrop or X to close. |
| Toast | message, type: success/error/info, duration | Bottom-right. Auto-dismiss. |
| Skeleton | width, height, rounded | Animated pulse placeholder. |
| Section | padding: sm/md/lg/xl, border: none/top/bottom/both | Semantic section wrapper. |
| Container | width: page/content/prose/narrow/detail | Width-constrained wrapper. |
| Stack | gap, direction: vertical/horizontal, align, justify | Flex layout primitive. |
| Grid | columns: 1/2/3/4/auto, gap | Responsive CSS grid. |
| Divider | strong, spacing | Horizontal rule. |

### 4.2 Feature Components

| Component | Location | Purpose | Dependencies |
|-----------|----------|---------|--------------|
| Header | components/layout/ | Fixed top navigation with auth state | use-auth hook |
| Footer | components/layout/ | Site footer | None |
| AuthGuard | components/layout/ | Wraps Tier 2 pages. Shows blurred preview with signup overlay. | use-auth hook |
| ProgramCard | components/programs/ | Card displaying program summary | DataCompletenessBadge, Badge |
| ProgramGrid | components/programs/ | Grid/list layout with view toggle | ProgramCard |
| ProgramDirectory | components/programs/ | Wraps filters + grid. Manages filter state. | FilterPanel, ProgramGrid |
| FilterPanel | components/programs/ | Search + filter dropdowns | None |
| CompareButton | components/programs/ | Toggle program in compare list | use-compare hook |
| CompareBar | components/programs/ | Floating bottom bar when programs selected | use-compare hook |
| CompareView | components/programs/ | Side-by-side comparison table | None |
| ShortlistButton | components/programs/ | Save/unsave program | API call (requires auth) |
| ProgramMap | components/map/ | Mapbox GL map with clustered markers | react-map-gl |
| ExploreView | components/map/ | Map + sidebar layout | ProgramMap |
| OpportunityCard | components/opportunities/ | Card for any opportunity type. On the dashboard, if the referenced opportunity has been deleted by an admin, renders a "Saved item no longer exists" placeholder instead of crashing. | Badge, DeadlineIndicator |
| OpportunityTabs | components/opportunities/ | Tab navigation between opportunity types | None |
| DeadlineIndicator | components/opportunities/ | Days remaining countdown with urgency color | None |
| StatusBadge | components/opportunities/ | Open/Closing Soon/Closed badge | Badge |
| LoginForm | components/auth/ | Email + password login | Supabase Auth |
| SignupForm | components/auth/ | Email + password registration | Supabase Auth |
| SubmissionForm | components/community/ | Submit opportunity for moderation | API call (requires auth) |
| NewsletterSignup | components/community/ | Email capture for newsletter | API call |

---

## 5. File Responsibility Map

### 5.1 Single Responsibility Rule

| File | Single Responsibility |
|------|----------------------|
| src/lib/supabase/server.ts | Creates server-side Supabase client. Used only in API routes. |
| src/lib/supabase/client.ts | Creates browser-side Supabase client. Used only for auth state. |
| src/lib/api/response.ts | Standardized API response helpers (apiSuccess, apiError, parsers). |
| src/lib/api/client.ts | Frontend fetch functions that call /api/ routes. |
| src/lib/hooks/use-auth.ts | React hook for authentication state. |
| src/lib/hooks/use-compare.ts | React hook for compare selections (localStorage). |
| Each API route file | One endpoint. Validates input, calls Supabase, returns formatted response. |
| Each page file | Fetches data, passes to components. Minimal logic. |
| Each component file | Renders UI. Receives data via props. No direct DB calls. |

### 5.2 Dependency Rules

| Layer | Can Import From | Cannot Import From |
|-------|----------------|-------------------|
| Pages (app/) | Components, lib, types | Nothing else |
| API Routes (app/api/) | lib/supabase/server, lib/api/response, types | Components, client code |
| Components | Other components, lib/hooks, lib/api/client, types | lib/supabase/*, API routes |
| Hooks (lib/hooks/) | lib/api/client, types | Components, pages, supabase |
| API client (lib/api/client.ts) | types | Everything else |

---

## 6. Design Tokens

### 6.1 Colors

```css
:root {
  --color-bg: #FAFAF9;
  --color-surface: #FFFFFF;
  --color-surface-muted: #F5F5F4;
  --color-ink-primary: #1C1917;
  --color-ink-secondary: #57534E;
  --color-ink-tertiary: #A8A29E;
  --color-line: #E7E5E4;
  --color-line-strong: #D6D3D1;
  --color-accent: #C23B22;
  --color-accent-hover: #A83220;
  --color-accent-light: #FDF2F0;
  --color-success: #15803D;
  --color-warning: #B45309;
  --color-info: #1D4ED8;
  --color-error: #B91C1C;
}
```

### 6.2 Typography

```css
:root {
  --font-sans: "Inter Variable", "Inter", -apple-system, sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", "Fira Code", monospace;
  --font-display: "Instrument Serif", Georgia, serif;

  --text-xs: 0.6875rem;
  --text-sm: 0.8125rem;
  --text-base: 0.9375rem;
  --text-lg: 1.125rem;
  --text-xl: 1.3125rem;
  --text-2xl: 1.75rem;
  --text-3xl: 2.25rem;
  --text-4xl: 3rem;
}
```

### 6.3 Spacing

```css
:root {
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-10: 2.5rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-20: 5rem;
  --space-24: 6rem;
}
```

### 6.4 Layout

```css
:root {
  --max-width-page: 1440px;
  --max-width-content: 1200px;
  --max-width-prose: 720px;
  --max-width-narrow: 540px;
  --max-width-detail: 860px;
  --header-height: 3.5rem;
}
```

---

## 7. Error Handling Patterns

### 7.1 API Route Pattern

```typescript
try {
  // Validate input
  // Query database
  // Return apiSuccess(data, meta)
} catch (err) {
  console.error("Context:", err);
  return apiError("User-friendly message", statusCode);
}
```

### 7.2 Supabase Error Codes

| Code | Meaning | Response |
|------|---------|----------|
| PGRST116 | Row not found | 404 Not found |
| 23505 | Unique constraint violation | 409 Already exists |
| 42501 | RLS policy violation | 403 Forbidden |
| Other | Unexpected database error | 500 Database error |

### 7.3 Auth Error Pattern

```typescript
const { data: { user } } = await supabase.auth.getUser();
if (!user) return apiError("Authentication required", 401);
```

### 7.4 Client-Side Error Pattern

```typescript
try {
  const result = await fetchPrograms(filters);
  setData(result.data);
} catch (err) {
  showToast({ message: "Failed to load programs", type: "error" });
}
```

---

## 8. Testing Strategy

### 8.1 Manual Testing Checklist (Per Feature)

| Check | Description |
|-------|-------------|
| Happy path | Core feature works as expected |
| Empty state | Component handles zero data gracefully |
| Error state | Component handles API failure gracefully |
| Auth gate | Tier 2 content blocked for unauthenticated users |
| Blurred preview | Unauthenticated users see teased content with signup overlay |
| Mobile | Layout works on 375px width |
| Performance | Page loads in under 3 seconds |
| Data integrity | Saved data persists across sessions |

### 8.2 API Testing

Test each endpoint via browser URL bar (GET) or client:

| Test | Expected |
|------|----------|
| /api/programs | Returns all programs |
| /api/programs?discipline=Architecture | Filtered results |
| /api/programs/[valid-id] | Returns single program detail |
| /api/programs/invalid-id | Returns 400 error |
| /api/opportunities/conferences (no auth) | Returns 401 |
| /api/opportunities/conferences (with auth) | Returns approved conferences |

### 8.3 Database Testing

After every schema change:

1. Run SELECT on each table to confirm structure
2. Test INSERT with sample data
3. Verify RLS with anon key (should only read public tables)
4. Verify RLS with authenticated user (should see opportunity tables)
5. Verify users cannot read other users saved items

---

## 9. Deployment Checklist

### 9.1 Initial Setup

- [ ] Create private GitHub repository
- [ ] Create Supabase project
- [ ] Enable Supabase Auth (Email + Password)
- [ ] Configure email verification as required
- [ ] Run full database schema SQL
- [ ] Create Vercel project linked to GitHub
- [ ] Add all environment variables to Vercel
- [ ] Create Mapbox account and token
- [ ] Verify auto-deploy works (commit triggers build)
- [ ] Set up daily cron ping to prevent Supabase pause

### 9.2 Environment Variables

| Variable | Source |
|----------|--------|
| NEXT_PUBLIC_SUPABASE_URL | Supabase Settings → API |
| NEXT_PUBLIC_SUPABASE_ANON_KEY | Supabase Settings → API |
| SUPABASE_SERVICE_ROLE_KEY | Supabase Settings → API (keep secret) |
| NEXT_PUBLIC_MAPBOX_TOKEN | Mapbox Account → Access Tokens |
| RESEND_API_KEY | Resend dashboard (when email set up) |
| ADMIN_SECRET | You choose (for protected admin routes) |

### 9.3 Per-Feature Deploy

- [ ] Code committed to main branch
- [ ] Vercel build succeeds (green checkmark)
- [ ] Feature works on production URL
- [ ] No console errors in browser
- [ ] Mobile layout verified
- [ ] Auth gates working (if applicable)
- [ ] Blurred preview renders for unauthenticated users on Tier 2 content

---

## 10. Database Connection Stability

### 10.1 Why Connections Drop

The ArchGrad Navigator project experienced disconnections because Supabase free tier pauses projects after 7 days of inactivity [1]. Additionally, serverless functions on Vercel are stateless — there is no persistent connection.

### 10.2 Prevention Strategy

| Strategy | Implementation |
|----------|---------------|
| Stateless clients | Every API request creates a fresh Supabase client. No connection pooling. |
| Daily ping | Set up free cron (cron-job.org) to hit /api/programs once daily. Prevents 7-day pause. |
| Error handling | If Supabase returns connection error, client shows graceful "Service temporarily unavailable" message. |
| No persistent sessions | Each request is independent. Server client does not persist state between calls. |

### 10.3 Server Client Configuration

```typescript
export function createServerClient() {
  return createClient(url, key, {
    auth: {
      persistSession: false,
      autoRefreshToken: false,
    },
    db: {
      schema: 'public',
    },
    global: {
      headers: {
        'x-client-info': 'datumfolio-server',
      },
    },
  });
}
```

### 10.4 If Database Pauses

1. Go to Supabase dashboard
2. Click "Restore" on the paused project
3. Wait approximately 2 minutes
4. Site works immediately after restoration

---

## 11. Modification Strategy

### 11.1 How to Modify This Spec

When you change something during development:

1. Update the relevant section in this document
2. Do NOT rewrite unrelated sections — they remain valid
3. Add a note in Section 12 (Changelog) describing what changed and when

### 11.2 Safe Changes (No Cascading Impact)

| Change | What to Update |
|--------|----------------|
| Add a new database table | Add to Section 2 schema |
| Add a new API endpoint | Add to Section 3 contracts |
| Change component props | Update Section 4 only |
| Add a new page | No spec update needed |
| Change color values | Update Section 6 tokens |
| Add new RLS policy | Add to Section 2 RLS block |

### 11.3 Cascading Changes (Requires Multiple Updates)

| Change | Sections Affected |
|--------|-------------------|
| Rename a database table | Schema, API routes, types, components |
| Change auth provider | Auth config, hooks, forms, Supabase setup |
| Change from API routes to direct Supabase | All data fetching (not recommended) |
| Add a new user role/tier | RLS policies, auth guard, API auth checks |

### 11.4 Per-Feature Spec Files

In addition to this master document, create individual feature spec files for daily Claude sessions:

| File | Content |
|------|---------|
| docs/specs/feature-programs.md | Everything about the graduate program feature |
| docs/specs/feature-auth.md | Everything about authentication and user accounts |
| docs/specs/feature-opportunities.md | Everything about research opportunities |
| docs/specs/feature-compare.md | Everything about the comparison tool |
| docs/specs/feature-map.md | Everything about the interactive map |
| docs/specs/feature-community.md | Everything about submissions and newsletter |
| docs/specs/feature-admin.md | Everything about admin and moderation |

Each feature spec file should contain:

- What the feature does (from PRD)
- Which database tables it uses
- Which API endpoints it calls
- Which components it renders
- How it handles errors
- How it interacts with auth

---

## 12. Changelog

| Date | Section | Change |
|------|---------|--------|
| June 2025 | All | Initial specification created |

---

*DatumFolio Technical Specification v1.0 — June 2025*
*Status: Ready for Development*
