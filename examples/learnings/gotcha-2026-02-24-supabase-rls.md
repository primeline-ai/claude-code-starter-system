---
type: gotcha
date: 2026-02-24
tags: [supabase, rls, security]
---

# Supabase RLS: Always Test With Multiple Roles

RLS policies that check team membership are not enough for row-level operations. A policy like `user_id IN (SELECT user_id FROM team_members WHERE team_id = tasks.team_id)` allows ANY team member to delete ANY task in the team.

**Fix:** Add ownership check for destructive operations: `auth.uid() = tasks.created_by` for DELETE and UPDATE on sensitive fields.

**Testing rule:** Always test RLS with three users - task owner, team member (non-owner), and outsider. If all three can do the same thing, the policy is wrong.
