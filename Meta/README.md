# Cerebrum - Personal Knowledge Management System

A second brain system designed for managing two life domains: Personal and Work.

---

## Philosophy

Cerebrum is built around three principles:

### 1. Low Friction Capture
Getting information in should be effortless:
- Quick task capture to daily notes
- Save links with summaries
- Meeting notes flow naturally

### 2. Smart Organization
AI helps categorize and connect:
- Tasks routed to correct domain (Personal/Work)
- Resources tagged by relevance
- Patterns discovered over time

### 3. Proactive Surfacing
Relevant knowledge appears when needed:
- Weekly reviews synthesize insights
- Connections between notes discovered
- Tone prompts evolve through learning

---

## Vault Structure

```
cerebrum/
├── Daily/                # Daily notes (YYYY-MM-DD.md)
├── Projects/
│   ├── Work/             # Work documentation
│   └── Personal/         # Personal projects
├── Prompts/              # Writing tone system
│   ├── writing-tone.md
│   ├── reply-tone.md
│   ├── hooks.md
│   ├── closings.md
│   ├── phrases.md
│   ├── anti-patterns.md
│   └── examples/
├── Resources/            # Saved links and content
├── Templates/
│   └── Daily Note.md
└── Meta/
    ├── README.md
    └── Changelogs/
```

---

## Daily Note Format

```markdown
## Personal
<!-- Personal tasks -->

## Work
<!-- Work tasks -->

## Notes
<!-- Free-form thoughts -->
```

**Task format:** `- [ ] Task description 📅 YYYY-MM-DD`

---

## Tone System

The `Prompts/` folder contains a learnable writing voice system:

- **writing-tone.md** — For posts and original content
- **reply-tone.md** — For replies and comments
- **hooks.md** — Opening patterns
- **closings.md** — Ending patterns
- **phrases.md** — Structural patterns
- **anti-patterns.md** — What to avoid
- **ct-trends.md** — Current trends (ephemeral)

Changes tracked in `Changelogs/tone-evolution.md`.

---

## Style Guidelines

- Use hyphens (-) instead of em-dashes
- Prefer bullet points for tasks
- Keep daily note entries concise
- Tag appropriately: #personal, #work, etc.
- All files must be .md format

---

## Environment

Set `$CEREBRUM_PATH` for portable scripts:

```bash
export CEREBRUM_PATH="$HOME/cerebrum"
```

Skills reference `$CEREBRUM_PATH/Daily/`, `$CEREBRUM_PATH/Prompts/`, etc.
