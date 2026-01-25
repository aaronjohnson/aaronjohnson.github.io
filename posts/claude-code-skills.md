# Claude Code Skills: Teaching Your AI Your Codebase

*A pattern for persistent project context in AI-assisted development.*

## The Problem

Every new Claude conversation starts fresh. You explain your project structure. Again. Your naming conventions. Again. Your changelog style. Again.

## The Solution: Skills Files

Create a `.claude/skills/` directory in your project. Add markdown files describing your conventions. Claude Code reads them automatically.

```
.claude/
└── skills/
    └── changelog.md
```

**The key:** Name the file after the keyword. When you say "update the changelog," Claude finds `changelog.md` and applies your style.

**The trick:** Have Claude write the skill. Describe what you want once, ask Claude to save it as a skill, and every future conversation inherits it. Teach once, use forever.

**Project or global:** Skills in `.claude/skills/` apply to that project. Put them in `~/.claude/skills/` and they follow you everywhere.

**See it in action:** [changelog.md](https://github.com/aaronjohnson/step_quest/blob/shoes/.claude/skills/changelog.md) — the skill that powers Step Quest's Dwarf Fortress-style changelogs.

## A Real Example

My step counter app uses Dwarf Fortress-style changelogs. Instead of explaining this every time, I created `.claude/skills/changelog.md`:

```markdown
# Changelog Style Guide

Step Quest changelogs follow Dwarf Fortress patch notes style.

## Version Naming
Each version gets a thematic subtitle:
- v0.7.0 - "The Quest for a Proper Address"
- v0.1.0 - "The Great Awakening"

## Section Headers
Use thematic headers instead of generic ones:

| Topic | Header |
|-------|--------|
| Bug fixes | **Menaces Vanquished** |
| Features | **New Chambers** |
| Infrastructure | **The Mason's Work** |
| Docs | **The Scribes' Archives** |

## Example
- **The Grand Gates Open** (Google Play) — The fortress welcomes adventurers
```

Now when I say "update the changelog," Claude writes entries like:

> **The Key Returns to the Void** (API Key Deleted) — The exposed key is revoked

No prompting. It just knows.

## What to Put in Skills

Anything you'd repeat across conversations:

- **Code style** — naming conventions, preferred patterns
- **Project structure** — where things live, why
- **Domain language** — terms specific to your project
- **Documentation style** — changelog format, commit messages
- **Forbidden patterns** — things to avoid

## Keep Them Short

Skills should be reference docs, not novels. Aim for:
- One skill per topic
- Under 200 lines
- Tables over paragraphs
- Examples over explanations

## The Pattern Works

After a few days with skills files, I noticed:
- Less repetition in prompts
- More consistent outputs
- Faster context-switching between tasks
- A growing "knowledge base" for the project

The files also serve as documentation for human contributors.

## Beyond Changelogs

Other skills I'm considering:
- `testing.md` — test file conventions, what to mock
- `naming.md` — variable, function, file naming rules
- `architecture.md` — where code belongs, layer boundaries
- `i18n.md` — how to add new translations

---

*Teach once, use forever. Your AI remembers what you write down.*
