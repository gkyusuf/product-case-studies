# E-Koç — YKS exam preparation platform

## Context

E-Koç is a commercial education product for students preparing for the Turkish university entrance exam (TYT/AYT). The platform connects coaches with students: coaches manage plans, review exam results, and track progress through a web dashboard; students record practice exam scores, follow study plans, and view analytics through a mobile app.

## Problem

YKS preparation involves scattered data: practice exam scores, subject-level performance, weekly study plans, and coach feedback are often managed across spreadsheets and messaging apps. Coaches need a single view of student progress; students need structured tracking and analysis without manual calculation.

## Approach

The system is split into two surfaces sharing a common backend:

- **Mobile app (student)** — score entry, subject analysis, study plans, progress charts
- **Web dashboard (coach)** — student management, weekly plan builder, exam analytics, scheduling, subscription and capacity management

Both clients connect to Supabase (authentication, database, row-level security, realtime). Server-side business logic runs on Netlify Functions (Express API).

### Architecture

```
Student app (Flutter)  ──┐
                         ├── Supabase (Auth, DB, RLS, Realtime)
Coach web dashboard    ──┤
                         └── Netlify Functions (Express API, PDF generation)
```

### Web stack

- Modern JavaScript (ES6+), Tailwind CSS
- Supabase JS client for authentication and data access
- Netlify Functions (Node.js 18, Express) for API routes, PDF report generation, and integrations
- Server-side caching for exam analytics endpoints

### Mobile stack

- Flutter (Dart)
- Supabase Auth and direct database access with RLS
- Shared week-start and scheduling logic aligned with the coach dashboard

### Selected engineering work

#### Coach dashboard and API layer

Built the coach-facing web panel end to end: student roster, invitation and pending-request flows, weekly and daily plan review, appointment scheduling, coach notes, and student detail views with paginated weekly reports. Backend exposes a JWT-authenticated REST API (`/api/dashboard/*`) on Netlify Functions (Express). Coach–student data isolation enforced at the database layer through Supabase RLS.

#### Exam analytics and scoring

Implemented TYT/AYT net scoring using the official rule set (four incorrect answers deduct one correct answer). Added subject-level breakdowns, date-based progress views, and an analytics modal backed by a five-minute server-side cache to avoid repeated aggregate queries on the same exam result.

#### Cross-platform plan and scheduling model

Defined a shared week-start and day-of-week convention (0 = Monday) used by both the Flutter mobile app and the web dashboard, so coaches and students operate on the same plan boundaries. Mobile daily-task completion flows sync back to the shared Supabase schema consumed by the coach panel.

#### PDF report pipeline

Server-side PDF generation (PDFKit) for exam summaries and weekly study plans. Separated view-model logic from template rendering so report content can evolve without changing the data layer.

#### Performance and caching

Introduced a TTL-based client cache (sessionStorage) with per-resource invalidation rules, plus server-side caching on hot analytics endpoints. Combined with auth and query-path optimizations, this reduced database load on frequently accessed dashboard routes (internal measurement: roughly 50–60% fewer requests on cached paths).

#### Authentication and onboarding

Coach registration and login via Supabase Auth, with SMS OTP verification (Firebase). Public landing site with pricing and early-access flows. Invitation-code system linking mobile students to coach accounts.

#### Security and deployment

Row-level security across coach and student tables, centralized rate limiting (Upstash Redis with in-memory fallback), CSP with nonce-based script loading on dashboard pages, and security headers configured in the Netlify deploy pipeline.

#### Subscription and capacity (in progress)

Phased billing UX on the coach dashboard: subscription tier display and student capacity limits. Payment integration planned; current release covers product-side flows and UI.

## Outcome

- Mobile app published on [Google Play](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) with 1,000+ installs
- Web product live at [ekocapp.com.tr](https://ekocapp.com.tr)
- Used in production by coaches and students in the YKS preparation market

## Visuals

Mobile app UI: [Google Play listing](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store listing](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) (screenshots on each store page).

Web product: [ekocapp.com.tr](https://ekocapp.com.tr)

## Source availability

Source repositories are private. A technical walkthrough or demo can be arranged on request.
