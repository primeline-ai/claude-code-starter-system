---
type: decision
date: 2026-02-27
tags: [supabase, deployment, infrastructure]
---

# Separate Supabase Projects for Dev vs Production

Using one Supabase project for dev and production causes problems: magic links point to wrong URLs, RLS changes affect live users during testing, and test data pollutes production.

**Decision:** Create a separate Supabase project for each environment. Use environment variables to switch between them. Extra cost is minimal (free tier covers both) and the isolation prevents a whole class of deployment bugs.
