# ProofBeforeBuild

**Don't build on a guess. Test the evidence.**

ProofBeforeBuild is an AI-powered startup validation platform that challenges business ideas, investigates available evidence, identifies risky assumptions, and designs practical experiments to test whether customers actually want the product.

---

## Problem Statement and Solution

### The Problem

Most startups fail because they build products nobody wants. Founders skip validation — they guess at the problem, assume customers will care, and start building before testing the riskiest assumptions. By the time they discover they were wrong, they've burned months of time and money.

Existing tools either generate hype ("your idea sounds great!") or provide raw data without actionable analysis. There's no tool that combines skeptical, honest assessment with evidence classification and practical experiment design in one workflow.

### The Solution

ProofBeforeBuild replaces guessing with evidence. You submit your business idea, and the AI acts like a skeptical product researcher — not a cheerleader. It:

1. **Stress-tests the idea** — scores the opportunity across four dimensions (Problem Severity, Evidence Strength, Differentiation, Feasibility), identifies the riskiest assumptions, and lists failure reasons
2. **Classifies evidence** — every claim is labeled as Supported, Partially Supported, Unverified, or Contradictory, with source links when available
3. **Designs an experiment** — turns your single riskiest assumption into a testable experiment with steps, success/failure thresholds, and decision criteria
4. **Challenges your thinking** — ask it skeptical questions about your idea and get honest, specific answers
5. **Tracks progress** — mark experiment steps complete, record notes and metrics, and revisit saved assessments

The tool is explicitly honest about its limitations — it labels unverified claims, never invents URLs, and reminds you that talking to real customers is irreplaceable.

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | React 18, TypeScript, Vite |
| **Styling** | Tailwind CSS, shadcn/ui, Lucide icons |
| **Routing** | React Router |
| **Backend / Database** | Supabase (PostgreSQL, Auth, Edge Functions) |
| **AI Model** | OpenAI GPT-4o-mini via Supabase Edge Function |
| **Charts** | Recharts |
| **Forms** | React Hook Form + Zod validation |

---

## Setup and Installation Instructions

### Prerequisites

- Node.js 18 or higher
- npm

### Installation

```bash
# Clone or download the project, then:
npm install
```

### Development Server

```bash
npm run dev
```

The app runs at `http://localhost:5173`.

### Production Build

```bash
npm run build
```

The built files are output to the `dist/` directory.

### Type Checking

```bash
npm run typecheck
```

### Linting

```bash
npm run lint
```

---

## Environment Variables

### Supabase (pre-populated)

The following are already configured in the hosted Bolt environment:

- `VITE_SUPABASE_URL` — Supabase project URL
- `VITE_SUPABASE_ANON_KEY` — Supabase anon public key

### OpenAI API Key (configured)

The `OPENAI_API_KEY` is set as a Supabase edge function secret and **is active**. Real AI analysis and challenge responses are enabled — no demo mode fallback needed.

To verify or update the key, set it as an edge function secret in the Supabase dashboard (not in frontend `.env` files).

See `.env.example` for reference.

---

## AI Methodology and Implementation

### Approach

The AI is designed as a **skeptical product researcher**, not a hype generator. The methodology is inspired by lean startup validation, evidence-based decision-making, and the scientific method applied to business assumptions.

### Analysis Workflow

1. **Input** — The user submits a structured idea (title, description, target customer, problem, solution, market, budget, assumptions to test)
2. **Assessment** — The AI evaluates the idea across four scored dimensions:
   - **Problem Severity** (0-10): How painful is the problem?
   - **Evidence Strength** (0-10): How much evidence supports the need?
   - **Differentiation** (0-10): How differentiated is the solution?
   - **Feasibility** (0-10): How feasible is execution?
3. **Evidence Classification** — Each evidence claim is classified as:
   - **Supported** — backed by known, verifiable sources
   - **Partially Supported** — some evidence exists but is incomplete
   - **Unverified** — the AI cannot source the claim (clearly labeled, never fabricated)
   - **Contradictory** — evidence exists that argues against the claim
4. **Assumption Analysis** — 5-8 key assumptions are identified, each with a risk level (high/medium/low) and testability flag. The 3 riskiest are highlighted.
5. **Experiment Design** — The single riskiest assumption becomes a testable experiment with:
   - Hypothesis (if [action], then [measurable outcome])
   - Target participants
   - Step-by-step procedure
   - Success and failure thresholds
   - Bias risks
   - Decision criteria after results
6. **Recommendation** — The AI issues one of three recommendations:
   - **Investigate** — the idea has potential but needs validation
   - **Revise** — the idea needs significant changes before proceeding
   - **Reconsider** — the idea is fundamentally flawed

### Challenge My Idea

Users can ask skeptical questions about their idea. The AI responds as a constructive but honest product researcher — referencing the specific idea details, acknowledging unknowns, and never fabricating statistics.

### Honesty Safeguards

- The opportunity score is explicitly labeled as a **prioritization aid**, not a probability of success
- Unverified claims are marked clearly — the AI never invents URLs or sources
- The AI acknowledges what it doesn't know in an "Unknowns" section
- Demo data is always labeled with a visible badge when used

---

## AI Model Names and API Usage

### Model

- **OpenAI GPT-4o-mini** — used for both analysis and challenge responses
- **Temperature**: 0.7 (balances consistency with natural variation)
- **Response format**: JSON object (structured output for reliable parsing)
- **Max tokens**: 4,000 for analysis, 1,500 for challenge responses

