---
name: daily-digest
description: Researches three configured topics, returns 5 bullets per topic in a Telegram-friendly format. Use when running the morning digest or when explicitly asked for "today's digest".
version: 1.0.0
metadata:
  hermes:
    tags: [news, research, automation, digest]
    category: personal
---

# Daily Digest

## When to Use

Run this skill when:

- The morning cron job fires (the prompt will say "run the daily-digest skill")
- The user asks for "today's digest" or similar in any chat
- The user asks to research the configured topics ad-hoc

## Topics

Research these three topics. The "brief" describes what counts as
relevant — be strict; reject filler.

### 1. EU AI Act updates

Most important developments in the past 7 days regarding the EU AI Act:
enforcement actions, guidance documents, member-state implementation,
court rulings, or significant industry compliance moves. Skip generic
explainers and opinion pieces.

### 2. M&E contractor news (UK construction)

News from the past 7 days about Mechanical and Electrical contractors
in the UK construction sector: major project wins, insolvencies, payment
disputes, regulatory changes affecting M&E, and notable housing
development tenders. Focus on commercial relevance to mid-size M&E firms
(£5m–£50m turnover).

### 3. Agentic AI launches

New agentic AI products, frameworks, or significant research releases
from the past 7 days. Prioritise things that ship code or runnable
agents over thinkpieces. Note who released it and what's actually new.

## Procedure

1. For each of the three topics in order:
   a. Use the `web_search` tool to find recent items. Use 3–5 searches
   per topic — vary the query so you find different angles, but
   don't spelunk; this is a digest, not a research paper.
   b. Pick the 5 most important items. Bias toward primary sources
   (regulator, company, court) over commentary.
   c. Write each item using the format below.

2. Assemble the full digest in this exact structure (Markdown, since
   Telegram renders it):

```
   ☀️ *Hermes — <weekday> <DD Mon YYYY>*

   *EU AI Act updates*

   1. <One-sentence headline> — <Source name, date>
      <One-sentence "so what" — why it matters>
   2. ...
   3. ...
   4. ...
   5. ...

   *M&E contractor news (UK construction)*

   1. ...
   ...

   *Agentic AI launches*

   1. ...
   ...
```

3. Deliver the digest to the home channel via the Telegram messaging
   tool. Send it as a single message if possible; if it exceeds
   4096 characters, split per-topic.

## Pitfalls

- **Generic AI-slop language.** Reject phrases like "this represents a
  significant development", "in an evolving landscape", "underscores
  the importance of". The "so what" lines must be blunt and specific
  to the item.
- **Missing the date filter.** If web_search returns items older than
  7 days, drop them — even if they're high-quality. Recency is the
  whole point of a daily digest.
- **Padding.** If a topic genuinely has fewer than 5 strong items, use
  the weakest slots for relevant context/background pieces and label
  them as such. Don't invent items.
- **URLs in bullets.** Don't include URLs in the bullet text — they
  clutter Telegram. Source name + date is enough.
- **Token blow-up.** Cap web_search at ~5 calls per topic. If you
  haven't found 5 strong items in 5 searches, ship what you have.

## Verification

The digest is correctly formed when:

- It has exactly 3 topic sections, in the order above.
- Each section has 5 numbered items.
- Each item is under 280 characters total.
- The header includes today's date.
- It posted to the configured home channel without errors.
