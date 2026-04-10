# sdk-analyzer

A [GitHub Copilot skill](https://code.visualstudio.com/docs/copilot/copilot-customization) that analyzes an Azure Python SDK package and generates a customization guide skill for it.

## What It Does

Given an Azure SDK package name (e.g., `azure-ai-projects`), the skill:

1. **Locates** the package under the `azure-sdk-for-python` repo's `sdk/` directory
2. **Scans and classifies** all `.py` files as generated, patch, init, or utility
3. **Analyzes 7 dimensions** of customization patterns:
   - **Client Layer** — custom client classes, constructor changes, method overrides, pipeline policies
   - **Model Layer** — custom models, added fields, serialization overrides
   - **Operations Layer** — operation overrides, LRO/paging customizations
   - **Public API Surface** — `__init__.py` re-exports, symbol renaming, added/removed exports
   - **Utilities & Policies** — helper modules, custom pipeline policies, constants
   - **Async Parity** — sync/async consistency checks
   - **Import Dependencies & Regeneration Risks** — import maps, ApiVersion enums, monkey-patches, wire format helpers, named patterns
4. **Generates** a customization guide skill (single-file or multi-file based on complexity) at `{package-path}/.github/skills/{package-name}/`

## Prerequisites

- The current working directory must be the root of the [`azure-sdk-for-python`](https://github.com/Azure/azure-sdk-for-python) repository.
- The target SDK package must exist under the `sdk/` directory.

## Usage

Invoke the skill in GitHub Copilot by asking it to analyze a package:

> Analyze `azure-ai-projects` and generate a customization guide.

## Output Modes

| Mode | Condition | Output |
|------|-----------|--------|
| **Single-file** | ≤ 3 customization files AND ≤ 300 non-blank lines | One `SKILL.md` with all patterns inline |
| **Multi-file** | Above threshold | `SKILL.md` + topic-specific files (`client-patterns.md`, `model-patterns.md`, etc.) |

## Skill Structure

```
sdk-analyzer/
  SKILL.md                 # Main skill definition and workflow
  analysis-dimensions.md   # The 7 analysis dimensions with scan targets and record formats
  skill-templates.md       # Output templates for single-file and multi-file modes
```
