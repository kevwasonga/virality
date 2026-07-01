# EduOS Growth Intelligence Platform (GIP) — Implementation Guideline

**Project:** EduOS Growth Intelligence Platform (GIP)  
**Institution:** ICS Technical College  
**Powered by:** YohPal Brain  
**Purpose:** Autonomous 24/7 student acquisition engine — converting visitors into enquiries, enquiries into applications, applications into admitted students, and students into revenue.

---

## Platform Architecture Overview

```
INTERNET (Search · Social · Ads · WhatsApp · Referrals)
        ↓
EduOS Growth Intelligence Platform (GIP)
        ↓
   Website (Presentation & Conversion Layer)
        ↓
   Smart Prospectus · Identity API
        ↓
   Admissions CRM → ICS Live
        ↓
   Smart Lecturer · AI E-Library · YohPal Brain
        ↓
   Employability Engine
```

**Three Platform Layers:**
- **Layer 1 — Growth Layer:** GIP + Website (acquire students)
- **Layer 2 — Academic Layer:** Smart Prospectus, Identity, ICS Live, Smart Lecturer, AI E-Library, YohPal Brain (teach students)
- **Layer 3 — Outcomes Layer:** Employability, Alumni, Partner Portal, Internships (student success)

---

## Ownership Rules

| Concern | Owner |
|---|---|
| Presentation & conversion | Website |
| Acquisition intelligence & campaign orchestration | GIP |
| Course, fees, intake, eligibility truth | Smart Prospectus |
| User identity & authentication | Identity API |
| Admissions CRM, enrollment, academic lifecycle | ICS Live |
| AI reasoning & LLM gateway | YohPal Brain |
| Learning resources | AI E-Library |

---

## Feature Index

