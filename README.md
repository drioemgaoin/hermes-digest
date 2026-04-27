# Hermes Daily Digest Agent

**An AI agent that wakes up every morning, reads the news for you, and sends a summary straight to your phone.**

If that sounds like having a personal intern who never sleeps, never complains, and costs about the same as a fancy coffee per month — congratulations, you've got the right idea.

---

## What This Project Actually Does (The Part You'd Tell Your Mum)

Every morning at 8am, a small AI agent living on a cheap server wakes up, searches the internet for news on three topics you care about, boils everything down to five bullet points per topic, and fires the whole thing to your Telegram (or Slack). Then it goes back to sleep. You read it over breakfast. That's it. That's the whole project.

The three topics it watches (you can change these, obviously):

1. **EU AI Act updates** — enforcement actions, new guidance, member-state moves
2. **M&E contractor news (UK construction)** — project wins, insolvencies, payment disputes, tenders
3. **Agentic AI launches** — new frameworks, tools, and products that actually ship code

The digest looks like this in your Telegram:

<!-- PLACEHOLDER: Screenshot of a Telegram message showing the daily digest format. Capture a real digest from your phone — ideally a few days' worth of messages in the chat thread to show the "building up over time" effect. -->

---

## Why Should You Care? (The "So What" Section)

Most AI demos live in a Jupyter notebook and die there. This one runs on a real server, on a real schedule, delivering real information to a real phone. Every single day. Without you lifting a finger.

That distinction — "I built a thing that _runs in production_" versus "I built a thing that works on my laptop" — is the difference employers and clients notice.

**What this project proves you can do:**

- Deploy an AI agent to a live server (not just run it locally)
- Schedule automated tasks with cron
- Wire up a messaging gateway (Telegram/Slack)
- Write a custom Hermes skill from scratch
- Keep the whole thing running reliably over time

---

## The Tech Stack (Don't Panic)

| Piece                   | What It Is                                                 | Why It's Here                                       |
| ----------------------- | ---------------------------------------------------------- | --------------------------------------------------- |
| **Hermes Agent**        | An open-source AI agent by Nous Research                   | The brains — it searches, summarises, and delivers  |
| **A VPS**               | A small virtual server (Hetzner, Contabo, or DigitalOcean) | Where the agent lives and runs 24/7                 |
| **Telegram** (or Slack) | Messaging app on your phone                                | How the digest reaches you                          |
| **An LLM API key**      | Anthropic (Haiku recommended)                              | The language model that powers the agent's thinking |
| **Cron**                | The oldest scheduling trick in Unix                        | Wakes the agent up every morning at 8am             |

**Monthly cost:** roughly £5 for the VPS + £1-2 in LLM tokens. Less than a Netflix subscription.

---

## Project Structure (What's in the Box)

```
hermes-digest/
├── README.md                  ← You are here
└── skills/
    └── daily-digest/
        └── SKILL.md           ← The brain of the operation
```

The `SKILL.md` file is where the magic happens. It's a Markdown file that tells the Hermes agent _exactly_ how to research your topics, _exactly_ how to format the output, and _exactly_ where to send it. Think of it as a very detailed instruction manual for a very obedient robot.

<!-- PLACEHOLDER: Annotated screenshot of the SKILL.md file open in your editor, with callouts pointing to the key sections (Topics, Procedure, Pitfalls). If you use VS Code, the Markdown preview side-by-side looks great. -->

---

## Setting It All Up (The Step-by-Step Part)

Don't worry — we'll take this one bite at a time. No step assumes you know what the previous one did internally. You just need to type the commands and watch the magic happen.

### Step 1: Get Yourself a Server

You need a small Linux VPS. Any of these will do:

- **Hetzner** (cheapest, EU-based)
- **Contabo** (good bang for the buck)
- **DigitalOcean** (nicest dashboard, slightly pricier)

