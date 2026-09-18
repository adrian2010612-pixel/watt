# WHAT IF v2 Engineering Blueprint

## Product thesis
WHAT IF is an interactive alternate-timeline platform, not a static article feed.

**Core loop:** Discover → Branch → Explore → Butterfly Effects → Predict → Quiz → XP → Save/Share → Return.

**North-star metric:** Weekly Completed Timelines.

## Target production structure
```text
apps/web          Next.js + TypeScript client
apps/admin        editorial + moderation console
packages/ui       design system
packages/domain   shared types + validation
services/ai       generation/orchestration
services/evidence claim/source verification
services/moderation safety + spam + review
PostgreSQL        source of truth
Redis/queue       jobs, rate limits, notifications
Object storage    images/media
Analytics         product events + retention
```

Start as a modular monolith. Split services only when scale justifies it.

## Core entities
users, profiles, scenarios, scenario_versions, branches, timeline_events, butterfly_effects, claims, sources, scenario_sources, predictions, votes, quizzes, quiz_attempts, saved_scenarios, achievements, user_achievements, xp_events, streaks, comments, reports, moderation_actions, notifications, analytics_events.

## API contracts
```text
GET  /v1/scenarios
GET  /v1/scenarios/:id
GET  /v1/daily
GET  /v1/recommendations
POST /v1/scenarios/:id/predictions
POST /v1/scenarios/:id/complete
POST /v1/scenarios/:id/save
POST /v1/scenarios/generate
POST /v1/community/scenarios
POST /v1/community/:id/report
GET  /v1/search?q=
GET  /v1/me
GET  /v1/me/activity
```

## Server-authoritative rules
Never trust the browser for XP, premium entitlement, achievements, moderation state, source verification or ownership. Use database transactions and idempotency keys for reward events.

## AI pipeline
```text
premise
  ↓
retrieve factual context
  ↓
claim graph
  ↓
branch/timeline generation
  ↓
evidence + confidence attachment
  ↓
safety/misinformation checks
  ↓
human/editor review
  ↓
published scenario version
```

AI should generate hypotheses. It should not fabricate citations or turn unsupported claims into facts.

## Analytics events
scenario_opened, branch_selected, prediction_submitted, timeline_completed, quiz_completed, scenario_saved, scenario_shared, daily_opened, scenario_generated, scenario_created, scenario_published, comment_created, report_submitted, invite_sent, subscription_started.

## First production milestone
1. Auth
2. PostgreSQL
3. Scenario API
4. User progress API
5. Prediction API
6. Analytics
7. Admin editor

Add AI generation after the core loop is measurable.
