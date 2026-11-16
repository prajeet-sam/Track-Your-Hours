# DevPulse Action Plan

## 1. Goal

Transform `Track-Your-Hours` into **DevPulse**, a developer growth intelligence platform that unifies coding time, GitHub activity, skills, projects, and career readiness in one privacy-conscious dashboard.

The product promise is: **“Don’t just track your commits. Track the engineer you’re becoming.”**

## 2. Current baseline

- The repository currently contains only a minimal `README.md` and `LICENSE`.
- There is no application scaffold, test runner, backend, database schema, or CI workflow yet.
- Because the baseline is empty, the first milestone must establish the foundation before feature work begins.

## 3. Product scope

### MVP

1. Developer overview dashboard
2. GitHub analytics
3. Coding activity tracker
4. Skill development tracker
5. Project portfolio tracker

### Post-MVP

6. Career readiness and skill-gap analysis
7. Developer Wrapped reports
8. WakaTime integration
9. Shareable reports and advanced growth insights

## 4. Recommended architecture

### Frontend

- React + TypeScript with Vite
- Tailwind CSS and shadcn/ui
- React Router for page navigation
- TanStack Query for server state and caching
- Recharts for analytics
- Lucide React for icons

### Backend and data

- Supabase Auth with GitHub OAuth
- Supabase PostgreSQL with Row Level Security enabled
- Supabase Storage for project screenshots and report assets
- Supabase Edge Functions for GitHub synchronization, aggregation, and scheduled reports

### External integrations

- GitHub REST/GraphQL APIs for repositories, commits, pull requests, issues, languages, stars, and forks
- WakaTime API as an optional coding-activity source

## 5. Delivery phases

### Phase 0 — Foundation

- Scaffold the React/TypeScript application and establish environment configuration.
- Add formatting, linting, type-checking, unit-test, and build scripts.
- Create the application shell: responsive navigation, page layout, loading states, empty states, and error states.
- Set up Supabase projects, migrations, typed database access, and local development instructions.
- Add GitHub Actions for lint, type-check, tests, and production build.
- Define a consistent design system for colors, spacing, typography, cards, charts, and accessible form controls.

**Exit criteria:** A new developer can install, run, test, and build the app; an authenticated user can reach an empty dashboard.

### Phase 1 — Authentication and GitHub Intelligence

- Implement sign-in, sign-out, session restoration, and protected routes with GitHub OAuth.
- Add connected-account settings and a sync control.
- Build the GitHub sync function with pagination, rate-limit handling, retry/backoff, and incremental updates.
- Store repositories, languages, contributions, pull requests, issues, stars, and forks.
- Build the GitHub Analytics page with repository table, contribution heatmap, language distribution, activity trends, and repository health signals.
- Show data freshness and sync errors so users can distinguish missing data from zero activity.

**Exit criteria:** A user can connect GitHub, sync public activity safely, and view useful analytics without exposing another user’s records.

### Phase 2 — Coding Activity and Projects

- Add start/stop coding sessions with project, activity category, notes, and timestamps.
- Add daily coding goals, streak calculation, weekly summaries, and timezone-aware date handling.
- Build project workspaces with description, repository link, tech stack, status, dates, deployment URL, screenshots, milestones, tasks, and total development time.
- Link coding sessions and GitHub activity to projects where possible.
- Add validation for durations, dates, URLs, ownership, and file uploads.

**Exit criteria:** A user can track a session, assign it to a project, complete milestones, and see project progress reflected on the dashboard.

### Phase 3 — Skills and Overview Dashboard

- Add skills with category, self-assessed proficiency, target level, learning goal, evidence links, and learning hours.
- Add skill timeline events for courses, projects, assessments, and milestones.
- Clearly label self-reported proficiency separately from evidence-backed indicators.
- Build the overview dashboard with total coding hours, GitHub contributions, active projects, technologies explored, current streak, consistency score, weekly activity, and growth trends.
- Add filters for date range, project, language, and activity type.