Pick the smallest tier — 1 CPU, 1-2GB RAM is plenty. You're running a script once a day, not hosting Netflix.

Once it's up, SSH into it:

```bash
ssh root@your-server-ip
```

<!-- PLACEHOLDER: Screenshot of your terminal SSHed into the VPS, showing the welcome banner. Bonus points if it shows the server's hostname. -->

### Step 2: Install Hermes Agent

One command. Seriously.

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Then reload your shell so the `hermes` command works:

```bash
source ~/.bashrc
```

Verify it installed:

```bash
hermes doctor
```

If `hermes doctor` comes back happy, you're golden. If it complains, it'll tell you exactly what's wrong — fix that before moving on.

<!-- PLACEHOLDER: Screenshot of `hermes doctor` output showing a healthy install. -->

### Step 3: Configure Your LLM Provider

```bash
hermes model
```

This walks you through picking a provider. For a daily digest running on autopilot, **Anthropic's Haiku model** is the sweet spot — cheap, fast, and smart enough for news summarisation.

```bash
hermes config set model anthropic/claude-haiku-4-5-20251001
hermes config set ANTHROPIC_API_KEY sk-ant-your-key-here
```

> **The Dummies Tip:** You get your API key from [console.anthropic.com](https://console.anthropic.com). It starts with `sk-ant-`. Guard it like a password — anyone with this key can run up your bill.

### Step 4: Set Up the Telegram Gateway

This is the part where your server learns to text you.

1. **Create a Telegram bot** — open Telegram, search for `@BotFather`, send `/newbot`, follow the prompts. You'll get a bot token.
2. **Tell Hermes about it:**

```bash
hermes gateway setup
```

Pick Telegram when prompted, paste your bot token.

3. **Start a chat with your new bot** on Telegram and send it `/start`.
4. **Check the gateway is alive:**

```bash
hermes gateway status
```

<!-- PLACEHOLDER: Screenshot of the BotFather conversation in Telegram where you create the bot and receive the token. Blur or redact the actual token. -->

<!-- PLACEHOLDER: Screenshot of `hermes gateway status` showing Telegram connected. -->

### Step 5: Install the Daily Digest Skill

Copy the skill from this repo into your Hermes skills directory:

```bash
cp -r skills/daily-digest ~/.hermes/skills/
```

Verify Hermes can see it:

```bash
hermes
# then inside the chat, type:
/skills
```

You should see `daily-digest` in the list.

<!-- PLACEHOLDER: Screenshot of the `/skills` output inside a Hermes chat session, with `daily-digest` visible in the list. -->

### Step 6: Test It (Before You Automate It)

Always test before you automate. Always. Open a Hermes session and ask it to run the skill manually:

```bash
hermes
```

Then type:

```
Run the daily-digest skill
```

Watch it search, summarise, and (if the gateway is set up) send the digest to your Telegram. If the message arrives on your phone, do a little victory dance. You've earned it.

<!-- PLACEHOLDER: Screenshot of your phone showing the first test digest arriving in Telegram. This is the "money shot" — make it look good. -->

### Step 7: Schedule It with Cron

Now we tell the server to run this every morning at 8am, no human required.

```bash
crontab -e
```

Add this line:

```
0 8 * * * /path/to/hermes -e "Run the daily-digest skill" --gateway telegram 2>&1 >> /var/log/hermes-digest.log
```

> **The Dummies Tip:** That `0 8 * * *` means "at minute 0 of hour 8, every day of every month, every day of the week." Cron syntax looks like someone fell asleep on their keyboard, but it works beautifully once you've got it right.

> **Important:** Replace `/path/to/hermes` with the actual path where Hermes was installed. Run `which hermes` to find it.

Verify cron is running:

```bash
crontab -l
```

<!-- PLACEHOLDER: Screenshot of `crontab -l` showing the scheduled job. -->

---

## Customising Your Topics (Make It Yours)

The topics are defined in `skills/daily-digest/SKILL.md` under the `## Topics` section. Each topic has a name and a "brief" that tells the agent what counts as relevant.

To change a topic, just edit the SKILL.md:

```bash
nano ~/.hermes/skills/daily-digest/SKILL.md
```

For example, swap out "M&E contractor news" for "Renewable energy project tenders in Scotland" — the agent doesn't care. It'll search for whatever you tell it to search for.

**Rules of thumb for good topics:**

- Be specific. "AI news" is too broad. "New open-source AI agent frameworks released this week" is just right.
- Include what to _exclude_. "Skip opinion pieces and generic explainers" saves the agent from wasting your time.
- Keep it to 3 topics. More than that and the digest stops being a quick breakfast read.

---

## The Stretch Goal: The `/dig` Command

Want to go deeper on a specific digest item? The stretch goal is to reply to any bullet in the digest with `/dig` and have the agent do a proper deep-dive research pass on that one item.

This is left as an exercise — but here's the shape of it:

1. Create a new skill called `deep-dive` in `skills/deep-dive/SKILL.md`
2. The skill takes one input: the headline text from a digest item
3. It runs 10+ searches, reads primary sources, and produces a 1-page briefing
4. It sends the result back to Telegram as a reply to the original message

<!-- PLACEHOLDER: If you implement the /dig command, add a screenshot of the Telegram thread showing a digest bullet followed by a "/dig" reply and the deep-dive response. -->

---

## Troubleshooting (When Things Go Sideways)

Because things _will_ go sideways. That's not a bug, it's computers.

| Symptom                     | What's Probably Wrong       | Fix                                                                    |
| --------------------------- | --------------------------- | ---------------------------------------------------------------------- |
| `hermes doctor` fails       | Installation incomplete     | Re-run the install script                                              |
| No Telegram message arrives | Gateway not running         | `hermes gateway status`, then `hermes gateway setup` again             |
| Digest is empty or generic  | LLM can't access the web    | Check `web_search` tool is enabled: `/tools` in a chat                 |
| Cron doesn't fire           | Wrong path to hermes binary | Run `which hermes` and update the crontab                              |
| API errors                  | Key expired or quota hit    | Check [console.anthropic.com](https://console.anthropic.com) for usage |
| Agent hallucinates old news | Date filter not working     | The SKILL.md already handles this — check the Pitfalls section         |

For anything else:

```bash
hermes doctor          # diagnostics
hermes gateway status  # is the bot alive?
hermes sessions list   # are sessions saving?
```

---

## What It Looks Like After a Week

<!-- PLACEHOLDER: Screenshot or screen recording of your Telegram chat with the bot, showing 5-7 days of daily digests stacked up. This is the "proof it runs in production" shot that makes the portfolio piece convincing. Scroll through the chat slowly if you're doing a screen recording. -->

---

## Cost Breakdown (Because You Asked)

| Item                               | Monthly Cost    |
| ---------------------------------- | --------------- |
| VPS (smallest tier)                | ~£5             |
| LLM tokens (Haiku, one digest/day) | ~£1-2           |
| Telegram                           | Free            |
| **Total**                          | **~£6-7/month** |

That's less than two coffees. For a personal news agent that never takes a day off.

---

## What You'll Learn Along the Way

Even if you never touch this project again after the initial setup, you'll have picked up:

- **How to deploy and operate an AI agent in production** — not just prototype one
- **The "agent that runs while you sleep" pattern** — scheduled automation with multi-channel delivery
- **How Hermes skills work** — portable Markdown files that encode domain knowledge
- **How messaging gateways bridge servers and phones** — the Telegram bot pattern applies everywhere
- **How cron scheduling works** — the unglamorous backbone of half the internet

---

## License

MIT — do whatever you want with it.

---

_Built with [Hermes Agent](https://hermes-agent.nousresearch.com) by Nous Research. Powered by good coffee and mild stubbornness._
