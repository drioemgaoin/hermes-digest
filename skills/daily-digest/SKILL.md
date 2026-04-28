---
name: daily-digest
description: Research 3 agentic AI topics, post 5 bullets each to Telegram. Triggered by morning cron or "today's digest" request.
version: 1.4.0
metadata:
  hermes:
    tags: [news, research, automation, digest]
    category: personal
---

# Daily Digest

## Topics

Research these three topics. Be strict — reject filler.

### 1. New agents & frameworks

Past 7 days: new AI agent releases, agentic frameworks, agent SDKs,
chat agents, coding agents (Claude Code, Hermes, Cursor, etc.), and
multi-agent orchestration tools. Prioritise shipped products and
runnable code over hype posts. Note who released it and what changed.

### 2. Agent skills, rules & integrations

Past 7 days: new agent skills/plugins, MCP servers, tool-use patterns,
rules files and conventions (.cursorrules, CLAUDE.md, etc.), prompt
templates, and integration protocols. Skip generic prompt-engineering
listicles. Focus on reusable building blocks for agentic systems.

### 3. Agentic engineering craft

Past 7 days: practical techniques for building better agents —
prompt/markdown structure (system prompts, CLAUDE.md, rules files),
token efficiency (context management, chunking, summarisation),
workflow & orchestration patterns (multi-step, handoffs, retries),
and agent evaluation (evals, benchmarks, observability).
Skip hype and theory. Prefer practitioner write-ups, code examples,
and measurable results.

## Procedure

For each topic in order:

1. Run 3–4 `web_search` calls with varied phrasing. Examples:
   - Agents & frameworks: `"new AI agent release {month} {year}"`, `"agentic framework launch this week"`
   - Skills & rules: `"MCP server new {year}"`, `"agent rules conventions OR skills plugin"`
   - Craft: `"agent prompt engineering patterns {year}"`, `"token optimization LLM agent"`, `"agentic workflow design"`, `"agent eval benchmark {month} {year}"`
2. From snippet text alone, pick the 5 most important items from the past 7 days. Drop anything older. Prefer primary sources over commentary.
3. If <5 strong items after 4 searches, ship what you have. Label weak slots "thin coverage". Never invent items.

Assemble the full digest in this exact format:

```
☀️ *Hermes — <weekday> <DD Mon YYYY>*

*New agents & frameworks*

1. <Headline> — <Source, date>
   <One blunt "so what" sentence>
2. ...
3. ...
4. ...
5. ...

*Agent skills, rules & integrations*

1. ...
...

*Agentic engineering craft*

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
