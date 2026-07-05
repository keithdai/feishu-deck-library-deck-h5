# feishu-deck-library-deck-h5

Standalone `deck-library` Skill package for managing reusable Feishu/Lark H5 deck page materials in Feishu Base.

This repository contains only the upper orchestration Skill. It does not vendor the `feishu-deck-h5` renderer.

## What It Does

- Archives rendered `feishu-deck-h5` deck outputs into Feishu Base.
- Stores one reusable Material row per page/slide.
- Uploads reusable deck artifacts such as `deck.json`, rendered HTML, `assets.zip`, cover thumbnails, and page thumbnails.
- Preserves each page's low-level `slide_payload_json` for reliable recomposition.
- Searches material metadata and returns stable material codes such as `M001`.
- Composes selected materials into a new `deck.json` and renders a final HTML deck through `feishu-deck-h5`.
- Tracks material quality and motion quality metadata, including `native_h5`, `replica_screenshot`, `has_motion`, and `motion_tier`.

## Peer Dependency

`deck-library` can be installed independently, but it expects `feishu-deck-h5` to be installed as a sibling Skill:

```text
skills/
  feishu-deck-h5/
  deck-library/
```

Without `feishu-deck-h5`, this Skill can still document, archive, and search Base metadata, but it cannot render or validate final H5 deliverables.

Required renderer tools from `feishu-deck-h5`:

- `skills/feishu-deck-h5/deck-json/render-deck.py`
- `skills/feishu-deck-h5/deck-json/deck-cli.py`
- `skills/feishu-deck-h5/assets/validate.py`

## Other Requirements

- Python 3
- `lark-cli`
- Feishu/Lark Base access
- `Decks` and `Materials` tables matching `skills/deck-library/references/base-schema.md`

## Configuration

Use explicit CLI flags or environment variables:

```bash
export DECK_LIBRARY_BASE_TOKEN="..."
export DECK_LIBRARY_DECKS_TABLE="..."
export DECK_LIBRARY_SLIDES_TABLE="..."
export DECK_LIBRARY_LARK_PROFILE="bytedance"
```

See `.env.example` for a copyable template.

## Install

Copy `skills/deck-library/` into the agent runtime's `skills/` directory next to `feishu-deck-h5`:

```bash
cp -R skills/deck-library /path/to/runtime/skills/
```

Then verify local dependencies:

```bash
python3 skills/deck-library/assets/preflight.py
```

## Usage

Archive a rendered deck output:

```bash
python3 skills/deck-library/assets/archive.py runs/example/output --write \
  --base-token "$DECK_LIBRARY_BASE_TOKEN" \
  --decks-table "$DECK_LIBRARY_DECKS_TABLE" \
  --materials-table "$DECK_LIBRARY_SLIDES_TABLE"
```

Search materials:

```bash
python3 skills/deck-library/assets/search.py "客户提案 AI 应用" \
  --base-token "$DECK_LIBRARY_BASE_TOKEN" \
  --materials-table "$DECK_LIBRARY_SLIDES_TABLE"
```

Compose selected materials:

```bash
python3 skills/deck-library/assets/compose_materials.py M001 M008 M012 \
  --title "客户反馈分析材料" \
  --write \
  --base-token "$DECK_LIBRARY_BASE_TOKEN" \
  --decks-table "$DECK_LIBRARY_DECKS_TABLE" \
  --materials-table "$DECK_LIBRARY_SLIDES_TABLE" \
  --output-dir runs/composed/output
```

## Tests

```bash
python3 -m unittest discover -s skills/deck-library/tests -p 'test_*.py'
```

## Do Not Commit

- Real Base tokens
- Private table IDs unless intentionally documenting a sample
- `runs/`
- `.deck-library-cache/`
- Customer materials
- Rendered deliverables containing private content

