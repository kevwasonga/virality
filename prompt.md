# EduOS Growth Intelligence Platform (GIP) — Project Build Guidelines & State

## Tech Stack
- **API**: NestJS
- **UI**: Next.js
- **Styling**: TailwindCSS
- **Database**: PostgreSQL with Prisma ORM

## Context Files
- **Database Schema**: [tables.md](file:///home/verdo/yohpal/virality/tables.md)
- **Features Spec**: [GUIDELINE.md](file:///home/verdo/yohpal/virality/GUIDELINE.md)

---

## Build Order (Strict — No skipping, no mixing)
1. **DB Initialisation / Migrations** → fully complete and committed
2. **API Layer** → fully complete and committed
3. **UI Layer** → fully complete and committed
4. **Authentication** is implemented LAST, following the same order above

---

## Per-Feature Workflow (Repeat for each feature)
- **Step 1** → Initialise / migrate DB for Feature [N]
- **Step 2** → Commit all DB files individually (see Git rules below)
- **Step 3** → Implement Feature [N] — API layer
- **Step 4** → Test API
- **Step 5** → Commit all API files individually
- **Step 6** → Implement Feature [N] — UI layer
- **Step 7** → Test UI
- **Step 8** → Full integration test
- **Step 9** → Commit all UI files individually
- **Step 10** → Await user greenlight before proceeding to Feature [N+1]

---

## Git Commit Rules (Strictly Enforced)
- Every committable file gets its own individual commit — never batch files in one commit.
- Commit message format must follow Conventional Commits, scoped to layer:
  - `feat(db): add growth_leads table migration`
  - `feat(api): implement POST /api/virality/leads endpoint`
  - `feat(ui): add lead capture form widget`
  - `fix(api): handle validation errors on lead entry`
  - `chore(db): seed initial growth_campaigns`
- Never commit files from two different features in the same commit.
- Never commit across layers in the same commit (no mixing db + api, api + ui, etc.).
- A feature is only considered done when all its commits are in.
- Commit only after the relevant test step passes.

---

## Active Session Status
- **Current Feature**: Feature 1 — Landing Page & Lead Capture Widget
- **Current Step**: Step 3 — Implement Feature 1 API Layer (NestJS endpoints for lead capture)
- **Stack**: NestJS / Next.js / TailwindCSS / PostgreSQL / Prisma


---

## Standing Rules
- Do not proceed to the next step or feature without explicit user greenlight.
- Follow the schema in [tables.md](file:///home/verdo/yohpal/virality/tables.md) exactly — no assumptions or additions.
- Write clean, modular, production-ready code at every step.
- After each step, summarise: what was done, what was committed, and what the next step will be.
