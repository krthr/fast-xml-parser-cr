# Domain Docs

How the engineering skills consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- `GLOSSARY.md` at the repo root.
- ADRs in `docs/adr/` that touch the area you're about to work in.

If these files don't exist, proceed silently. The `/domain-modeling` skill, also reached via `/grill-with-docs` and `/improve-codebase-architecture`, creates them lazily when terms or decisions get resolved.

## File structure

This repo uses a single-context layout:

```text
/
├── GLOSSARY.md
├── docs/
│   ├── agents/
│   └── adr/
│       └── 0001-<decision-slug>.md
└── src/
```

Keep domain documentation at this repo's root. `fast-xml-parser-js/` is an upstream reference submodule, not a separate domain context for this configuration.

## Use the glossary's vocabulary

When naming a domain concept in an issue title, refactor proposal, hypothesis, or test name, use the term defined in `GLOSSARY.md`.

If a needed concept isn't in the glossary, reconsider whether it belongs to the project's vocabulary or note the gap for `/domain-modeling`.

## Flag ADR conflicts

If your output contradicts an existing ADR, identify the ADR and explain why the decision should be reopened.
