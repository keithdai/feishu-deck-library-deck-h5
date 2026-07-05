# Install

Install `deck-library` as a sibling Skill next to `feishu-deck-h5`:

```text
skills/
  feishu-deck-h5/
  deck-library/
```

`deck-library` is not a renderer. It composes DeckJSON and calls the renderer from `feishu-deck-h5`.

## Required Runtime

- Python 3
- `lark-cli`
- Feishu/Lark Base access
- Installed sibling Skill: `feishu-deck-h5`
- Base schema matching `skills/deck-library/references/base-schema.md`

## Verify

```bash
python3 skills/deck-library/assets/preflight.py
python3 -m unittest discover -s skills/deck-library/tests -p 'test_*.py'
```

