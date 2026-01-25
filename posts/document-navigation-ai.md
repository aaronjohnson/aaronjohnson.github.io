# Multi-Section Document Navigation with AI

*Helping users find their place in complex paperwork.*

## The Problem

Government and legal forms are designed for completeness, not usability:
- 100+ page applications
- Sections reference other sections
- Conditional logic buried in fine print
- Users don't know what they don't know

## The Approach

Three layers of navigation:

### 1. Section Map

Build a navigable index:

```python
sections = {
    "1": {"title": "Applicant Info", "pages": "1-5"},
    "2": {"title": "Business Details", "pages": "6-12"},
    "2a": {"title": "Corporate Structure", "requires": "2.type == 'corp'"},
    "2b": {"title": "LLC Operating Agreement", "requires": "2.type == 'llc'"},
}
```

### 2. Relevance Filter

Not everyone needs every section:

```python
def relevant_sections(answers):
    return [
        s for s in sections.values()
        if evaluate_condition(s.get("requires", "True"), answers)
    ]

# User is sole prop? Skip 2a, 2b entirely.
```

### 3. Context Injection

When Claude helps with a section, inject only what's needed:

```python
def build_context(section_id, answers):
    section = sections[section_id]
    referenced = section.get("references", [])

    return {
        "current": section,
        "prior_answers": {k: answers[k] for k in referenced},
        "instructions": section["help_text"]
    }
```

## The User Experience

```
> Where am I?
You're on Section 3 (Financial Information).
Based on your answers, you can skip Sections 4-6.
Next: Section 7 (Certifications).

> What do I need for Section 7?
You'll need:
- EIN (you entered: 12-3456789)
- State registration number (not yet provided)
- Signature of authorized officer
```

---

*GPS for paperwork.*

[Source: form-copilot](https://github.com/aaronjohnson/form-copilot)
