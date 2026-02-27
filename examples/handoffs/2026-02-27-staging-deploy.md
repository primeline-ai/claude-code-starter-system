# Handoff: 2026-02-27 - Staging Deploy

**Project:** TaskFlow
**Phase:** building
**Date:** 2026-02-27

## Session Summary

Deployed TaskFlow to Vercel staging with Supabase production project. Auth, task CRUD, real-time updates, and team invites all working on staging. Found one issue: magic links in emails redirect to localhost instead of the staging URL because the Supabase site URL was still set to localhost.

## Decisions Made

- **Supabase production project:** Created a separate Supabase project for staging/production instead of using the dev project. Keeps data isolated and lets us test RLS policies in a clean environment.
- **Vercel preview deploys:** Enabled for PRs so team can review changes before merging to main.

## Open Items

- Fix Supabase site URL in dashboard (currently localhost:3000, needs to be staging URL)
- Add error boundary component for graceful crash handling
- Write end-to-end tests for the invite flow

## Next Session

Fix the magic link redirect URL in Supabase dashboard settings, verify email flow works on staging, then deploy to production.

## Blockers

Magic link redirect - quick fix in Supabase dashboard, no code change needed.
