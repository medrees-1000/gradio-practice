# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

This repo is multi-context: it's a pnpm monorepo with separate Python and JS/Svelte packages that each have their own concerns (backend components/events, frontend rendering, and two thin client libraries).

## Before exploring, read these

- **`CONTEXT-MAP.md`** at the repo root: it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **`docs/adr/`** at the repo root: system-wide decisions that cut across contexts.
- **`<context>/docs/adr/`**: context-scoped decisions (e.g. `gradio/docs/adr/`, `js/docs/adr/`). Read the ones that touch the area you're about to work in.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## Contexts

| Context       | Path            | Concern                                                              |
| ------------- | --------------- | --------------------------------------------------------------------- |
| Backend       | `gradio/`       | Python source for the Gradio library: components, events, routing, CLI |
| Frontend      | `js/`           | Svelte/TypeScript frontend; each component lives in its own subdirectory |
| Python client | `client/python/`| The `gradio_client` Python client library                            |
| JS client     | `client/js/`    | The `@gradio/client` JavaScript client library                       |

## File structure

```
/
├── CONTEXT-MAP.md
├── docs/adr/                    ← system-wide decisions
├── gradio/
│   ├── CONTEXT.md
│   └── docs/adr/                ← backend-specific decisions
├── js/
│   ├── CONTEXT.md
│   └── docs/adr/                ← frontend-specific decisions
└── client/
    ├── python/
    │   ├── CONTEXT.md
    │   └── docs/adr/
    └── js/
        ├── CONTEXT.md
        └── docs/adr/
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in the relevant context's `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders), but worth reopening because…_