### API Call Flow

```
User submits idea
    → Frontend calls Supabase Edge Function (pbb-analyze)
        → Edge function authenticates user via JWT
        → Edge function calls OpenAI Chat Completions API
            → System prompt: validation analyst persona + JSON schema
            → User prompt: idea details
        → OpenAI returns structured JSON
    → Frontend parses assessment, evidence, experiment
    → Data saved to Supabase database (pbb_projects, pbb_assessments, etc.)
```

### Edge Function Endpoints

The `pbb-analyze` edge function handles two actions:

- `action: "analyze"` — Full idea assessment (returns assessment + evidence + experiment)
- `action: "challenge"` — Skeptical Q&A response (returns answer text)

Both require authenticated user JWT. Input is size-limited to 8,000 characters.

---

## Product Screenshots

> Screenshots are captured from the live app. Below is a guide walkthrough of each screen.

### 1. Landing Page

The hero section with the headline "Before You Build It, Prove Someone Wants It." Includes feature highlights, a how-it-works section, FAQ accordion, and footer with legal links.

### 2. Authentication

Clean sign-up / sign-in page with email and password fields. Toggle between creating an account and signing in.

### 3. Idea Submission

Guided form with fields for title, idea description, target customer, problem, solution, market, budget, and assumptions to test. Includes a "Load Example" button that populates the form with a sample idea (AI Marketing Assistant for Independent Restaurants).

### 4. Research Workspace

Live progress view showing research stages: Analyzing Idea, Researching Evidence, Identifying Assumptions, Designing Experiment, Compiling Report. Each stage animates as it completes.

### 5. Assessment Report

Full report with:
- Opportunity score (0-100) with dimension breakdown bars
- Executive summary
- Customer profile and problem-solution fit
- Competitor analysis and differentiation
- Key assumptions table with risk levels
- Top 3 risky assumptions highlighted
- Evidence list with classification badges (Supported / Partial / Unverified / Contradictory)
- Failure reasons and improvement suggestions
- Unknowns section
- Recommendation card (Investigate / Revise / Reconsider)
- "Challenge My Idea" interactive Q&A panel

### 6. Experiment Workspace

Experiment tracker with:
- Hypothesis and assumption being tested
- Step-by-step checklist (mark steps complete)
- Notes section (add timestamped observations)
- Metrics tracking
- Success/failure thresholds
- Decision criteria

### 7. My Projects

List of all saved projects with status badges, opportunity scores, and dates. Click any project to revisit its assessment report.

### 8. Settings / Methodology

Explains the validation methodology, AI limitations, privacy practices, and how the opportunity score should (and should not) be used.

---

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
│   ├── ai-service.ts         — AI API calls (analyze + challenge)
│   └── demo-data.ts          — Pre-loaded example assessment
├── hooks/
│   └── use-auth.tsx          — Auth context provider
└── components/
    ├── DashboardLayout.tsx   — Sidebar nav layout
    ├── Logo.tsx              — Brand logo
    └── ui/                   — shadcn/ui component library

supabase/
├── functions/
│   └── pbb-analyze/index.ts  — Edge function: AI analysis + challenge
└── migrations/
    ├── 20261016201940_create_profiles_table.sql
    ├── 20261016202000_create_projects_and_usage_tables.sql
    ├── 20261016202014_create_preferences_table.sql
    └── 20261017212514_create_pbb_core_tables.sql
```

---

## Database Schema

| Table | Purpose |
|-------|---------|
| `pbb_projects` | User's submitted business ideas |
| `pbb_assessments` | AI-generated assessment versions (JSONB, supports versioning) |
| `pbb_evidence` | Evidence items linked to assessments |
| `pbb_experiments` | Validation experiments with steps and thresholds |
| `pbb_experiment_notes` | Notes and metrics recorded during experiments |
| `pbb_challenges` | Challenge My Idea Q&A history |

All tables have **Row Level Security (RLS)** enabled with owner-scoped policies (`auth.uid() = user_id`). Zero security advisor findings.

---

## Features

### Complete and Working

- Landing page with full conversion flow
- User authentication (sign up / sign in / sign out)
- Idea submission form with validation and progress indicator
- Research workspace with live progress stages
- AI-generated assessment report with scores, evidence, risks, unknowns
- Evidence classification with source links and confidence notes
- Experiment designer and tracker with checklist, notes, metrics
- Challenge My Idea interactive AI feature
- Saved projects with assessment history
- Settings and methodology page
- Privacy policy and terms pages
- Demo mode fallback with clearly labeled example data
- Responsive design (mobile + desktop)
- Row-level security on all database tables
- OpenAI API key configured and active

### Planned Future Improvements

- Live web search API integration for real-time evidence retrieval
- Customer interview integrations
- Landing page analytics for experiment tracking
- Longitudinal tracking of validation outcomes
- Assessment version comparison diff view

---

## Limitations

- The opportunity score is a prioritization aid, not a prediction of success
- AI reasoning can contain errors — always verify critical claims with original sources
- The AI cannot access paywalled or proprietary research
- Market research does not guarantee commercial success
- Talking to real customers is essential and cannot be replaced by this tool

---

## Team Member Contributions

| Member | Role | Contributions |
|--------|------|--------------|
| _Add your name_ | _Add your role_ | _Describe what you worked on_ |

> Edit this table to credit each team member and their specific contributions (e.g., frontend development, AI prompt design, database schema, UI/UX design, testing).

---

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

---

## License

Built for hackathon demonstration purposes.
