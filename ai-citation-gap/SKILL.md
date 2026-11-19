---
name: ai-citation-gap
description: Diagnose why Zenskar does or doesn't get recommended when buyers ask ChatGPT, Perplexity, Claude, or Gemini category questions like "best billing platform for usage-based pricing" or "Chargebee alternatives." Trigger whenever the user asks about AI search visibility, LLM citations, being recommended in ChatGPT/Perplexity, "share of voice" in AI answers, or GEO (generative engine optimization) — this is the AI-search layer, distinct from Google ranking work.
---

# AI Citation Gap

Find out who gets recommended in AI answers for Zenskar's category, and diagnose why Zenskar is or isn't among them.

## Required input

The competitor set and category prompts from this project's knowledge file. Use web_search with real search queries — do not answer from memory, since AI-search visibility changes fast and stale training data will misrepresent current citation patterns.

## Process

1. **Generate or use the buyer-intent prompts** someone would type into ChatGPT/Perplexity when shopping in this category. Start from the ones already logged in the knowledge file ("best Tabs alternative," "Lago alternatives," "Nue vs Zenskar," etc.) and add any missing high-intent variants (e.g., "best billing platform for usage-based B2B SaaS," "[competitor] alternatives for enterprise").
2. **For each prompt, search for what's currently being surfaced** — check recent comparison articles, review-aggregator pages (G2, Capterra, SourceForge, GetApp), and Reddit/Quora threads that would plausibly be feeding an LLM's answer. These sources are what AI answers actually draw from, not the brand's own site.
3. **Determine whether Zenskar shows up** in that source material for each prompt, and where it ranks relative to competitors when it does.
4. **Where Zenskar is missing, diagnose the specific cause**:
   - **No authoritative page**: Zenskar has no comparison/alternative page targeting this exact prompt (cross-check against content-gap-analysis output if available).
   - **Weak entity signals**: Zenskar's own site exists but lacks the specific claims (customer counts, integration counts, compliance certifications) that let a model confidently cite it.
   - **Absent from the sources models pull from**: third-party comparison sites, Reddit, Quora, or G2/Capterra either don't mention Zenskar or mention it thinly compared to competitors — hand off to reddit-quora-geo for the community-thread half of this.
5. **Rank the fixes** by how many prompts each would move and how fast (a missing comparison page you can ship this week beats a long-term authority-building play).

## Output format (always)

```
FINDING: Zenskar shows up in [X] of [N] tested buyer-intent prompts; the biggest
gap is [specific prompt/reason].

EVIDENCE:
1. "[prompt]" — currently recommends: [competitors, in order]. Zenskar: [missing /
   present at position N]. Source pulling the answer: [G2 / Reddit thread / blog].
2. ...

ACTION PLAN (ranked by speed x impact):
1. [specific move, e.g., "Publish /comparison/nue-vs-zenskar — third-party sites
   already compare the two, Zenskar has no page capturing that demand"].
2. ...
```

## Rules

- Always search before answering — this skill is meaningless run from stale knowledge, since AI-search citation patterns shift week to week.
- Distinguish clearly between "no page exists" (a content fix) and "page exists but isn't being cited" (an authority/entity-signal problem) — they need different action types.
- Don't claim a specific LLM's live citation behavior without a real search result to back it — describe what the current source material supports, not a guess at what ChatGPT would say today.