**Exit criteria:** The dashboard gives an accurate, understandable snapshot of a user’s activity and growth, including useful empty and partial-data states.

### Phase 4 — Career Readiness

- Let users create multiple career goals such as Frontend Developer, Full-stack Developer, AI/ML Engineer, and Backend Developer.
- Support pasted job descriptions and manual skill requirements.
- Extract or map required skills with confidence and allow user review before saving.
- Compare target levels with self-assessed levels and verified evidence.
- Generate prioritized skill gaps, recommended learning actions, and a roadmap tied to projects and resources.

**Exit criteria:** A user can compare a target role against their current evidence, understand the gaps, and receive an editable learning roadmap.

### Phase 5 — Developer Wrapped and Launch Polish

- Generate monthly and yearly reports from persisted snapshots rather than recalculating unreliable historical API data.
- Include coding hours, contributions, projects, technologies, streaks, most productive periods, and notable milestones.
- Add responsive mobile layouts, accessible keyboard navigation, performance optimization, and data export/delete controls.
- Add shareable report generation with explicit user consent and privacy-safe defaults.
- Document deployment, environment variables, OAuth setup, data retention, and troubleshooting.

**Exit criteria:** A user can generate a polished report, export or delete their data, and use the core product on desktop and mobile.

## 6. Initial database model

Create migrations for:

- `profiles`
- `github_accounts`
- `repositories`
- `repository_languages`
- `contributions`
- `pull_requests`
- `issues`
- `coding_sessions`
- `coding_goals`
- `skills`
- `skill_events`
- `projects`
- `project_technologies`
- `milestones`
- `tasks`
- `career_goals`
- `skill_requirements`
- `learning_roadmap_items`
- `report_snapshots`

Every user-owned table must include `user_id`, timestamps, appropriate indexes, and Row Level Security policies that allow a user to read and modify only their own records. External IDs should be unique within their provider/account scope, and sync operations must be idempotent.

## 7. API and data rules

- Keep provider tokens server-side; never expose GitHub or WakaTime secrets to the browser.
- Request the smallest OAuth scope required and explain why each scope is needed.
- Paginate all provider requests and respect rate limits.
- Treat provider data as eventually consistent and record sync status, errors, and timestamps.
- Cache derived analytics and recompute them from stored snapshots when possible.
- Normalize dates to UTC in storage and render them in the user’s selected timezone.
- Validate all client input again in server-side functions.
- Use signed, expiring URLs for private project assets.

## 8. Testing strategy

- Unit tests for streaks, durations, consistency scores, health scores, proficiency comparisons, gap prioritization, and report aggregation.
- Component tests for forms, protected routes, loading/error/empty states, and accessible chart summaries.
- Integration tests for authentication, RLS isolation, GitHub sync idempotency, pagination, retries, and rate-limit handling.
- End-to-end smoke tests for sign-in, GitHub sync, session tracking, project creation, skill updates, and report generation.
- Run lint, type-check, tests, and production build in CI on every pull request.

## 9. Milestone sequence

1. Scaffold and CI
2. Supabase schema and RLS
3. Authentication
4. GitHub sync and analytics
5. Coding sessions
6. Projects and milestones
7. Skills and dashboard
8. Career readiness
9. Wrapped reports, exports, and launch polish

Each milestone should ship with its schema migration, UI states, tests, documentation updates, and a short manual verification checklist.

## 10. Definition of done

- Core workflows work for a new user and a user with partial or no data.
- All user data is protected by RLS and provider secrets remain server-side.
- Syncs are observable, retry-safe, paginated, and rate-limit-aware.
- The UI is responsive, keyboard accessible, and clear about self-reported versus verified information.
- Tests, type-checking, linting, and production build pass in CI.
- Users can export and delete their data.
- README documentation explains local setup, Supabase configuration, OAuth setup, deployment, and known API limitations.
