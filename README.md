# SalesPortl Agency Skills Suite Import Copy

This repository is a flattened, import-friendly copy of the canonical skill suite in [`danthamanvoiss/SalesPortl-Agency-Skills-Suite-v01`](https://github.com/danthamanvoiss/SalesPortl-Agency-Skills-Suite-v01).

- **Authoritative import index:** `manifest.json`
- **Expanded metadata catalog:** `catalog.json`
- **Skill files:** `skills/<category>/<skill-name>/SKILL.md`
- **Source snapshot:** `main` @ `12f7e3f9343e612125fb8595596f3b3b0360ec74`

## Purpose

Use this repository when a harness needs a simple, static skill inventory without traversing the full source repository structure.

This repo is **not** the authoritative source for skill development. It mirrors the current stable source content for import and discovery.

## Layout

```text
README.md
manifest.json
catalog.json
skills/
  <category>/
    <skill-name>/
      SKILL.md
```

## Module inventory

- `skills/ai-creative-skills/` — 4 skills
- `skills/client-success/` — 6 skills
- `skills/core/` — 26 skills
- `skills/graphic-design-creation-skills/` — 3 skills
- `skills/marketing/` — 8 skills
- `skills/partners/` — 2 skills
- `skills/print-design-skills/` — 3 skills
- `skills/sales/` — 9 skills
- `skills/seo-geo/` — 3 skills
- `skills/video-creation-skills/` — 4 skills
- `skills/website-creation-skills/` — 5 skills

## Import guidance

1. Read `manifest.json` to enumerate every available skill.
2. Load only the `SKILL.md` files needed for the current task.
3. Treat the markdown files as the source of skill behavior.
4. Refer back to the upstream repository for ongoing maintenance or future updates.

## Upstream

- Repository: https://github.com/danthamanvoiss/SalesPortl-Agency-Skills-Suite-v01
- Commit: `12f7e3f9343e612125fb8595596f3b3b0360ec74`
- Upstream license at snapshot: https://github.com/danthamanvoiss/SalesPortl-Agency-Skills-Suite-v01/blob/12f7e3f9343e612125fb8595596f3b3b0360ec74/LICENSE
- Mirrored license text in this import repo: `LICENSE`
