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

| Area | Description |
|------|-------------|
| Coach dashboard | Student list, detail views, weekly plan builder, appointment schedule, exam result statistics with subject-level breakdown |
| Exam analytics | TYT/AYT net calculation (4 wrong = 1 correct deducted), date-based progress graphs, server-side cache for analytics modal |
| PDF reports | Automated exam and weekly plan reports (PDFKit) |
| Security | RLS policies, centralized rate limiting (Upstash Redis with in-memory fallback), CSP and security headers on deploy |
| Billing UX | Subscription and capacity management flows on the coach dashboard (phased rollout) |

## Outcome

- Mobile app published on [Google Play](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) with 1,000+ installs
- Web product live at [ekocapp.com.tr](https://ekocapp.com.tr)
- Used in production by coaches and students in the YKS preparation market

## Visuals

Mobile app UI: [Google Play listing](https://play.google.com/store/apps/details?id=com.ekoc.e_koc) and [App Store listing](https://apps.apple.com/tr/app/e-ko%C3%A7-yks-takip-ve-analiz/id6757975623) (screenshots on each store page).

Web product: [ekocapp.com.tr](https://ekocapp.com.tr)

## Source availability

Source repositories are private. A technical walkthrough or demo can be arranged on request.
