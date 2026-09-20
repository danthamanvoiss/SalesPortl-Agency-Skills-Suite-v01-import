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

| Module | Skills | Path |
|---|---:|---|
| ai-creative-skills | 4 | `skills/ai-creative-skills/` |
| client-success | 6 | `skills/client-success/` |
| core | 26 | `skills/core/` |
| graphic-design-creation-skills | 3 | `skills/graphic-design-creation-skills/` |
| marketing | 8 | `skills/marketing/` |
| partners | 2 | `skills/partners/` |
| print-design-skills | 3 | `skills/print-design-skills/` |
| sales | 9 | `skills/sales/` |
| seo-geo | 3 | `skills/seo-geo/` |
| video-creation-skills | 4 | `skills/video-creation-skills/` |
| website-creation-skills | 5 | `skills/website-creation-skills/` |

## Import guidance

1. Read `manifest.json` to enumerate every available skill.
2. Load only the `SKILL.md` files needed for the current task.
3. Treat the markdown files as the source of skill behavior.
4. Refer back to the upstream repository for ongoing maintenance or future updates.

## Upstream

- Repository: https://github.com/danthamanvoiss/SalesPortl-Agency-Skills-Suite-v01
- Commit: `12f7e3f9343e612125fb8595596f3b3b0360ec74`
- License: `LICENSE`
