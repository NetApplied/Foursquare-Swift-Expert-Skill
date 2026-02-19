# Foursquare Swift Expert Skill

A modular agent skill for implementing Foursquare Places API integrations in Swift.

This repository contains a production-oriented skill at:

- `foursquare-swift-expert/`

The skill is designed to help agents generate consistent Swift code using:

- `async/await`
- `URLSession`
- `Codable`
- explicit `CodingKeys` mappings (including `fsq_id -> id`)
- standardized API error decoding

## Skill Contents

```text
foursquare-swift-expert/
├── SKILL.md
├── agents/
│   ├── openai.yaml
│   ├── claude.yaml
│   └── gemini.yaml
└── references/
    ├── core-data.md
    ├── search-and-discovery.md
    ├── place-content.md
    ├── crowdsourcing.md
    ├── suggest-and-status.md
    └── geotagging.md
```

## What Each Reference Covers

- `core-data.md`
  - Categories, Chains, response field mapping guidance
- `search-and-discovery.md`
  - Autocomplete, Place Search
- `place-content.md`
  - Place Details, Tips, Photos
- `crowdsourcing.md`
  - Suggest Merge, Suggest Edit, Suggest Remove, Flagging
- `suggest-and-status.md`
  - Suggest Place, Suggest Status
- `geotagging.md`
  - Geotagging candidates

Each reference includes:

- required headers
- path/query parameter documentation
- JSON response schema examples
- complete Swift endpoint functions
- reusable error handling baseline (`FoursquareErrorResponse`, `FoursquareAPIError`)

## Validation

If you have the Codex `skill-creator` tooling available, validate with:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  ./foursquare-swift-expert
```

If validation fails with `ModuleNotFoundError: No module named 'yaml'`, install PyYAML:

```bash
python3 -m pip install --user pyyaml
```

## Usage

1. Load the skill from `foursquare-swift-expert/SKILL.md`.
2. Route endpoint work to the matching file in `foursquare-swift-expert/references/`.
3. Reuse the provided Swift and error-handling patterns to keep integration consistent.

## Metadata Notes

- `agents/openai.yaml` is OpenAI/Codex UI metadata.
- `agents/claude.yaml` and `agents/gemini.yaml` are adapter manifests for cross-platform orchestration.
- `SKILL.md` + `references/*` are the canonical, portable skill instructions.
