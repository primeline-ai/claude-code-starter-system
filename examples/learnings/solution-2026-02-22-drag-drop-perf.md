---
type: solution
date: 2026-02-22
tags: [react, performance, drag-and-drop]
---

# Drag-and-Drop Performance Fix

When implementing drag-and-drop with large lists (200+ items), avoid updating global state on every position change during the drag. This causes full re-renders of all items.

**Fix:** Use local component state for the drag preview position. Only sync to global state on drop. Memoize list items with `React.memo` to prevent re-renders of items that haven't moved.

Before: 200ms+ render per drag frame. After: <16ms (smooth 60fps).
