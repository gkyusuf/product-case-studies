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

### Stack

**Web:** JavaScript, Tailwind CSS, Supabase client, Netlify Functions (Node.js, Express)

**Mobile:** Flutter, Supabase

### Engineering highlights

- **Product architecture** — Two client surfaces (Flutter app, coach web dashboard) on a shared Supabase backend with a serverless API layer for business logic, reporting, and integrations.

- **Coach platform** — Operational dashboard covering student management, planning, scheduling, exam analytics, PDF reporting, and coach notes. Multi-tenant data model with database-level isolation per coach.

- **Cross-platform consistency** — Shared domain model for plans and scheduling across mobile and web so coaches and students work from the same source of truth.

- **Performance** — Client and server-side caching on high-traffic dashboard paths; reduced database load on production workloads.

- **Go-to-market** — Public landing and pricing, coach onboarding with OTP verification, invitation-based student linking.

- **Production posture** — RLS, rate limiting, CSP, and deploy-time security headers on a serverless infrastructure.

- **Commercial layer** — Subscription and capacity UX on the dashboard; payment integration in progress.

## Outcome

- Mobile app published on [Google Play](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) with 1,000+ installs
- Web product live at [ekocapp.com.tr](https://ekocapp.com.tr)
- Used in production by coaches and students in the YKS preparation market

## Visuals

Mobile app UI: [Google Play listing](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store listing](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) (screenshots on each store page).

Web product: [ekocapp.com.tr](https://ekocapp.com.tr)

## Source availability

Source repositories are private. A technical walkthrough or demo can be arranged on request.
