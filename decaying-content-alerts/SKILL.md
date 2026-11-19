---
name: decaying-content-alerts
description: Flag Zenskar pages that are losing search ranking or clicks before they fall off page one. Trigger whenever the user uploads a multi-month Search Console export (ideally monthly, 3+ months) and asks about decaying content, pages losing ground, content that needs a refresh, or wants a proactive health check rather than a reaction to an already-known drop.
---

# Decaying Content Alerts

Catch pages sliding toward page two while they're still recoverable, not after they've already fallen off page one.

## Required input

A Search Console export with monthly granularity covering at least the last 3-6 months, with clicks, impressions, and average position per page (query-level detail helps but page-level trend is the minimum).

## Process

1. **Compute a 3-month trendline per page** for average position, clicks, and impressions.
2. **Flag three decay patterns**, each treated differently:
   - **Position decay**: average position worsening month over month, even gradually (e.g., 4.1 → 5.3 → 6.8). Most urgent when the page is still in positions 3-8 — this is the "one rewrite away from recovering top 3" zone. Flag before it crosses page one (position ~10).
   - **CTR rot**: impressions flat or growing, but clicks declining — position may look stable but something (a new SERP feature, a competitor's more compelling title, an AI overview) is siphoning clicks. This needs a title/meta/snippet fix, not a content rewrite.
   - **Slow slide from page one to page two**: positions 8-14, moving in the wrong direction. Lower urgency than the 3-8 zone but worth queuing.
3. **If publish/update dates are available** (ask the user for these if not in the export — check the CMS or ask directly), cross-reference: pages untouched for 12+ months decaying in parallel suggest a refresh cadence problem, not isolated page issues.
4. **Rank by recoverability**, not just severity. A page at position 6 sliding toward 9 is higher priority than a page already buried at position 40 — the former is fixable with a refresh, the latter likely needs a different strategy entirely.

## Output format (always)

```
FINDING: [N] pages in active decay; [M] are in the 3-8 position range and
recoverable with a single refresh.

EVIDENCE:
- [URL]: avg position [trend, e.g. "4.1 -> 5.3 -> 6.8 over 3 months"]. Clicks
  [trend]. Likely cause: [position decay / CTR rot / slow slide].
- ...

ACTION PLAN (ranked by recoverability):
1. [URL] — [specific refresh action, e.g., "update the pricing comparison table,
   competitor moved theirs 2 months ago"]. Flagged: one rewrite from top 3.
2. ...
```

## Rules

- Distinguish CTR rot from position decay explicitly in every finding — they take different fixes (title/snippet vs. content depth).
- Flag pages in the 3-8 position band as highest priority regardless of raw click volume — that's the recoverable zone.
- If publish-date data isn't available, say so and recommend pulling it before setting a refresh cadence, rather than guessing at content age.
