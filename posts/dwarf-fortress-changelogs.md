# Dwarf Fortress-Style Changelogs

*Why narrative documentation is more fun and more useful.*

## The Standard Changelog

```markdown
## v1.2.0

### Added
- Firebase Crashlytics integration
- Danish, Norwegian, Swedish, Finnish translations

### Fixed
- Theme color reference error
```

Functional. Boring. Nobody reads it.

## The Dwarf Fortress Way

```markdown
## v0.6.0 - "The Watchtower Speaks Many Tongues"

### The Watchtower
- **Eyes on the Walls** (Crashlytics) — The fortress now reports when walls crumble

### The Translator's Guild
- **The Nordic Expansion** — The northern realms join the quest
  - Danish (da) — For the Danes who march through wind and rain
  - Finnish (fi) — For the sauna warriors

### Menaces Vanquished
- **The Sacred Colors Corrected** — The `xpGold` color did not exist in the theme scrolls
```

Same information. Actually more memorable.

## Why It Works

**1. Section headers tell stories**

| Boring | Narrative |
|--------|-----------|
| Added | New Chambers |
| Fixed | Menaces Vanquished |
| Changed | The Great Upheaval |
| Docs | The Scribes' Archives |

**2. Bugs become quests**

Instead of:
> Fixed: null pointer in step counter

Write:
> The step counter no longer loses count when confronted with the void

**3. Version names add memory hooks**

"The Watchtower Update" sticks better than "v0.6.0"

## The Format

```markdown
- **Thematic Name** (Technical Term) — Poetic description
```

- **Thematic Name** — leads with flavor
- **(Technical Term)** — searchable for greps and searches
- **Description** — what actually happened

## Is This Serious?

Yes. The Dwarf Fortress changelog has documented decades of development. Players read it for entertainment. Developers use it for reference. It works.

Your changelog can be:
- Accurate AND entertaining
- Searchable AND readable
- Documentation AND culture

## Getting Started

Pick one section header to rename:
- `### Fixed` → `### Menaces Vanquished`

See if it sparks joy. Expand from there.

---

*"Losing is fun!" — [Dwarf Fortress](https://www.bay12games.com/dwarves/)*
