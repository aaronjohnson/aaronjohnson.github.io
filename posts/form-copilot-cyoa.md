# Choose Your Own Adventure: AI for Complex Forms

*Navigating multi-section applications with Claude.*

## The Problem

Complex forms are labyrinths:
- 50+ pages
- Conditional sections ("If yes, complete Section 7B")
- Cross-references ("See Schedule A, Line 12")
- Domain-specific terminology

Users get lost. AI gets confused by context length.

## The CYOA Approach

Instead of feeding the entire form to Claude, treat it like a Choose Your Own Adventure book:

1. **Start at the beginning** — What's your situation?
2. **Branch based on answers** — Skip irrelevant sections
3. **Focus on one section at a time** — Full context for current page
4. **Carry forward only what's needed** — Summary, not full history

```python
class FormCopilot:
    def __init__(self, config):
        self.sections = config["sections"]
        self.current = 0
        self.answers = {}

    def next_section(self):
        section = self.sections[self.current]
        if self.should_skip(section):
            self.current += 1
            return self.next_section()
        return section

    def should_skip(self, section):
        condition = section.get("skip_if")
        if condition:
            return eval(condition, self.answers)
        return False
```

## Config-Driven Navigation

```json
{
  "sections": [
    {
      "id": "business_type",
      "prompt": "What type of business are you?",
      "options": ["sole_prop", "llc", "corp"]
    },
    {
      "id": "corp_details",
      "skip_if": "business_type != 'corp'",
      "prompt": "Enter corporation details..."
    }
  ]
}
```

## Why It Works

- **Reduced context** — Claude sees one section, not 50 pages
- **Accurate branching** — Logic in config, not prompt engineering
- **Resumable** — Save progress, continue later
- **Auditable** — Clear path through the form

---

*Turn left to Section 7B. Turn right to skip to Schedule C.*

[Source: form-copilot](https://github.com/aaronjohnson/form-copilot)
