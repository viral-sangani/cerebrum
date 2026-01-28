# Cerebrum

A personal knowledge management system designed for AI-assisted workflows. Built for Obsidian + Clawdbot.

## What is this?

Cerebrum is your "second brain" — a structured Obsidian vault that works seamlessly with AI assistants. It combines:

- **Daily notes** with task management
- **Project documentation** for work and personal life
- **Tone prompts** for consistent writing voice (especially X/Twitter)
- **Resource storage** for saved links and content

## Quick Start

1. **Clone this repo** to your preferred location:
   ```bash
   git clone https://github.com/yourusername/cerebrum-template.git ~/cerebrum
   ```

2. **Open in Obsidian**:
   - Open Obsidian
   - "Open folder as vault"
   - Select the cloned directory

3. **Set up Clawdbot** (optional but recommended):
   - See `clawd-setup.md` for instructions on setting up your AI workspace
   - Set `$CEREBRUM_PATH` environment variable:
     ```bash
     export CEREBRUM_PATH="$HOME/cerebrum"
     ```

4. **Customize**:
   - Edit `Projects/Work/Overview.md` with your work context
   - Edit `Projects/Personal/Overview.md` with your personal projects
   - Modify `Templates/Daily Note.md` to match your workflow

## Directory Structure

```
cerebrum/
├── Daily/                 # Daily notes (YYYY-MM-DD.md)
├── Projects/
│   ├── Work/              # Work project documentation
│   └── Personal/          # Personal project documentation
├── Prompts/               # Writing tone system
│   ├── writing-tone.md    # Voice for original content
│   ├── reply-tone.md      # Voice for replies
│   ├── hooks.md           # Opening patterns
│   ├── closings.md        # Ending patterns
│   ├── phrases.md         # Structural patterns
│   ├── anti-patterns.md   # What to avoid
│   ├── ct-trends.md       # Current trends (ephemeral)
│   └── examples/          # Learned post examples
├── Resources/             # Saved links, articles, content
├── Templates/             # Note templates
│   └── Daily Note.md
└── Meta/
    ├── README.md          # This documentation
    └── Changelogs/
        └── tone-evolution.md
```

## Daily Notes

Daily notes use a two-section structure:

```markdown
## Personal
<!-- Personal tasks, errands, family -->

## Work
<!-- Work tasks and projects -->
```

Tasks use Obsidian Tasks format: `- [ ] Task description 📅 YYYY-MM-DD`

## Tone System (X/Twitter Growth)

The `Prompts/` folder contains a learnable tone system:

1. **Learn from posts you like** — Analyze successful posts and extract patterns
2. **Generate drafts** — Create posts matching your evolved voice
3. **Generate replies** — Craft replies that match thread energy
4. **Consolidate** — Periodic cleanup of tone files

Files evolve over time. Changes are logged in `Meta/Changelogs/tone-evolution.md`.

## Recommended Obsidian Plugins

- **Obsidian Tasks** — Task management with dates and filters
- **Templater** — Advanced templates for daily notes
- **Calendar** — Visual calendar for daily notes

## Environment Variables

If using with Clawdbot or other AI tools:

```bash
# Add to ~/.zshrc or ~/.bashrc
export CEREBRUM_PATH="$HOME/cerebrum"
```

Skills and scripts can reference `$CEREBRUM_PATH` for portability.

## Customization

This is a starting point. Adapt it to your workflow:

- **Different domains?** Rename Work/Personal sections in daily notes
- **Different tone?** Update `Prompts/` files with your voice
- **More projects?** Add folders under `Projects/`
- **Health tracking?** Add a `Personal/Health/` folder

## License

MIT — do whatever you want with it.
