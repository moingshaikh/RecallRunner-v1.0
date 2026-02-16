## Deterministic browser execution service wrapped in a simple API.

Deterministic browser execution service wrapped in a simple API.

RecallRunner v1.0 demonstrates how structured data can be extracted from a public web interface using browser automation and exposed through a stable API contract.

This project showcases browser automation mechanics, API design, artifact generation, and execution determinism.

## What It Does

RecallRunner navigates the Government of Canada recall portal and:

- Captures the latest recall entries
- Extracts structured recall URLs
- Deep-dives into the first item for detail enrichment
- Generates execution artifacts (JSON + screenshot)
- Exposes results via HTTP endpoints

All results are persisted to disk for traceability.

## Why This Exists
Many public systems do not provide stable APIs.

Browser automation becomes a deterministic integration layer when:

- No official API exists
- DOM structures are semi-stable
- Structured extraction is required
- Execution needs to be reproducible
- RecallRunner models how an external agent or system could reliably interact with such a site.

## API Endpoints (v1.0)
## GET /health
Basic service health check.

## POST /run/recalls

Triggers a recall snapshot run.

Returns structured JSON including:

- metadata
- ranked recall URLs
- enriched detail for first item
- execution status
- artifact paths

## GET /latest

Returns the most recent execution JSON.

## GET /latest.png

Returns the latest full-page screenshot artifact.

## Example Response (Trimmed)

```{
  "meta": {
    "schema_version": "1.0",
    "run_id": "2026-02-16T01:58:05.943138",
    "source": "recall_canada",
    "max_items": 5
  },
  "list": [
    {
      "rank": 1,
      "detail_url": "https://recalls-rappels.canada.ca/en/alert-recall/..."
    }
  ],
  "detail": {
    "title": "...",
    "summary": "...",
    "hazard": "..."
  },
  "status": {
    "ok": true
  }
}
```

## Artifacts
After execution:
```outputs/
  ├── latest.json
  └── latest.png
```
Artifacts provide:

- Observability
- Replay/debug capability
- Verifiable evidence of execution

## Architecture Overview

Execution flow:

1. FastAPI endpoint triggered
2. Playwright launches headless Chromium
3. Target page loaded
4. Recall URLs collected
5. First item deep-dived
6. JSON + screenshot persisted
7. API returns structured result
 
Design principle:
Execution is deterministic. Planning is external.

This mirrors how an AI agent would delegate web interaction to a tool.

## Design Decisions (v1.0)

- Deep-dive only first item for speed and determinism
- Persist artifacts even on failure
- Avoid brittle DOM selectors
- Filter URLs using structural pattern matching
- Headless execution for portability
