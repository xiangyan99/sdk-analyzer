# Skill Templates for Generated Customization Guides

## Complexity Threshold

**Customization files** are defined as:
- All non-empty `_patch.py` files (see analysis-dimensions.md for the definition of "empty")
- All `.py` files outside `_generated/`, excluding `__init__.py` and `_version.py`

**Threshold rule:**
- If customization files <= 3 AND total non-blank lines across all customization files <= 300 → **Single-file mode**
- Otherwise → **Multi-file mode**

---

## Output Location

Write all generated files to:

```
{package-path}/skills/sdk-customization-guide/
```

If the directory already exists, inform the user that existing files will be overwritten, then proceed.

---

## Single-File Mode

Generate one file: `SKILL.md`

````markdown
---
name: {package-name}-customization-guide
description: Customization patterns and conventions for {package-name}. Use when adding new customizations to this SDK package.
---

# {package-name} Customization Guide

## Overview

{1-3 sentences describing what this package does, what is customized, and the overall customization approach.}

## File Map

> **WARNING:** Files inside `_generated/` are auto-generated and MUST NOT be edited. They are listed here only as reference for understanding the base API.

### Generated Files (DO NOT EDIT)

| File | Purpose |
|------|---------|
| {path} | {purpose} |

### Customization Files (SAFE TO MODIFY)

| File | Purpose |
|------|---------|
| {path} | {purpose} |

### Dependencies

{List which customization files import from which other customization files or generated files.}

## Customization Patterns

### Client Layer

{If analysis found client customizations, describe the patterns with code examples.}

```python
# Example extracted from actual SDK code
{real code snippet from the package}
```

### Model Layer

{If analysis found model customizations, describe the patterns with code examples.}

```python
# Example extracted from actual SDK code
{real code snippet from the package}
```

### Operations Layer

{If analysis found operations customizations, describe the patterns with code examples.}

```python
# Example extracted from actual SDK code
{real code snippet from the package}
```

### Public API Surface

{Describe how __init__.py files re-export symbols from _patch.py and _generated/.}

### Utilities & Policies

{If analysis found utility modules or custom policies, describe them with code examples.}

```python
# Example extracted from actual SDK code
{real code snippet from the package}
```

## Async Parity Checklist

| Sync File | Async File | Status | Differences |
|-----------|------------|--------|-------------|
| {sync_path} | {async_path} | {identical / equivalent / divergent / sync-only / async-only} | {specific differences or "none"} |

## Rules

1. **Never edit files inside `_generated/`** — they will be overwritten on next code generation.
2. **Always maintain async parity** — every sync customization must have an async counterpart in the `aio/` tree.
3. {SDK-specific rule derived from analysis}
4. {SDK-specific rule derived from analysis}
````

**Section removal rules:**
- Remove Client Layer, Model Layer, Operations Layer, Public API Surface, or Utilities & Policies subsections if the analysis found nothing for that dimension.
- **NEVER omit:** Overview, File Map, Async Parity Checklist, Rules. These are always present even if minimal.

---

## Multi-File Mode

Generate the following files. Omit sub-files (not `SKILL.md`) for dimensions where the analysis found nothing.

### SKILL.md (always generated)

````markdown
---
name: {package-name}-customization-guide
description: Customization patterns and conventions for {package-name}. Use when adding new customizations to this SDK package.
---

# {package-name} Customization Guide

## Overview

{1-3 sentences describing what this package does, what is customized, and the overall customization approach.}

## File Map

> **WARNING:** Files inside `_generated/` are auto-generated and MUST NOT be edited. They are listed here only as reference for understanding the base API.

### Generated Files (DO NOT EDIT)

| File | Purpose |
|------|---------|
| {path} | {purpose} |

### Customization Files (SAFE TO MODIFY)

| File | Purpose |
|------|---------|
| {path} | {purpose} |

### Dependencies

{List which customization files import from which other customization files or generated files.}

## How to Use This Guide

This guide is split across multiple files by topic:

- **[client-patterns.md](./client-patterns.md)** — Client class customizations, constructors, methods, policies, credentials
- **[model-patterns.md](./model-patterns.md)** — Model class customizations, fields, serialization
- **[operations-patterns.md](./operations-patterns.md)** — Operations overrides, LRO/paging customizations
- **[api-surface.md](./api-surface.md)** — Public API exports and symbol management
- **[utilities.md](./utilities.md)** — Utility modules and custom pipeline policies
- **[async-parity.md](./async-parity.md)** — Sync/async file parity checklist

{Remove links for sub-files that were not generated.}

## Rules

1. **Never edit files inside `_generated/`** — they will be overwritten on next code generation.
2. **Always maintain async parity** — every sync customization must have an async counterpart in the `aio/` tree.
3. {SDK-specific rule derived from analysis}
4. {SDK-specific rule derived from analysis}
````

### client-patterns.md (if client customizations exist)

````markdown
# Client Layer Patterns

