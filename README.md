# line-sticker-vault

Canonical archive for LINE sticker **character reference sheets** and **phrase-set JSON** produced by [Sprite-Animator](https://github.com/poirotw66/Sprite-Animator).

Pipeline outputs (`stickers/`, sheet PNGs, ZIPs) stay in local `output/` — only reusable inputs live here.

## Layout

```text
line-sticker-vault/
├── registry/sticker-registry.json   # same schema as Sprite-Animator
├── characters/{slug}/
│   ├── meta.json
│   └── character-ref.webp
└── sets/{set-id}/
    ├── meta.json
    └── phrase-set.json
```

- **set-id**: `SET-YYYYMMDD-NNN` (e.g. `SET-20260712-001`)
- **slug**: `[a-z0-9]+(-[a-z0-9]+)*` (e.g. `street-graffiti-fox`)

## Sync from Sprite-Animator

```bash
# preview
npx tsx .claude/skills/line-sticker-maker/scripts/archive-sync.mts --dry-run

# write into sibling ../line-sticker-vault (default)
npx tsx .claude/skills/line-sticker-maker/scripts/archive-sync.mts --sync

# custom vault path
npx tsx .claude/skills/line-sticker-maker/scripts/archive-sync.mts --sync --vault /path/to/line-sticker-vault
```

Environment override: `STICKER_VAULT_ROOT=/path/to/line-sticker-vault`

## Daily factory (A-slot rotation)

`daily-pack.mts` auto-detects this repo as a sibling folder and merges `registry/sticker-registry.json` with local `output/sticker-registry.json` when planning A slots (reuse archived B characters).

```bash
npx tsx .claude/skills/line-sticker-maker/scripts/daily-pack.mts --backfill --plan-only
```

Disable vault: `--no-vault`

## Git LFS

Character reference images are stored as **WebP** (`character-ref.webp`) and tracked with Git LFS (see `.gitattributes`). After clone:

```bash
git lfs install
git lfs pull
```

## Private repo

Keep this repository **private** — it contains your character art and phrase copy.
