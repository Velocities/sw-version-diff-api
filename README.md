# sw-version-diff-api

**sw-version-diff-api** is a service that provides structured,
version-aware software change data for libraries and APIs.

## Problem

Modern software evolves quickly, but understanding *what actually
changed between versions* is often difficult:

-   Changelogs are inconsistent or incomplete\
-   Git diffs are too low-level and noisy\
-   Documentation can lag behind real changes\
-   AI tools often miss subtle but critical differences between versions

This leads to: - Breaking changes going unnoticed\
- Incorrect usage of updated APIs\
- Friction when upgrading dependencies

------------------------------------------------------------------------

## Solution

This project builds a pipeline that transforms raw version changes into
**structured, queryable data**.

Instead of relying on unstructured text, the system:

1.  Collects version data (Git tags, releases, changelogs)
2.  Analyzes code changes using deterministic methods (e.g., AST
    parsing)
3.  Uses an LLM to interpret and normalize changes
4.  Stores results as structured "change objects"
5.  Exposes them through a simple HTTP API

------------------------------------------------------------------------

## Example Use Case

Request:

    GET /changes?library=django&from=4.1&to=4.2

Response:

``` json
{
  "library": "django",
  "from_version": "4.1",
  "to_version": "4.2",
  "changes": [
    {
      "change_type": "deprecated",
      "component": "file storage",
      "description": "DEFAULT_FILE_STORAGE is deprecated",
      "migration": "Use STORAGES setting instead",
      "confidence": 0.92
    }
  ]
}
```

------------------------------------------------------------------------

## Key Design Principles

-   **Structured over unstructured**\
    All outputs conform to a defined schema (OpenAPI)

-   **Deterministic first, AI second**\
    Git diffs and AST analysis provide ground truth; LLMs interpret, not
    guess

-   **Precomputed results**\
    Changes are processed once and stored, making API queries fast and
    cheap

-   **Version-aware**\
    Every change is tied to explicit version transitions

------------------------------------------------------------------------

## Architecture Overview

-   **Ingestion Layer**
    -   Collects version data from repositories and changelogs
-   **Analysis Layer**
    -   Git diffing
    -   AST-based change detection
-   **LLM Processing**
    -   Interprets raw changes
    -   Produces structured outputs
-   **Storage**
    -   Stores normalized change records
-   **API Layer**
    -   Serves change data via HTTP endpoints

------------------------------------------------------------------------

## Goals

-   Make dependency upgrades safer and more predictable\
-   Provide reliable data for AI-assisted development tools\
-   Reduce ambiguity in software version transitions\
-   Serve as infrastructure for version-aware tooling

------------------------------------------------------------------------

## Project Status

Early development --- focused on building a minimal end-to-end pipeline
for a single ecosystem (starting with Python libraries).

------------------------------------------------------------------------

## Contributing

Contributions are welcome. Areas of interest include:

-   AST-based change detection
-   Changelog parsing
-   LLM prompt and validation design
-   API development
-   Supporting additional languages and ecosystems
