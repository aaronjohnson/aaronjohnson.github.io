# Building a Claude Skills Library Across Projects

*One developer's growing collection of AI memory.*

## The Ecosystem

After several months with Claude Code, I have skills scattered across projects:

| Project | Skills | Purpose |
|---------|--------|---------|
| step_quest | 2 | Changelog style, WIF setup |
| C4IROcean | 7 | Ocean data, STAC, Dask, notebooks |
| form-copilot | 1 | Form navigation commands |

Each project teaches Claude something different. Together, they form a library.

## Patterns That Emerged

### 1. Name After Keywords

```
changelog.md     → triggers on "changelog"
wif.md           → triggers on "WIF", "keyless CI"
dask-odp.md      → triggers on "dask", "distributed"
```

### 2. Have Claude Write Them

Don't write skills from scratch. Do the work once with Claude, then:

> "Save what we just did as a Claude skill for next time."

Claude writes the skill. You review. Future sessions inherit it.

### 3. Project vs Global

```
~/.claude/skills/           # Follows you everywhere
project/.claude/skills/     # Project-specific
```

Global: coding style, git conventions, personal preferences
Project: domain knowledge, API patterns, team standards

### 4. Link Skills to Docs

```markdown
## Further Reading
See the [WIF skill](link) for copy-paste setup.
```

Blog posts explain why. Skills provide how.

## The Compound Effect

New project? Copy relevant skills:

```bash
cp -r ../other-project/.claude/skills/useful-skill.md .claude/skills/
```

Skills accumulate. Context grows. AI gets smarter per-project.

## What I'd Do Differently

- Start with skills earlier (not after repeating myself 10 times)
- Keep them shorter (under 100 lines)
- Add examples, not just explanations

---

*Your AI has memory. Use it.*

[Step Quest skills](https://github.com/aaronjohnson/step_quest/tree/shoes/.claude/skills) ·
[Ocean Data skills](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform/tree/main/.claude/skills)
