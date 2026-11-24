# AOItems

Anarchy Online item database in the export format served at
[items.aobots.org](https://items.aobots.org/). Nadybot's `ITEMS_MODULE`
downloads and imports these files; the bots check `*.json.version` once
a day and re-import when the version changes.

## Files

| File | Content |
|---|---|
| `rk{dimension}-items.json` | Full item export for that dimension: `version`, `families`, `templates` |
| `rk{dimension}-items.json.version` | Just the version string of the export, e.g. `18.08.58.03` |
| `rk{dimension}-not-in-game.json` | JSON array of AOIDs present in the export but not available on that dimension |
| `FORMAT.md` | Specification of the export format, as imported by Nadybot |

Currently, the real export data is maintained for RK5; the RK4 and
RK6 files are symlinks to the RK5 files, and RK7 uses its own
`rk7-not-in-game.json`, as it differs from the RK5 list. RK7 does not
have its own `rk{dimension}-items.json` yet.

Any change to the data **must** come with a changed version string in
`rk{dimension}-items.json.version`, or the bots won't notice the update
and keep the old data.

## Data format

The export is a JSON document with three sections:

- `version` — version of the server patch the data was taken from.
- `families` — groups of items that belong together (e.g. all QLs of a
  weapon). Each family holds an optional `name` and, under `items`,
  template pairs:
  - `[[aoid, ql]]` — a single template at exactly this QL
  - `[[lowid, lowql], [highid, highql]]` — a low/high pair covering the
    QL range from `lowql` to `highql`, inclusive
- `templates` — one entry per AOID with `name`, `ql` and optional
  `stats` (under their official stat IDs), `buffs`, `types` and
  `froob_friendly`.

See [FORMAT.md](FORMAT.md) for the complete specification.
