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

