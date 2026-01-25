# Claude Skills for Scientific Notebooks

*Seven skills for AI-assisted oceanographic data development.*

## The Setup

The [C4IROcean-OceanDataPlatform](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform) repo has seven Claude skills in `.claude/skills/`. Each teaches Claude a specific domain.

## The Skills

### 1. stac-api-reference
How to search and access STAC catalogs. Includes common queries, authentication patterns, and asset loading.

### 2. odp-dataset-patterns
Ocean Data Platform specifics: available collections, data formats, coordinate systems.

### 3. hub-ocean-odp
Navigation for the ODP Hub: finding datasets, understanding metadata, data lineage.

### 4. dask-odp-patterns
Chunking strategies, lazy loading, distributed processing for large ocean datasets.

### 5. notebook-formatting
Cell structure, markdown conventions, visualization standards for reproducible notebooks.

### 6. odp-troubleshooting
Common errors and fixes: authentication, timeouts, memory issues, coordinate mismatches.

### 7. proposal-writing
Grant and project proposal templates, boilerplate for ocean data projects.

## Why It Works

Each skill is:
- **Focused** — One topic per file
- **Searchable** — Named after keywords Claude will encounter
- **Practical** — Code snippets, not theory

When you say "load SST data from ODP," Claude finds `hub-ocean-odp` and `odp-dataset-patterns`. When you hit a memory error, `dask-odp-patterns` and `odp-troubleshooting` activate.

## Building Your Own

1. Identify repeated explanations in your notebooks
2. Extract to `.claude/skills/topic-name.md`
3. Include code snippets that work
4. Let Claude write the skill from your examples

---

*Domain expertise, always available.*

[See the skills](https://github.com/aaronjohnson/C4IROcean-OceanDataPlatform/tree/main/.claude/skills)
