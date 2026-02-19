# Foursquare Swift Expert Skill

A modular Codex skill for implementing the [Foursquare Places API](https://docs.foursquare.com/fsq-developers-places/reference) in Swift.

This repository contains a production-oriented skill package focused on:
- Swift `async/await` + `URLSession` request patterns
- `Codable`, `Identifiable`, `Hashable` models with explicit `CodingKeys`
- Endpoint-level parameter/header documentation
- Response code coverage and JSON schema references
- Standardized error handling via `FoursquareAPIError` and `FoursquareErrorResponse` (`error_detail` parsing)

## What Is Included

- `foursquare-swift-expert/SKILL.md`: Main skill manifest and workflow instructions
- `foursquare-swift-expert/agents/*.yaml`: Skill UI metadata
- `foursquare-swift-expert/references/*.md`: Endpoint-specific implementation references and Swift examples

## Repository Structure

```text
codex-project/
├── foursquare-swift-expert/
│   ├── SKILL.md
│   ├── agents/
│   │   ├── claude.yaml
│   │   ├── gemini.yaml
│   │   └── openai.yaml
│   └── references/
│       ├── common-data.md
│       ├── autocomplete.md
│       ├── place-search.md
│       ├── place-details.md
│       ├── place-tips.md
│       ├── place-photos.md
│       ├── place-flag.md
│       ├── suggest-merge-places.md
│       ├── suggest-edit-place.md
│       ├── suggest-remove-place.md
│       ├── suggest-new-place.md
│       ├── suggest-status.md
│       ├── suggest-review.md
│       ├── geotagging-candidates.md
│       └── geotagging-confirm.md
└── foursquare-skill-creation-prompt.md
```

## Endpoints Covered

- Autocomplete
- Place Search
- Place Details
- Place Tips
- Place Photos
- Flag a Place
- Suggest Merge Places
- Suggest Edit Place
- Suggest Remove Place
- Suggest New Place
- Suggest Status
- Suggest Review
- Geotagging Candidates
- Geotagging Confirm
- Common data/reference fields (categories, chains, response fields)

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


### Where to Save Skills

Follow your tool’s official documentation, here are a few popular ones:
- **Codex:** [Where to save skills](https://developers.openai.com/codex/skills/#where-to-save-skills)
- **Claude:** [Using Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#using-skills)
- **Cursor:** [Enabling Skills](https://cursor.com/docs/context/skills#enabling-skills)
- **Xcode:** [Setting up coding intelligence](https://developer.apple.com/documentation/xcode/setting-up-coding-intelligence)


## Metadata Notes

- `agents/openai.yaml` is OpenAI/Codex UI metadata.
- `agents/claude.yaml` and `agents/gemini.yaml` are adapter manifests for cross-platform orchestration.
- `SKILL.md` + `references/*` are the canonical, portable skill instructions.


## Notes

- Reference files were generated from official Foursquare documentation sources listed in each file.
- API docs evolve over time; re-check versioned headers and schemas when upgrading API versions.
