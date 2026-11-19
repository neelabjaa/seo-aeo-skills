---
name: cannibalization-cleanup
description: Find and resolve keyword cannibalization across Zenskar's pages using Search Console query+page exports. Trigger whenever the user uploads or pastes GSC data and asks about cannibalization, duplicate rankings, "two pages competing," which page to keep for a keyword, or when the traffic-drop-diagnostics skill flags a cannibalization pattern and hands off here.
---

# Cannibalization Cleanup

Find keywords where multiple Zenskar URLs compete for the same query, and give a specific KEEP/MERGE/REDIRECT/DIFFERENTIATE call for each.

## Required input

A Search Console query + page export (clicks, impressions, position per query-page pair). Optionally, an internal link map — if provided, use it to recommend which page internal links should consolidate toward.

## Process

1. **Group by query.** For each query, list every distinct Zenskar URL that received impressions for it.
2. **Filter to real cannibalization.** A query with 2+ URLs both getting meaningful impressions (not one URL with 1 impression) is a candidate. Ignore incidental overlap — e.g., a homepage picking up 1-2 impressions on a query a dedicated page owns isn't worth resolving.
3. **For each real case**, determine intent match:
   - If both URLs target the same intent and audience — this is true cannibalization. Pick the rightful target (better position, more clicks, more topically precise URL/title) to KEEP.
   - If the URLs actually serve different intents that happen to share a query (e.g., a blog post and a comparison page both ranking for "X alternative") — recommend DIFFERENTIATE: retarget one page's on-page signals (title, H1, internal links) to a more specific long-tail variant instead of merging.
4. **Decide the action** for the non-kept URL(s):
   - **MERGE**: content overlaps heavily, both are thin — combine into the KEEP page, 301 the other.
   - **REDIRECT**: the other URL has little unique value and near-zero traffic of its own — 301 straight to the KEEP page.
   - **DIFFERENTIATE**: both pages have distinct value — retarget the losing page to a more specific keyword variant and update its title/H1/meta accordingly, no redirect.
5. **If an internal link map was provided**, name which pages currently link to the URL being merged/redirected, and note they should be repointed to the KEEP URL.

## Output format (always)

```
FINDING: [N] cannibalized keywords found, representing [X] total clicks at stake.

EVIDENCE (ranked by clicks at stake):
1. "[query]" — [URL A] (pos X, Y clicks) vs [URL B] (pos X, Y clicks)
2. ...

ACTION PLAN:
1. "[query]": KEEP [URL A]. [MERGE/REDIRECT/DIFFERENTIATE] [URL B] — [exact action,
   e.g., "301 into URL A" or "retarget URL B's title to '[specific long-tail]'"].
2. ...
```

## Rules

- Order everything by total clicks at stake, highest first — that's where the effort pays off.
- Never recommend merging two pages that serve genuinely different funnel stages (e.g., a TOFU guide and a BOFU comparison page) just because they share a keyword — differentiate instead.
- State the exact URL to keep and the exact mechanism (301, retarget, no action) — "consolidate these pages" is not an actionable output.
