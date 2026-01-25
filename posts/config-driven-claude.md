# Config-Driven Claude: Schema-Based Prompting

*When prompts become data structures.*

## The Problem

Complex AI workflows hardcode logic in prompts:

```python
prompt = """You are a form assistant. If the user is a corporation,
ask about shareholders. If LLC, ask about members. If sole prop,
skip to Schedule C. Also remember to..."""
```

This doesn't scale. It's hard to test. It mixes logic with language.

## The Pattern

Separate what Claude says from what Claude does:

```json
{
  "schema_version": "1.0",
  "steps": [
    {
      "id": "entity_type",
      "type": "choice",
      "prompt": "What type of entity?",
      "options": ["corporation", "llc", "sole_prop"],
      "next": {
        "corporation": "shareholders",
        "llc": "members",
        "sole_prop": "schedule_c"
      }
    }
  ]
}
```

## Benefits

**1. Testable**
```python
def test_corp_path():
    copilot = FormCopilot(config)
    copilot.answer("entity_type", "corporation")
    assert copilot.current_step == "shareholders"
```

**2. Versionable**
```
configs/
  v1.0.json
  v1.1.json  # Added new question
  v2.0.json  # Breaking change
```

**3. Swappable**
Same engine, different forms:
```python
tax_form = FormCopilot("configs/tax.json")
permit_app = FormCopilot("configs/permit.json")
```

**4. Auditable**
```python
copilot.get_path()
# ["entity_type:corporation", "shareholders:3", "board:yes", ...]
```

## Schema Validation

```python
from jsonschema import validate

with open("config_schema.json") as f:
    schema = json.load(f)

validate(instance=my_config, schema=schema)
```

Catch errors before runtime. No more "prompt didn't handle this case."

---

*Logic in JSON. Language in prompts. Both versioned.*

[Source: form-copilot](https://github.com/aaronjohnson/form-copilot)
