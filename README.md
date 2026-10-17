# ProofBeforeBuild

**Don't build on a guess. Test the evidence.**

ProofBeforeBuild is an AI-powered startup validation platform that challenges business ideas, investigates available evidence, identifies risky assumptions, and designs practical experiments to test whether customers actually want the product.

## What It Does

1. **Idea Stress Test** — Submit your business idea and get a brutally honest assessment: opportunity score, riskiest assumptions, competitor analysis, and failure reasons.
2. **Evidence Research** — Every claim is classified as Supported, Partially Supported, Unverified, or Contradictory. Source links are provided when available.
3. **Experiment Designer** — Turn your riskiest assumption into a testable experiment with clear steps, success/failure thresholds, and decision criteria.
4. **Challenge My Idea** — Ask the AI skeptical questions about your idea. It acts like a product researcher, not a cheerleader.
5. **Saved Projects** — Every assessment is saved. Track experiments, record notes, and revisit your analysis.

## Tech Stack

- **Frontend:** React, TypeScript, Vite, Tailwind CSS, shadcn/ui, Lucide icons
- **Backend:** Supabase (PostgreSQL database, Auth, Edge Functions)
- **AI:** OpenAI API (gpt-4o-mini) via Supabase Edge Function
- **Routing:** React Router

## Getting Started

### Prerequisites

- Node.js 18+
- npm

### Installation

```bash
npm install
```

### Development

```bash
npm run dev
```

### Build

```bash
npm run build
```

## Environment Variables

See `.env.example` for required variables. Supabase variables are pre-populated. To enable real AI analysis:

1. Set `OPENAI_API_KEY` as an edge function secret in Supabase (dashboard or CLI)
2. Without it, the app runs in **demo mode** with pre-loaded example data

## Architecture

```
src/
├── pages/
│   ├── LandingPage.tsx       — Hero, features, how-it-works, FAQ, footer
│   ├── AuthPage.tsx          — Sign in / sign up
│   ├── SubmitPage.tsx        — Guided idea submission form
│   ├── WorkspacePage.tsx     — Research progress with live stages
│   ├── ReportPage.tsx        — Full assessment report + Challenge My Idea
│   ├── ExperimentPage.tsx    — Experiment tracker with checklist, notes, metrics
│   ├── ProjectsPage.tsx      — Saved projects list
│   ├── SettingsPage.tsx      — Methodology, AI limitations, privacy
│   └── LegalPage.tsx         — Privacy policy & terms
├── lib/
│   ├── types.ts              — TypeScript interfaces
│   ├── supabase.ts           — Supabase client
│   ├── ai-service.ts         — AI API calls
│   └── demo-data.ts          — Pre-loaded example assessment
├── hooks/
│   └── use-auth.tsx          — Auth context provider
└── components/
    ├── DashboardLayout.tsx   — Sidebar nav layout
    └── Logo.tsx              — Brand logo

supabase/
├── functions/
│   └── pbb-analyze/index.ts  — Edge function: AI analysis + challenge
└── migrations/
    └── 20261017212514_create_pbb_core_tables.sql
```

## Database Schema

- **pbb_projects** — User's submitted business ideas
- **pbb_assessments** — AI-generated assessment versions
- **pbb_evidence** — Evidence items linked to assessments
- **pbb_experiments** — Validation experiments
- **pbb_experiment_notes** — Notes and metrics recorded during experiments
- **pbb_challenges** — Challenge My Idea Q&A history

All tables have RLS enabled with owner-scoped policies (`auth.uid() = user_id`).

## Demo Mode

When `OPENAI_API_KEY` is not configured, the app uses pre-loaded demo data from an example idea ("AI Marketing Assistant for Independent Restaurants"). All demo data is clearly labeled with a "Demo Data" badge.

## Features: Complete vs. Requires Credentials

### Complete
- Landing page with full conversion flow
- User authentication (sign up / sign in)
- Idea submission form with validation and progress indicator
- Research workspace with live progress stages
- Assessment report with scores, evidence, risks, unknowns
- Experiment designer and tracker with checklist, notes, metrics
- Challenge My Idea interactive AI feature
- Saved projects with assessment history
- Settings & methodology page
- Privacy policy and terms pages
- Demo mode with clearly labeled example data
- Responsive design (mobile + desktop)
- Row-level security on all database tables

### Requires OpenAI API Key
- Real AI-generated assessments (currently shows demo data)
- Real Challenge My Idea responses (currently shows demo responses)

### Planned Future Improvements
- Live web search API integration for real-time evidence retrieval
- Customer interview integrations
- Landing page analytics for experiment tracking
- Longitudinal tracking of validation outcomes
- Assessment version comparison diff view

## Limitations

- The opportunity score is a prioritization aid, not a prediction of success
- AI reasoning can contain errors — always verify critical claims with original sources
- The AI cannot access paywalled or proprietary research
- Market research does not guarantee commercial success
- Talking to real customers is essential and cannot be replaced by this tool

## Hackathon Demo Guide

1. Visit the landing page — read the hero, features, and how-it-works
2. Click "Stress-Test My Idea" to create an account
3. On the submit page, click "Load Example" to populate the example idea
4. Submit the idea and watch the research workspace progress through stages
5. View the full assessment report — note the opportunity score, evidence classifications, and risk analysis
6. Try "Challenge My Idea" with suggested questions
7. Open the experiment workspace — mark steps complete, add notes and metrics
8. Visit "My Projects" to see saved projects
9. Visit "Settings" to read the methodology and AI limitations

## License

Built for hackathon demonstration purposes.