## Inheritance Pattern

{Describe how custom clients inherit from generated clients.}

```python
# From {filename}
{real code snippet showing class definition and base class}
```

## Constructor Customizations

{Describe added/modified __init__ parameters, default overrides, credential wrapping.}

```python
# From {filename}
{real code snippet showing __init__ customization}
```

## Method Overrides

{Describe methods that replace generated behavior.}

```python
# From {filename}
{real code snippet showing method override}
```

## New Methods

{Describe methods not present in generated client.}

```python
# From {filename}
{real code snippet showing new method}
```

## Pipeline Policies

{Describe which custom policies are injected into the client pipeline and at what position.}

```python
# From {filename}
{real code snippet showing policy injection}
```

## Credential Handling

{Describe custom credential types, token scope overrides, authentication flow changes.}

```python
# From {filename}
{real code snippet showing credential handling}
```
````

### model-patterns.md (if model customizations exist)

````markdown
# Model Layer Patterns

## Inheritance & Discriminators

{Describe custom model class hierarchy and polymorphic discriminator values.}

```python
# From {filename}
{real code snippet showing model inheritance}
```

## Field Additions

{Describe fields added beyond what the generated model provides.}

```python
# From {filename}
{real code snippet showing added fields}
```

## Serialization Customizations

{Describe custom _serialize/_deserialize methods, attribute map overrides.}

```python
# From {filename}
{real code snippet showing serialization customization}
```
````

### operations-patterns.md (only if operations customizations exist)

````markdown
# Operations Layer Patterns

## Overridden Operations

{Describe methods that override generated operations, noting what changed and why.}

```python
# From {filename}
{real code snippet showing operation override}
```

## New Operations

{Describe operations not present in the generated layer.}

```python
# From {filename}
{real code snippet showing new operation}
```

## LRO Customizations

{Describe custom polling, result extraction, or status check behavior.}

```python
# From {filename}
{real code snippet showing LRO customization}
```

## Paging Customizations

{Describe custom by_page behavior, continuation token handling, result mapping.}

```python
# From {filename}
{real code snippet showing paging customization}
```
````

### api-surface.md (if public API customizations exist)

````markdown
# Public API Surface

## Export Rules

{Describe how __init__.py files manage the public API.}

## Re-exports from Patch Files

{List symbols imported from _patch.py and exposed publicly.}

```python
# From {filename}
{real code snippet showing re-exports}
```

## Symbol Renaming

{List cases where internal names are re-exported under different public names.}

## Additional Exports

{List symbols in __init__.py that do not come from _generated/.}

## Removed Exports

{List symbols present in _generated/__init__.py but absent from the public __init__.py.}
````

### utilities.md (if utility modules or custom policies exist)

````markdown
# Utilities & Policies

## Module Inventory

| Module | Purpose | Used By |
|--------|---------|---------|
| {filename} | {one-line purpose} | {list of files that import from it} |

## Custom Policies

{Describe classes inheriting from *Policy or *SansIOHTTPPolicy.}

```python
# From {filename}
{real code snippet showing policy implementation}
```

## Utility Functions

{Describe public helper functions with signatures and purpose.}

```python
# From {filename}
{real code snippet showing utility function}
```

## Constants & Enums

{Describe module-level constants, string enums, configuration values.}
````

### async-parity.md (always generated)

````markdown
# Async Parity

## Parity Checklist

| Sync File | Async File | Status | Differences |
|-----------|------------|--------|-------------|
| {sync_path} | {async_path} | {identical / equivalent / divergent / sync-only / async-only} | {specific differences or "none"} |

## How to Create Async Counterparts

When adding a new sync customization, create the async counterpart by:

1. Copy the sync file to the corresponding `aio/` location.
2. Add `async` keyword to all I/O-bound method definitions.
3. Add `await` to all I/O-bound calls within those methods.
4. Update imports to use async versions of clients and operations from `_generated.aio`.
5. Verify the `__all__` list in `aio/__init__.py` includes the new async symbols.

```python
# Sync pattern (from {sync_filename})
{real sync code snippet}

# Async equivalent (from {async_filename})
{real async code snippet}
```
````

---

## Quality Requirements

Every generated skill file MUST satisfy all of the following:

1. **Real code examples only** — All code blocks must contain snippets extracted from the actual SDK package being analyzed. Never invent or fabricate example code.
2. **Package-specific content** — All descriptions, rules, and guidance must be specific to the SDK package under analysis. Do not include generic Azure SDK advice.
3. **`_generated/` warning present** — The File Map section must include the warning about never editing generated files.
4. **Async parity always included** — The async parity checklist must be present even if no async customizations exist yet. In that case, note that async counterparts should be created for any future sync customizations.
5. **Empty dimensions omitted cleanly** — In single-file mode, remove subsections for dimensions with no findings. In multi-file mode, do not generate sub-files for empty dimensions. **Never omit:** Overview, File Map, Async Parity Checklist, Rules.
