# Clawdbot Setup Guide

This guide walks you through setting up your Clawdbot workspace to work with Cerebrum. By the end, you'll have a personalized AI assistant with skills for task management, content creation, and more.

---

## Prerequisites

- **Clawdbot installed** — [Installation guide](https://docs.clawd.bot)
- **Cerebrum cloned** — This repo, opened in Obsidian
- **Node.js 18+** — For running skill scripts
- **Python 3.11+** — For some optional skills (yahoo-finance, parallel)

---

## Part 1: Create Your Clawd Workspace

Create a separate directory for your Clawdbot workspace:

```bash
mkdir -p ~/clawd/{memory,skills,scripts}
```

### Set Environment Variable

Add to your `~/.zshrc` or `~/.bashrc`:

```bash
export CEREBRUM_PATH="$HOME/cerebrum"  # Path to your Cerebrum vault
```

Reload your shell:
```bash
source ~/.zshrc
```

---

## Part 2: Generate Core Files

Run Clawdbot and paste the following prompt. It will ask you questions to create personalized configuration files.

### Generate All Core Files

```
I need you to help me set up my Clawdbot workspace. Ask me questions one at a time to understand who I am, then generate these files:

1. **AGENTS.md** — Behavioral rules for how you should operate
2. **SOUL.md** — Your personality and voice
3. **USER.md** — Information about me (the human)
4. **MEMORY.md** — Starting point for long-term memories
5. **TOOLS.md** — Notes about my tools and accounts
6. **IDENTITY.md** — Your name, emoji, and avatar description
7. **HEARTBEAT.md** — What to check during periodic heartbeats

Ask me about:
- My name, location, timezone
- What I do for work
- My current priorities and goals
- Communication preferences (casual vs formal, emoji usage)
- What personality traits I want my AI assistant to have
- What name I want to give my assistant
- What tools/apps I use daily
- What I want you to proactively check on

After gathering info, generate each file with proper formatting. Save them to ~/clawd/

Important paths to include in the generated files:
- Cerebrum vault: $CEREBRUM_PATH (or the actual path I provide)
- Daily notes: $CEREBRUM_PATH/Daily/
- Prompts: $CEREBRUM_PATH/Prompts/
```

---

## Part 3: Browser Setup (Important!)

The X-growth skill requires browser access for X/Twitter. You have two browser profiles:

### Profile: `chrome` (Your logged-in browser)

Used for:
- X/Twitter (requires login to avoid rate limits)
- Any site where you need your logged-in session

**Setup:**
1. Install the **Clawdbot Browser Relay** Chrome extension
2. When you want Clawdbot to access a tab, click the extension icon (badge turns ON)
3. Clawdbot can now see and interact with that tab

**Usage in skills:**
```
browser action=navigate url="https://x.com/..." profile=chrome
browser action=snapshot profile=chrome
```

### Profile: `clawd` (Isolated browser)

Used for:
- General web research
- Sites that don't need login
- Automation tasks

**No setup needed** — Clawdbot manages this browser automatically.

**Usage in skills:**
```
browser action=navigate url="https://example.com" profile=clawd
browser action=snapshot profile=clawd
```

### Quick Reference

| Task | Profile | Notes |
|------|---------|-------|
| X/Twitter anything | `chrome` | Click extension icon first |
| Web research | `clawd` | Safe, isolated |
| Logged-in sites | `chrome` | Attach extension to tab |
| Screenshot/scraping | `clawd` | No personal data |

---

## Part 4: Generate Skills

Skills teach Clawdbot how to do specific tasks. Run these prompts to generate personalized skills.

### Skill 1: Task Capture

Quick capture tasks to your daily notes.

```
Create a Clawdbot skill called "task" that captures tasks to my Obsidian daily notes.

Requirements:
- Trigger: /task <description> or when I say "add task" / "remind me to"
- Parse the task for: description, domain (Personal or Work), due date if mentioned
- Domain detection:
  - Personal: health, family, home, errands, personal projects
  - Work: [ASK ME what keywords should map to Work]
- Format: `- [ ] Task description 📅 YYYY-MM-DD` (date only if specified)
- Add to today's daily note at: $CEREBRUM_PATH/Daily/YYYY-MM-DD.md
- Create daily note from template if it doesn't exist
- Confirm: "✅ Added to [Domain]: Task description"

Ask me:
1. What keywords should trigger the "Work" domain?
2. What's my typical daily note template structure?
3. Any other task formatting preferences?

Then generate the SKILL.md file for ~/clawd/skills/task/SKILL.md
```

### Skill 2: Weekly Review

Generate weekly summaries from your daily notes.

```
Create a Clawdbot skill called "weekly-review" that generates a weekly summary.

Requirements:
- Trigger: /weekly-review or "what did I do this week"
- Read daily notes from the past 7 days: $CEREBRUM_PATH/Daily/
- Parse completed tasks (- [x]) and incomplete tasks (- [ ])
- Group by domain: Personal, Work
- Generate a structured summary with:
  - Accomplishments per domain
  - Task completion stats
  - Incomplete tasks to carry forward
  - AI-generated insights on patterns
- Save to: $CEREBRUM_PATH/Meta/Reviews/YYYY-WXX.md

Ask me:
1. What format do you prefer for weekly reviews?
2. Any specific insights you want me to look for?
3. Should I include recommendations for next week?

Then generate the SKILL.md file for ~/clawd/skills/weekly-review/SKILL.md
```

### Skill 3: X-Growth (Content Creation)

Your marketing assistant for X/Twitter.

```
Create a Clawdbot skill called "x-growth" for X/Twitter content creation.

Requirements:
- Commands:
  - /learn <url> — Analyze a post I like, extract patterns, update tone files
  - /draft <topic> — Create 3 post drafts using my learned tone
  - /reply <url> — Create 5 reply options matching thread vibe
  - /consolidate — Weekly cleanup of tone files

- Tone files location: $CEREBRUM_PATH/Prompts/
  - writing-tone.md (post voice)
  - reply-tone.md (reply voice)
  - hooks.md (opening patterns)
  - closings.md (ending patterns)
  - phrases.md (structural patterns)
  - anti-patterns.md (what to avoid)
  - examples/ (saved analyzed posts)

- Browser rules:
  - X/Twitter: ALWAYS use profile="chrome" (needs extension attached)
  - Other URLs: use profile="clawd"

- /learn workflow:
  1. Open URL in browser, snapshot
  2. For X posts: scroll to capture top 10-15 replies
  3. Analyze: hook type, structure, phrases, what worked
  4. Update relevant tone file with new pattern
  5. Save full example to examples/YYYY-MM-DD-slug.md
  6. Log change to Meta/Changelogs/tone-evolution.md

- /draft workflow:
  1. Load all tone files
  2. Generate 3 drafts: Punchy (short), Thoughtful (depth), Contrarian (hot take)
  3. Handle feedback to improve future drafts

- /reply workflow:
  1. Open post in Chrome, snapshot with replies
  2. Analyze thread vibe (playful, serious, technical, etc.)
  3. Generate 5 different reply styles:
     - Ultra short (1-3 words)
     - Playful/witty
     - Insightful/technical
     - Self-deprecating
     - Question/curiosity
  4. Match the energy of top-performing replies

Generate the SKILL.md file for ~/clawd/skills/x-growth/SKILL.md
```

### Skill 4: Humanizer (Writing Editor)

Remove AI patterns from your writing.

```
Create a Clawdbot skill called "humanizer" that removes AI writing patterns.

Base it on Wikipedia's "Signs of AI writing" guide. Include patterns like:
- Undue emphasis on significance ("serves as a testament", "pivotal moment")
- Superficial -ing analyses ("highlighting", "showcasing", "fostering")
- Promotional language ("vibrant", "groundbreaking", "nestled")
- AI vocabulary overuse ("delve", "landscape", "tapestry", "crucial")
- Rule of three abuse
- Em dash overuse
- Sycophantic tone ("Great question!", "I'd be happy to help!")

Now ask me questions to create a personalized "MY VOICE" section:
1. How would you describe your writing style in 3 words?
2. What words/phrases do YOU actually use? (casual expressions, technical terms)
3. What words do you NEVER use?
4. For social media: more casual or professional?
5. Emoji usage: never, rarely, sometimes, often?
6. Do you have any writing quirks or signature phrases?

Then generate the SKILL.md with:
- Full anti-pattern reference
- My personalized voice section
- Before/after examples in my voice

Save to ~/clawd/skills/humanizer/SKILL.md
```

### Skill 5: Search X (Twitter Search)

Real-time X/Twitter search using Grok.

```
Create a Clawdbot skill called "search-x" for searching X/Twitter via xAI's Grok.

Requirements:
- Trigger: "search x for", "search twitter", "find tweets about"
- Uses xAI API with grok-4-1-fast model
- Supports filters: --days (time range), --handles (specific accounts)
- Returns: tweet content, author, date, direct link

Setup:
- Requires XAI_API_KEY environment variable
- Get key at: https://console.x.ai

Ask me:
1. What topics do you typically search for on X?
2. Any accounts you frequently want to filter by?
3. Default time range preference (7 days, 30 days)?

Generate a script at ~/clawd/skills/search-x/scripts/search.js and SKILL.md

The skill should include the full xAI API integration code.
```

### Skill 6: Video Transcript Downloader

Get transcripts from YouTube and other video sites.

```
Create a Clawdbot skill called "video-transcript-downloader" for getting video transcripts.

Requirements:
- Trigger: "get transcript", "download video", "rip audio"
- Capabilities:
  - Get clean paragraph-style transcripts (default)
  - Download video/audio
  - Download subtitles
  - List available formats

- Dependencies: yt-dlp, ffmpeg (document installation)

Commands:
- transcript --url <url> [--timestamps] [--lang en]
- download --url <url> --output-dir ~/Downloads
- audio --url <url> --output-dir ~/Downloads
- formats --url <url>

Ask me:
1. Where should downloaded files go by default?
2. Preferred transcript format (clean paragraph vs timestamped)?
3. Any specific sites you frequently download from?

Generate scripts and SKILL.md for ~/clawd/skills/video-transcript-downloader/
```

### Skill 7: Reddit Search

Discover and search Reddit communities.

```
Create a Clawdbot skill called "reddit-search" for Reddit discovery.

Requirements:
- Trigger: "search reddit", "find subreddit", "reddit posts about"
- Commands:
  - info <subreddit> — Get subreddit details (subscribers, description)
  - search <query> — Find subreddits matching query
  - popular [limit] — List popular subreddits
  - posts <subreddit> [limit] — Get top posts from a subreddit

- No API key needed (uses public Reddit JSON endpoints)

Ask me:
1. What topics do you typically research on Reddit?
2. Any subreddits you frequently check?
3. Default number of posts to fetch?

Generate scripts and SKILL.md for ~/clawd/skills/reddit-search/
```

### Skill 8: Yahoo Finance

Stock and crypto data without API keys.

```
Create a Clawdbot skill called "yahoo-finance" for financial data.

Requirements:
- Trigger: "stock price", "check AAPL", "how's BTC doing"
- Uses yfinance Python library (no API key needed)
- Commands:
  - price <symbol> — Quick price check
  - quote <symbol> — Detailed quote
  - fundamentals <symbol> — PE, EPS, margins
  - earnings <symbol> — Earnings dates and history
  - compare <symbols> — Side-by-side comparison
  - search <query> — Find ticker symbols

Symbol formats:
- US: AAPL, MSFT, NVDA
- Crypto: BTC-USD, ETH-USD
- International: RELIANCE.NS (India NSE)

Dependencies: Python 3.11+, uv (Python package runner)

Ask me:
1. What stocks/crypto do you track regularly?
2. Any specific markets (US, India, crypto)?
3. What metrics matter most to you?

Generate a Python script with uv inline dependencies and SKILL.md for ~/clawd/skills/yahoo-finance/
```

### Skill 9: Parallel (Deep Research)

High-accuracy web research with citations.

```
Create a Clawdbot skill called "parallel" for deep web research using Parallel.ai.

Requirements:
- Trigger: "deep research", "research this", "find sources for"
- Uses Parallel.ai API (high accuracy, built for AI agents)
- Returns structured results with excerpts and citations

Setup:
- Requires PARALLEL_API_KEY
- Get key at: https://platform.parallel.ai

When to use:
- Fact-checking with evidence
- Company/person research
- Complex queries needing multi-hop reasoning
- When citations matter

Ask me:
1. What types of research do you typically do?
2. Preferred output format (summary vs detailed)?
3. Any topics you research frequently?

Generate a Python script and SKILL.md for ~/clawd/skills/parallel/

Include the full Parallel SDK integration.
```

---

## Part 5: Verify Setup

After generating all files, verify your workspace:

```bash
# Check structure
ls -la ~/clawd/
# Should see: AGENTS.md, SOUL.md, USER.md, MEMORY.md, TOOLS.md, IDENTITY.md, HEARTBEAT.md

ls -la ~/clawd/skills/
# Should see: task, weekly-review, x-growth, humanizer, search-x, video-transcript-downloader, reddit-search, yahoo-finance, parallel

ls -la ~/clawd/memory/
# Should be empty (will populate over time)
```

### Test Skills

```bash
# Test task capture
/task Buy groceries tomorrow

# Test weekly review
/weekly-review

# Test X search (requires XAI_API_KEY)
search x for "AI agents"

# Test transcript
get transcript https://youtube.com/watch?v=...
```

---

## Part 6: Optional Enhancements

### Todoist Integration

If you use Todoist, ask Clawdbot to enhance your task skill:

```
Enhance my task skill to sync with Todoist bidirectionally.

Requirements:
- After adding task to Obsidian, also create in Todoist
- Map domains to Todoist projects: Personal → "Personal", Work → "[my work project]"
- Sync completed tasks both ways

I have Todoist API token ready. Ask me for my project names, then update the skill.
```

### Automated Morning Brief

Set up a daily market/news summary:

```
Create a cron job that runs every morning at [TIME] and:
1. Summarizes my calendar for today
2. Checks for important unread emails
3. Gives me a brief on any topics I care about

Ask me what time and what I want included.
```

### Health Tracking

If you use a fitness tracker:

```
Create a skill to sync my [DEVICE] data to Cerebrum's health tracking.

Ask me:
1. What device do you use?
2. What metrics do you want to track?
3. Where should health data be logged?
```

---

## Troubleshooting

### Skills not loading

```bash
# Check skill format
cat ~/clawd/skills/task/SKILL.md | head -20
# Should have proper YAML frontmatter
```

### Browser not working

1. Make sure Chrome extension is installed
2. Click extension icon on the tab you want to control
3. Badge should show "ON"

### Environment variables not found

```bash
# Check if set
echo $CEREBRUM_PATH
echo $XAI_API_KEY

# If empty, add to ~/.zshrc and reload
source ~/.zshrc
```

### Python skills failing

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verify
uv --version
```

---

## Summary

After completing this setup, you'll have:

| Component | Location | Purpose |
|-----------|----------|---------|
| Core config | `~/clawd/*.md` | Personality, rules, user context |
| Memory | `~/clawd/memory/` | Daily logs, state |
| Skills | `~/clawd/skills/` | Task capture, X-growth, research tools |
| Knowledge | `$CEREBRUM_PATH/` | Notes, prompts, projects |

Your AI assistant is now personalized to you, knows your tone, and can help with tasks, content creation, and research.

---

## Next Steps

1. Use it daily — the more you interact, the better it gets
2. Update tone files — run `/learn` on posts you like
3. Review weekly — `/weekly-review` surfaces patterns
4. Evolve your setup — add skills as you discover needs

Welcome to your second brain. 🧠
