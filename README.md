# Foursquare Swift Expert Skill

Swift-focused Codex skill for implementing the Foursquare Places API with production-ready patterns.

## Overview

`foursquare-swift-expert` is a modular skill package that provides endpoint-level guidance and reusable Swift patterns for:

- `async/await` networking with `URLSession`
- strict `Codable` model design with explicit `CodingKeys`
- consistent request construction with `URLComponents`
- standardized API error handling (`error_detail` decoding for non-2xx responses)

The skill is designed as a practical implementation reference for engineers building or maintaining Foursquare Places API integrations in Swift.

## Repository Structure

```text
foursquare-swift-expert/
├── SKILL.md
├── agents/
│   ├── openai.yaml
│   ├── claude.yaml
│   └── gemini.yaml
└── references/
    ├── autocomplete.md
    ├── place-search.md
    ├── place-details.md
    ├── place-flag.md
    ├── geotagging-candidates.md
    ├── shared-models.md
    └── shared-error-handling.md
```

## Implemented Endpoints

- Autocomplete
- Place Search
- Place Details
- Flag Place
- Find Geotagging Candidates

## Shared Building Blocks

- `references/shared-models.md`
  - shared base models such as `Place`, `Category`, `Location`, `Chain`, `Icon`, `SocialMedia`, and related geo models.
- `references/shared-error-handling.md`
  - centralized `FoursquareAPIError`, `FoursquareErrorResponse`, and non-2xx handling helper.

## How To Use This Skill

1. Open `foursquare-swift-expert/SKILL.md`.
2. Identify the endpoint needed for your task.
3. Open the matching file in `foursquare-swift-expert/references/`.
4. Apply the documented path/query parameters, required headers (including `X-Places-Api-Version`), response-code/schema expectations, and Swift request/model/error-handling patterns.

### Where to Save Skills

Follow your tool’s official documentation, here are a few popular ones:
- **Codex:** [Where to save skills](https://developers.openai.com/codex/skills/#where-to-save-skills)
- **Claude:** [Using Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
- **Cursor:** [Enabling Skills](https://cursor.com/docs/context/skills#enabling-skills)
- **Xcode:** [Setting up coding intelligence](https://developer.apple.com/documentation/xcode/setting-up-coding-intelligence)


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
Expected output:

```text
Skill is valid!
```


## Metadata Notes

- `agents/openai.yaml` is OpenAI/Codex UI metadata.
- `agents/claude.yaml` and `agents/gemini.yaml` are provider-specific adapter manifests.
- `SKILL.md` + `references/*` are the canonical, portable skill instructions.


## Source Documentation

Endpoint content is based on the official Foursquare Places API documentation:

- [Autocomplete](https://docs.foursquare.com/fsq-developers-places/reference/autocomplete)
- [Place Search](https://docs.foursquare.com/fsq-developers-places/reference/place-search)
- [Place Details](https://docs.foursquare.com/fsq-developers-places/reference/place-details)
- [Place Flag](https://docs.foursquare.com/fsq-developers-places/reference/place-flag)
- [Geotagging Candidates](https://docs.foursquare.com/fsq-developers-places/reference/geotagging-candidates)


## Notes

- Reference files were generated from official Foursquare documentation sources listed in each file.
- API docs evolve over time; re-check versioned headers and schemas when upgrading API versions.
