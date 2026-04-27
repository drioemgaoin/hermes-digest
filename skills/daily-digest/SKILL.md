---
name: daily-digest
description: Research 3 topics, post 5 bullets each to Telegram. Triggered by morning cron or "today's digest" request.
version: 1.2.0
metadata:
  hermes:
    tags: [news, research, automation, digest]
    category: personal
---

# Daily Digest

## Topics

Research these three topics. Be strict — reject filler.

### 1. AI tools & agentic workflows

Past 7 days: new AI dev tools, agentic frameworks, IDE integrations,
AI coding assistants, prompt engineering techniques, and workflow
automation releases. Prioritise shipped products and runnable code
over opinion pieces. Note who released it and what's new.

### 2. Crypto (builder-focused)

Past 7 days: protocol launches, smart contract tooling, DeFi
infrastructure, SDKs, developer platforms, L2 updates, and notable
open-source releases. Skip price speculation and exchange drama.
Focus on what matters to builders.

### 3. Developer tooling & infrastructure

Past 7 days: new dev tools, language/runtime releases, CI/CD updates,
cloud platform changes, open-source project milestones, and DX
improvements. High signal for working developers.

## Procedure

For each topic in order:

1. Run 3–4 `web_search` calls with varied phrasing. Examples:
   - AI tools: `"AI dev tools released {month} {year}"`, `"agentic AI framework launch"`
   - Crypto: `"crypto developer tools this week"`, `"DeFi protocol launch OR SDK"`
   - Dev tooling: `"developer tools release this week"`, `"programming language update {year}"`
2. From snippet text alone, pick the 5 most important items from the past 7 days. Drop anything older. Prefer primary sources over commentary.
3. If <5 strong items after 4 searches, ship what you have. Label weak slots "thin coverage". Never invent items.

Assemble the full digest in this exact format:

```
☀️ *Hermes — <weekday> <DD Mon YYYY>*

*AI tools & agentic workflows*

1. <Headline> — <Source, date>
   <One blunt "so what" sentence>
2. ...
3. ...
4. ...
5. ...

*Crypto (builder-focused)*

1. ...
...

*Developer tooling & infrastructure*

1. ...
...
```

Rules:
- Each item under 280 characters. No URLs — source name + date only.
- Ban slop: "significant development", "evolving landscape", "underscores the importance". Be blunt and specific.
- Send to home channel via Telegram. Single message; split per-topic if >4096 chars.

## Checklist

Verify before sending:

- [ ] 3 topic sections in order above
- [ ] 5 numbered items each (or labelled thin coverage)
- [ ] Each item <280 chars
- [ ] Today's date in header
- [ ] No URLs in bullet text
- [ ] Posted to home channel