1. [Landing Page](#1-landing-page)
2. [Lead Capture Engine](#2-lead-capture-engine)
3. [AI Career Scanner](#3-ai-career-scanner)
4. [Viral Referral Engine](#4-viral-referral-engine)
5. [AI Lead Scoring Engine](#5-ai-lead-scoring-engine)
6. [Campaign Attribution & Tracking](#6-campaign-attribution--tracking)
7. [Admissions Command Center (Dashboard)](#7-admissions-command-center-dashboard)
8. [GEO / AEO Engine](#8-geo--aeo-engine)
9. [AI Search Authority Engine](#9-ai-search-authority-engine)
10. [Autonomous PPC & Campaign Experiment Engine](#10-autonomous-ppc--campaign-experiment-engine)
11. [Predictive Demand & Competitor Intelligence Engine](#11-predictive-demand--competitor-intelligence-engine)
12. [AI Content Factory](#12-ai-content-factory)
13. [Autonomous Outreach & Nurture Engine](#13-autonomous-outreach--nurture-engine)
14. [Growth Autopilot Governance & Safety Engine](#14-growth-autopilot-governance--safety-engine)
15. [CRM Sync & Revenue Attribution](#15-crm-sync--revenue-attribution)
16. [Multi-Agent Growth Swarm](#16-multi-agent-growth-swarm)
17. [Production Hardening & Certification](#17-production-hardening--certification)
18. [Controlled Admissions Pilot](#18-controlled-admissions-pilot)

---

## 1. Landing Page

**Purpose:** The primary conversion surface. Turns visitors from Google Search, TikTok, Facebook, WhatsApp, and referral links into leads. Must be fast, mobile-first, and SEO/AEO-ready.

### 1.1 Page Structure & Layout
- 1.1.1 Hero section with headline, subheadline, and primary CTA button ("Apply Now" / "Get Course Guidance")
- 1.1.2 Trust bar — accreditation badges, student count, years established
- 1.1.3 Course highlight cards (top 5–6 courses: ICT, AI, Computer Applications, Business, CPA/KASNEB)
- 1.1.4 "Why ICS" section — practical learning, flexible study modes, career outcomes
- 1.1.5 Testimonial / social proof section (student quotes, photos, video thumbnails)
- 1.1.6 Career pathway preview (where this course can take you)
- 1.1.7 Footer with campus addresses, contacts, social links, and privacy policy

### 1.2 Lead Capture Widget (inline / floating)
- 1.2.1 Inline form embedded in hero section — fields: Full Name, Phone, Interest/Course, Study Mode
- 1.2.2 Floating sticky CTA button that opens a modal lead form on scroll
- 1.2.3 WhatsApp CTA button linking to admissions WhatsApp with pre-filled message
- 1.2.4 Form validation — required fields: Full Name, Phone
- 1.2.5 Source attribution auto-capture from URL query params (`utm_source`, `utm_campaign`, `ref`)
- 1.2.6 On submission: POST to `/api/virality/leads`, show success confirmation, trigger score API call
- 1.2.7 Referral code detection from URL (`?ref=CODE`) and pre-population into hidden form field

### 1.3 Course Funnel Entry Points
- 1.3.1 Each course card links to its dedicated course landing page (GEO/AEO page)
- 1.3.2 "Find My Course" CTA routes to AI Career Scanner
- 1.3.3 Course filter/search bar for quick navigation

### 1.4 Viral Share Mechanics
- 1.4.1 "Share & Earn" banner for existing students — links to referral widget
- 1.4.2 WhatsApp share button with pre-composed message and referral link
- 1.4.3 TikTok / social share buttons

### 1.5 SEO & AEO Readiness
- 1.5.1 Structured data: `EducationalOrganization` schema on homepage
- 1.5.2 Meta title, meta description, Open Graph tags for all social sharing
- 1.5.3 Canonical URL defined
- 1.5.4 FAQ section with `FAQPage` schema markup (top 5 questions about ICS)
- 1.5.5 Page speed optimization — images lazy-loaded, fonts preloaded, LCP < 2.5s

### 1.6 Analytics & Attribution
- 1.6.1 UTM parameter capture on page load, stored in sessionStorage
- 1.6.2 Conversion event fired on lead form submission (`LEAD_CREATED`)
- 1.6.3 Scroll-depth tracking (25%, 50%, 75%, 100%)
- 1.6.4 CTA click tracking per button/section

### 1.7 Mobile & Accessibility
- 1.7.1 Fully responsive — mobile-first layout
- 1.7.2 WCAG 2.1 AA compliance — contrast ratios, alt text, keyboard navigation
- 1.7.3 Touch-friendly CTAs (minimum 44x44px tap targets)
- 1.7.4 WhatsApp CTA opens native app on mobile via `wa.me` link

---

## 2. Lead Capture Engine

**Purpose:** Capture, store, and manage all inbound student enquiries from every channel into the `growth_leads` / `virality_leads` tables.

### 2.1 API Endpoints
- 2.1.1 `POST /api/virality/leads` — create a new lead (public, no auth)
- 2.1.2 `GET /api/virality/leads` — fetch lead list + dashboard totals (admin auth required)
- 2.1.3 `GET /growth/leads/dashboard` — GIP backend admin dashboard summary
- 2.1.4 `PATCH /growth/leads/:id` — update lead status (admin auth)

### 2.2 Lead Data Model
- 2.2.1 Required fields: `full_name`, `phone`, `source`
- 2.2.2 Optional fields: `email`, `interest`, `preferred_course_code`, `preferred_campus`, `preferred_study_mode`
- 2.2.3 Auto-set fields on creation: `id` (CUID), `status = NEW`, `score` (computed), `created_at`
- 2.2.4 Attribution fields: `campaign_id`, `referral_code`
- 2.2.5 Source enum values: `WEBSITE`, `WHATSAPP`, `CAREER_SCANNER`, `REFERRAL`, `TIKTOK`, `FACEBOOK`, `GOOGLE`, `DIRECT`

### 2.3 Lead Status Lifecycle
- 2.3.1 `NEW` → `CONTACTED` → `QUALIFIED` → `APPLIED` → `ADMITTED` → `LOST`
- 2.3.2 Status transitions logged as conversion events
- 2.3.3 Status change triggers downstream actions (nurture sequence enrollment, CRM sync, escalation)

### 2.4 Input Validation
- 2.4.1 Reject if `full_name` or `phone` missing — return 400 with descriptive message
- 2.4.2 Validate `source` is one of the allowed enum values
- 2.4.3 Email format validation (optional field)
- 2.4.4 Phone number sanitisation (strip spaces, non-numeric chars)
- 2.4.5 Use `class-validator` DTOs (`CreateLeadDto`) on GIP NestJS backend

### 2.5 Post-Capture Actions (event-driven)
- 2.5.1 Publish `GROWTH_LEAD_CREATED` event to Event Bus
- 2.5.2 Trigger AI lead scoring immediately after creation
- 2.5.3 Enroll lead into appropriate nurture sequence based on source + course
- 2.5.4 Queue CRM sync to ICS Live
- 2.5.5 Request next-best-action from YohPal Brain

### 2.6 Duplicate Handling
- 2.6.1 Check for existing lead by phone number before creating
- 2.6.2 If duplicate: update record with latest source/campaign, do not create new entry
- 2.6.3 Log deduplication event for audit trail

---

## 3. AI Career Scanner

**Purpose:** Interactive tool that helps prospective students discover the right course based on their interests, background, and career goals. Highest-quality lead source — adds +15 to lead score.

### 3.1 Scanner UI (Website Component)
- 3.1.1 Multi-step questionnaire — Step 1: interests/passions, Step 2: current education level, Step 3: preferred study mode, Step 4: career goals
- 3.1.2 Progress bar showing completion percentage
- 3.1.3 Animated transitions between steps
- 3.1.4 Back/Next navigation with answer persistence
- 3.1.5 Mobile-optimised layout

### 3.2 Course Matching Logic
- 3.2.1 Scoring matrix mapping interests + education level → course recommendations
- 3.2.2 Return top 3 recommended courses ranked by match score
- 3.2.3 Each recommendation shows: course name, duration, career outcomes, fees teaser, and "Apply Now" CTA
- 3.2.4 Pull course truth from Smart Prospectus API (not duplicated in GIP)

### 3.3 Lead Capture Integration
- 3.3.1 After recommendations are shown, prompt for Full Name + Phone to save results
- 3.3.2 Source set to `CAREER_SCANNER` automatically
- 3.3.3 `preferred_course_code` pre-filled from top recommendation
- 3.3.4 POST to `/api/virality/leads` with scanner results as metadata

### 3.4 Results Page
- 3.4.1 Personalised results page: "Based on your answers, here are your best-fit courses"
- 3.4.2 Each course card: title, duration, career pathway, "Learn More" → GEO course page, "Apply Now" → lead form
- 3.4.3 WhatsApp CTA: "Chat with an advisor about these results"
- 3.4.4 Share results link (shareable URL with course codes encoded)

---

## 4. Viral Referral Engine

**Purpose:** Turn existing students, alumni, and satisfied enquirers into acquisition channels through trackable referral links.

### 4.1 Referral Creation API
- 4.1.1 `POST /api/virality/referrals` — create referral code + link for a referrer
- 4.1.2 `GET /api/virality/referrals/:code` — look up referral stats by code
- 4.1.3 Input: `referrer_phone`, optional `referrer_name`
- 4.1.4 Output: unique `referral_code`, full `referral_link` (e.g. `https://ics.ac.ke/?ref=ABC123`)
- 4.1.5 Stored in `growth_referrals` table

### 4.2 Referral Tracking
- 4.2.1 Click counter incremented on every visit via referral link
- 4.2.2 Lead counter incremented when a referred visitor submits lead form
- 4.2.3 `referral_code` written to lead record on creation
- 4.2.4 Conversion chain: Click → Lead → Application → Admission tracked per referrer

### 4.3 Referral Widget (Website Component — `ViralReferralWidget.tsx`)
- 4.3.1 Displayed post-enquiry confirmation screen: "Know someone interested? Share your link"
- 4.3.2 Displays referrer's unique link in copy-to-clipboard text box
- 4.3.3 WhatsApp share button with pre-composed referral message
- 4.3.4 TikTok/Instagram share button
- 4.3.5 QR code display for in-person sharing
- 4.3.6 Mini leaderboard / "You've referred X students" counter

### 4.4 Referral Message Templates
- 4.4.1 WhatsApp: "Hey! I applied to ICS Technical College — check out their courses here: [link]"
- 4.4.2 SMS fallback template
- 4.4.3 Editable templates in admin panel

### 4.5 Student Ambassador Programme
- 4.5.1 Ambassador dashboard showing total clicks, leads generated, admissions attributed
- 4.5.2 Top referrer leaderboard (optional gamification)
- 4.5.3 Ambassador status tiers (Bronze / Silver / Gold based on referral count)

---

## 5. AI Lead Scoring Engine

**Purpose:** Automatically score every lead 0–100 to prioritise follow-up. High-score leads (HOT) get immediate attention; low-score leads enter nurture sequences.

### 5.1 Scoring API
- 5.1.1 `POST /api/virality/score` — score a lead and return score + priority + reason
- 5.1.2 `POST /growth/leads/score` — GIP backend scoring endpoint
- 5.1.3 Called automatically on every new lead creation
- 5.1.4 Can be re-called when lead data is updated

### 5.2 Scoring Rules
- 5.2.1 Base score: +30 (every lead starts here)
- 5.2.2 Has `preferred_course_code`: +20
- 5.2.3 Has `phone`: +15
- 5.2.4 Has `email`: +10
- 5.2.5 Has `preferred_study_mode`: +10
- 5.2.6 Source is `CAREER_SCANNER`: +15
- 5.2.7 Source is `REFERRAL`: +10
- 5.2.8 Source is `GOOGLE`: +5
- 5.2.9 Has `preferred_campus`: +5
- 5.2.10 Maximum score capped at 100

### 5.3 Priority Classification
- 5.3.1 Score ≥ 80 → `HOT` — immediate follow-up required (escalate to admissions team)
- 5.3.2 Score 50–79 → `WARM` — enroll in active nurture sequence
- 5.3.3 Score < 50 → `NURTURE` — enroll in long-term drip sequence

### 5.4 Score Output & Storage
- 5.4.1 Score written to `growth_leads.score` field
- 5.4.2 Priority written to `growth_leads.priority` field
- 5.4.3 Score reason stored for audit trail
- 5.4.4 Publish `GROWTH_LEAD_SCORED` event to Event Bus
- 5.4.5 Agent decision recorded in `growth_agent_decisions` table

### 5.5 Score-Triggered Actions
- 5.5.1 HOT lead → create escalation record → notify admissions staff (WhatsApp/email)
- 5.5.2 HOT lead → trigger immediate outreach message (if consent granted)
- 5.5.3 WARM lead → enroll in 5-step nurture sequence
- 5.5.4 NURTURE lead → enroll in 10-step long-term drip campaign
- 5.5.5 Score update triggers re-evaluation of nurture sequence assignment

---

## 6. Campaign Attribution & Tracking

**Purpose:** Track every lead back to the exact campaign, channel, and ad that generated it. Measure ROI per channel, per course, per campaign.

### 6.1 Campaign Data Model (`growth_campaigns` / `campaigns`)
- 6.1.1 Fields: `name`, `channel`, `target_course_code`, `objective`, `budget`, `status`
- 6.1.2 Status lifecycle: `DRAFT` → `ACTIVE` → `PAUSED` → `COMPLETED` → `CANCELLED`
- 6.1.3 CRUD API: `POST /growth/campaigns`, `GET /growth/campaigns`, `PATCH /growth/campaigns/:id`

### 6.2 UTM Parameter Capture
- 6.2.1 Capture `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term` from URL on page load
- 6.2.2 Persist UTM params in `sessionStorage` for the session duration
- 6.2.3 Attach UTM data to lead record on form submission
- 6.2.4 Map `utm_source` → `source` enum value automatically

### 6.3 Attribution Component (`CampaignAttribution.tsx`)
- 6.3.1 Silent background component — no visible UI
- 6.3.2 Reads UTM params and referral codes from URL on mount
- 6.3.3 Injects `campaign_id` and `referral_code` into all lead forms on the page
- 6.3.4 Fires attribution event on first page view

### 6.4 Conversion Event Tracking (`growth_conversion_events`)
- 6.4.1 Events to track: `PAGE_VIEW`, `FORM_OPEN`, `LEAD_CREATED`, `LEAD_SCORED`, `WHATSAPP_CLICK`, `REFERRAL_CLICK`, `APPLICATION_STARTED`, `APPLICATION_SUBMITTED`, `ADMISSION_GRANTED`, `PAYMENT_RECEIVED`
- 6.4.2 Each event stores: `lead_id`, `event_type`, `source`, `metadata` (JSON)
- 6.4.3 Event API: `POST /growth/events`
- 6.4.4 Events published to Event Bus for downstream consumption

### 6.5 Campaign Performance Metrics (`growth_campaign_metrics`)
- 6.5.1 Per-campaign, per-channel metrics: `impressions`, `clicks`, `leads`, `applications`, `admissions`, `spend`, `revenue`
- 6.5.2 Metrics ingestion endpoint: `POST /growth/campaigns/:id/metrics`
- 6.5.3 Computed derived metrics: CTR (clicks/impressions), CPL (spend/leads), CPA (spend/admissions), ROAS (revenue/spend)
- 6.5.4 Metrics updated in real-time as events arrive

### 6.6 Revenue Attribution (`growth_revenue_attributions`)
- 6.6.1 Link each admission/payment back to originating campaign + channel
- 6.6.2 Attribution model: last-touch (default), with future support for multi-touch
- 6.6.3 Attribution record created when `ADMISSION_GRANTED` or `PAYMENT_RECEIVED` event received
- 6.6.4 Revenue attribution API: `POST /growth/revenue-attributions`

---

## 7. Admissions Command Center (Dashboard)

**Purpose:** Real-time executive visibility into the full acquisition funnel — from visitors to revenue. The primary operational interface for the admissions team and management.

### 7.1 Dashboard Pages

#### 7.1.1 Overview / Executive Dashboard (`/admin/virality` · `/admissions-ai`)
- Live visitor count (today / this week / this month)
- Total leads (new today, this week, this month)
- Hot leads count with action queue
- Applications count
- Admissions count
- Revenue forecast vs actual
- Active campaigns count
- AI recommendations pending review
- Open approvals count
- Active escalations count
- CRM sync failure alerts

#### 7.1.2 Lead Pipeline Board
- Kanban-style view of leads by status: NEW → CONTACTED → QUALIFIED → APPLIED → ADMITTED → LOST
- Each lead card shows: name, phone, course interest, score badge (HOT/WARM/NURTURE), source icon, time since creation
- Click lead card → lead detail drawer
- Filter by: source, priority, course, campus, date range
- Sort by: score (desc), created date (desc)
- Bulk actions: change status, assign to staff, export CSV

#### 7.1.3 Lead Detail View
- Full lead profile: all fields, score breakdown, priority
- Conversation history (outreach messages sent + responses received)
- Timeline of conversion events
- Nurture sequence status (current step, next step, scheduled time)
- "Send WhatsApp" / "Send Email" manual action buttons
- "Escalate to Admissions" button
- CRM sync status

#### 7.1.4 Campaign Performance Dashboard
- Table of all campaigns: name, channel, status, spend, leads, applications, CPL, ROAS
- Click campaign → drill-down with ad variants, experiment results, budget recommendations
- Time-range selector (today / 7d / 30d / custom)
- Channel comparison charts (bar chart: leads per channel)
- Course performance breakdown

#### 7.1.5 AI Recommendations Queue
- List of pending AI agent decisions requiring human review
- Each item shows: agent name, decision, reason, confidence score, risk level, timestamp
- Actions: APPROVE / REJECT / REQUEST_CHANGES
- Filter by: agent, status, risk level
- Audit log of all past decisions

#### 7.1.6 Content Approval Queue
- List of AI-generated content assets pending review
- Preview pane showing title + body + channel + course
- Actions: APPROVE / REJECT / REQUEST_CHANGES with reason
- Approved content auto-schedules to content calendar

#### 7.1.7 Market Intelligence Panel
- Trend signals table: keyword, volume score, growth score, opportunity score, course, region
- Competitor signals: competitor name, signal type, severity, title, source URL
- Demand forecasts: course, period, predicted leads/applications/admissions/revenue, confidence %
- Market alerts: open alerts sorted by severity

#### 7.1.8 Safety & Compliance Panel
- Active autonomy policies and their limits
- Emergency pause status (ON/OFF with one-click toggle)
- Compliance log: recent pass/fail/warning entries
- Safety rules list with enabled/disabled toggles
- Autopilot decisions log filtered by risk level

### 7.2 Dashboard API Endpoints
- 7.2.1 `GET /api/virality/leads` — totals + lead list (Website BFF)
- 7.2.2 `GET /growth/leads/dashboard` — GIP backend full dashboard data
- 7.2.3 `GET /growth/campaigns` — all campaigns with metrics
- 7.2.4 `GET /growth/agent-decisions` — pending + recent decisions
- 7.2.5 `GET /growth/market-alerts` — open alerts
- 7.2.6 `GET /growth/compliance-logs` — compliance log entries
- 7.2.7 `GET /growth/autopilot/policies` — active policies

### 7.3 Access Control
- 7.3.1 Admin dashboard behind `GipAuthGuard` (Bearer token)
- 7.3.2 Role-based views: Executive (read-only), Manager (approve/reject), Super Admin (full)
- 7.3.3 All approval actions require authenticated user identity written to `approved_by`/`reviewed_by`

---

## 8. GEO / AEO Engine

**Purpose:** Make ICS Technical College discoverable by Google AI Overviews, ChatGPT, Gemini, Perplexity, and all future AI search engines — through structured, AI-citation-ready course and career pages.

### 8.1 GEO Entity Pages (`geo_entity_pages`)
- 8.1.1 One page per course — e.g. `/courses/ict-diploma-nairobi`
- 8.1.2 Page types: `COURSE_PAGE`, `CAREER_PATHWAY`, `COURSE_COMPARISON`, `FAQ_PAGE`, `CAMPUS_PAGE`
- 8.1.3 Page fields: `course_code`, `slug`, `page_type`, `title`, `summary`, `faq_json`, `schema_json`
- 8.1.4 Status lifecycle: `DRAFT` → `PENDING_REVIEW` → `APPROVED` → `PUBLISHED` → `ARCHIVED`
- 8.1.5 Human approval required before publishing (via `geo_approvals` table)

### 8.2 Structured Data Generation (`GeoSchemaService`)
- 8.2.1 `Course` schema: `@type: Course`, `name`, `description`, `provider` (ICS), `educationalCredentialAwarded`, `courseCode`, `hasCourseInstance`
- 8.2.2 `FAQPage` schema: array of `Question` + `Answer` pairs injected as JSON-LD
- 8.2.3 `EducationalOrganization` schema for institution pages
- 8.2.4 `BreadcrumbList` schema for navigation trail
- 8.2.5 Schema injected via Next.js `<script type="application/ld+json">` in `<head>`

### 8.3 AI-Optimised Page Content
- 8.3.1 Page summary written in answer-engine format — direct, factual, concise first paragraph
- 8.3.2 FAQ section: minimum 8 questions per course page, covering: What is [course]? Who is it for? What are entry requirements? What does it cost? What jobs can I get? How long does it take? What modes are available? How do I apply?
- 8.3.3 Comparison tables (e.g. ICT Diploma vs Certificate vs Short Course)
- 8.3.4 Career outcomes section with salary ranges and employer examples
- 8.3.5 "People Also Ask" section mirroring Google PAA structure

### 8.4 GEO Page Generation API
- 8.4.1 `POST /growth/geo/generate` — AI generates a new GEO page for a given course code
- 8.4.2 `GET /growth/geo/pages` — list all GEO pages with status
- 8.4.3 `GET /growth/geo/pages/:slug` — get single page
- 8.4.4 `POST /growth/geo/pages/:id/submit-for-review` — submit draft for human approval
- 8.4.5 `POST /growth/geo/approvals/:id/approve` — approve and queue for publish
- 8.4.6 `POST /growth/geo/approvals/:id/reject` — reject with reason

### 8.5 Search Intent Signals (`search_intent_signals`)
- 8.5.1 Ingest search keywords with intent type, course code, volume score, urgency score, opportunity score
- 8.5.2 Intent types: `INFORMATIONAL`, `NAVIGATIONAL`, `TRANSACTIONAL`, `COMPARISON`
- 8.5.3 Status: `NEW` → `ANALYZING` → `QUALIFIED` → `ACTING` → `DISMISSED`
- 8.5.4 High-opportunity signals (opportunity_score ≥ 150) trigger GEO page generation recommendation
- 8.5.5 Signals API: `POST /growth/search-intent`, `GET /growth/search-intent`

### 8.6 Website GEO Page Rendering
- 8.6.1 Next.js dynamic route: `app/courses/[slug]/page.tsx`
- 8.6.2 Fetch page data from GIP at build time (ISR) and on-demand
- 8.6.3 Revalidate every 24 hours or on publish webhook
- 8.6.4 Render JSON-LD schema, meta tags, Open Graph, Twitter Card
- 8.6.5 Breadcrumb navigation rendered from page hierarchy

---

## 9. AI Search Authority Engine

**Purpose:** Build ICS's citation authority across the web so that AI search engines (ChatGPT, Perplexity, Gemini) cite ICS as a trusted source when students ask about courses.

### 9.1 Authority Citation Targets (`authority_citation_targets`)
- 9.1.1 Maintain a database of target platforms for ICS citations
- 9.1.2 Target types: Education directories (KNEC, TVETA), review sites, career platforms, Wikipedia, news outlets, government sites
- 9.1.3 Fields: `name`, `platform`, `url`, `course_code`, `priority`, `status`, `notes`
- 9.1.4 Status: `TARGET` → `IN_PROGRESS` → `SECURED` → `LOST`
- 9.1.5 Priority: `LOW` / `MEDIUM` / `HIGH` / `CRITICAL`

### 9.2 Citation Tracking
- 9.2.1 Monitor when ICS is mentioned or cited on target platforms
- 9.2.2 Log citation acquisition date and source URL
- 9.2.3 Track citation health — alert if a secured citation is removed
- 9.2.4 Authority score per course (count of secured citations)

### 9.3 Reputation & Review Monitoring
- 9.3.1 Track Google Business Profile reviews — volume, average rating, recent reviews
- 9.3.2 Track reviews on education directories
- 9.3.3 Alert when negative review posted (severity: HIGH)
- 9.3.4 Generate suggested responses for negative reviews (human must approve before posting)

### 9.4 Directory Listings Management
- 9.4.1 Maintain NAP consistency (Name, Address, Phone) across all listings
- 9.4.2 Track listing status per directory
- 9.4.3 Flag inconsistencies for human correction

### 9.5 Authority Engine API
- 9.5.1 `POST /growth/authority/targets` — add new citation target
- 9.5.2 `GET /growth/authority/targets` — list all targets with status
- 9.5.3 `PATCH /growth/authority/targets/:id` — update status/notes
- 9.5.4 `GET /growth/authority/score/:courseCode` — get authority score for a course

---
