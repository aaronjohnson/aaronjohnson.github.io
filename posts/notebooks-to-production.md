# From Notebooks to Production: Claude-Assisted Refactoring

*The messy middle between exploration and deployment.*

## The Pattern

Data science workflow:
1. **Explore** in notebooks — messy, iterative, lots of dead ends
2. **Prove** the concept works — "it runs on my laptop"
3. **??? ** — here be dragons
4. **Production** — clean, tested, deployable

Step 3 is where projects die. Claude helps.

## The Conversation

```
> This notebook works but it's a mess. Help me refactor.

Let me analyze the notebook structure...

I see:
- 47 cells
- 12 are markdown (good)
- 8 are exploratory dead ends (can remove)
- 3 have hardcoded paths
- 1 function is duplicated with slight variations

Want me to:
1. Extract the core logic into a module?
2. Clean up the notebook to call that module?
3. Add tests for the extracted functions?
```

## The Process

### 1. Identify the Core

```python
# Notebook has this scattered across 15 cells
def load_data(): ...
def clean_data(): ...
def transform(): ...
def analyze(): ...
def visualize(): ...
```

### 2. Extract to Module

```python
# src/pipeline.py
class DataPipeline:
    def __init__(self, config):
        self.config = config

    def run(self):
        data = self.load()
        data = self.clean(data)
        data = self.transform(data)
        return self.analyze(data)
```

### 3. Simplify Notebook

```python
# Now the notebook is 5 cells
from src.pipeline import DataPipeline

pipeline = DataPipeline(config)
results = pipeline.run()
visualize(results)
```

### 4. Add Tests

```python
def test_pipeline_handles_missing_data():
    config = {"allow_missing": True}
    pipeline = DataPipeline(config)
    # ...
```

## Why Claude Helps

- Sees the whole notebook at once
- Identifies duplication humans miss
- Generates boilerplate (tests, docstrings)
- Suggests refactoring patterns

The human decides what matters. Claude does the tedious extraction.

---

*Notebooks explore. Modules endure.*
