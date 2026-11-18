# StackPulse

A dashboard prototype for startup founders that reads the inbox, pulls out the commitments hidden in it, drafts replies, and flags tech-stack news worth acting on. It suggests and drafts; the founder approves every action.

## The problem

Founders lose time to a noisy inbox and a stream of tool and model updates. Most AI assistants either act too much on their own, which is risky, or only produce generic summaries. StackPulse aims for the middle: it drafts, scores and proposes, and never acts without the founder.

## What it does

- **Daily brief:** overdue commitments, a risk meter and an estimate of time saved.
- **Ops agent:** reads emails, extracts commitments (who promised what, by when), drafts replies and tracks follow-up workflows.
- **Tech agent:** scores tech-feed items, such as model releases, pricing changes and trending GitHub projects, by relevance, risk and migration cost.
- **Calendar:** suggests events from the commitments it found.
- **Activity log:** a record of every AI suggestion and every action the founder took.
- **Integrations screen:** Google plus placeholder connections for other tools such as Stripe, Notion and HubSpot.
- **Simulation mode:** the whole app works on built-in sample data, so you can try it without connecting anything.

In this version the inbox is sample data, and the tech agent generates a realistic sample feed with the model rather than reading live sources.

## How it works

- The React front end calls two Supabase Edge Functions, `ops-agent` and `tech-agent`.
- Each function calls a Google Gemini model through the Lovable AI Gateway and returns structured JSON, which is saved to Postgres.
- Tables include `emails`, `commitments`, `calendar_events`, `activity_log`, `ops_tasks`, `tech_feed`, `tech_analyses`, `tech_tasks` and `workflows`.
- There is no login in the demo. Data is tied to a random ID stored in your browser.

## Run it

You need Node.js 18 or newer (or Bun).

```bash
git clone https://github.com/moazzanimran2007/stack-pulse-ai.git
cd stack-pulse-ai
npm install
npm run dev
```

The app reads `VITE_SUPABASE_URL`, `VITE_SUPABASE_PUBLISHABLE_KEY` and `VITE_SUPABASE_PROJECT_ID` from `.env`. Copy `.env.example` to `.env` and fill them in from your own Supabase project, then deploy the functions in `supabase/functions`.

To deploy the front end on Vercel, import the repo with the Vite preset, build command `npm run build`, output folder `dist`, and the three variables above.

## Tech stack

React 18 and TypeScript on Vite, Tailwind CSS with shadcn/ui, React Router, TanStack Query, Three.js with react-three-fiber for the 3D logo, and Supabase (Postgres and Edge Functions). Built with Lovable.

## Project structure

```
src/
  components/      Feature components and shadcn/ui building blocks
  pages/           Screens: home, brief, ops, tech, calendar, activity
  lib/             Services: mode, activity log, commitments, sample data, device ID
  hooks/           Shared React hooks
  integrations/    Generated Supabase client and types
supabase/
  functions/       ops-agent and tech-agent
```

## Next steps

- Real sign-in with Supabase Auth and Google OAuth, with per-user data rules
- Live Gmail and Google Calendar instead of sample data
- Scheduled daily briefs
- End-to-end tests with Playwright
