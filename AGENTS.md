# AGENTS.md — canvus-custom-menu

**READ THIS FIRST** before reviewing or editing anything in this repo.

## What this repo is

A **configuration + asset** package for the MultiTaction **Canvus** custom finger-menu.
It is **not application code** — there is no build, no runtime logic here. Canvus (the
wall collaboration product) reads a `menu.yml` and, when a user taps an item, performs the
declared `create` actions on the canvas (drop a note, image, PDF, video, or browser tile).

`codegraph init` indexes 0 nodes / 0 edges — expected. There are no symbols, call graphs,
or functions. Reviewers should treat this as a **declarative-config + media** review, not a
source-code review.

## Repo map

| Path | Role |
|---|---|
| `menu.yml` (84 KB) | **The live/production menu.** The primary review target. |
| `other-menus/` | Alternate & work-in-progress menus (`menu_master`, `generic_menu`, `menu_just_tasks`, `menu_testing`). Not shipped as the active menu. |
| `examples/custom-menu-*/` | Per-client copies (Deloitte, GSK, KPMG, Vodafone), each a self-contained `menu.yml` + `content/` + `icons/`. |
| `content/` (165 MB) | Media dropped onto the canvas — PDFs, MP4s, PNGs. Referenced by `source:`. |
| `icons/` (11 MB) | Menu-button icons (incl. `task_icons/`, `converted_icons/`). Referenced by `icon:`. |
| `CUSTOM_MENU_MANUAL.md`, `README.md`, `tasks.md` | Authoring reference. README documents newline handling in note text. |

~202 MB of the repo is media assets; the reviewable surface is the YAML.

## The schema (what a reviewer is checking)

A menu is a nested tree of items:

```yaml
tooltip: <label>          # hover/press label
icon: <repo-relative path>  # button image
items:                    # submenu (mutually recursive)
  - tooltip: ...
    icon: ...
    actions:              # leaf: what tapping does
      - name: create      # the ONLY action verb in use across the whole repo
        parameters:
          type: note | image | pdf | video | browser
          ...
```

- **`create` is the only action verb** anywhere in the repo. There is **no `exec`, no
  `command`, no script hook, no shell-out**. The schema is purely declarative content
  placement — a menu item cannot run code. If a review is scoped to "embedded
  scripts / command hooks," the finding is: **none exist, by design.** The only external
  reach is `type: browser` opening a `url:` in an in-canvas web tile.
- Parameter families by `type`:
  - `note` — `color` (8-digit `#RRGGBBAA`, last byte = opacity), `text` (YAML block
    scalar `|`), `coordinate-system` (`viewport`/`canvas`), `location`, `size`, `scale`, `pinned`.
  - `image`/`pdf`/`video` — `source:` (repo-relative asset path), `scale`, placement.
  - `browser` — `url:`, `origin`, `size` (px), `scale`, `pinned`.

## Invariants & review focus

1. **Asset paths must resolve.** Every `source:` and `icon:` is **repo-relative** (e.g.
   `content/Foo.mp4`, `icons/task_icons/1-swot.png`). A typo = a blank/broken menu item at
   runtime with no error. Verifying these is the single highest-value config check.
   ⚠️ Many asset filenames contain **spaces** (e.g. `content/Toronto Raptors.mp4`) and are
   unquoted in `menu.yml`. When auditing paths, do NOT split on whitespace — a naive
   `grep source: | read` reports false "missing" files. Resolve the full value to EOL.
2. **YAML must parse.** Indentation and block-scalar (`|`) alignment are load-bearing.
   Known issue: `other-menus/menu_testing.yml` currently **fails to parse** (block-mapping
   error ~line 74/89). It is a WIP alternate menu, not the live `menu.yml`, but flag it.
3. **`menu.yml` is authoritative.** Changes intended to ship go there. `other-menus/` and
   `examples/` are copies/experiments — do not assume edits there take effect.
4. **Client examples may embed tenant-specific URLs** (Power BI / PowerApps / SAP / Vodafone
   dashboards with tenant IDs and report GUIDs in the query string). Treat these as
   client-confidential config; check nothing sensitive leaks into the generic `menu.yml`.
5. **Note colors** are `#RRGGBBAA` — the trailing pair is alpha (opacity), not RGB. A note
   that "looks invisible" is usually an intended low-alpha value, not a bug.

## Out of scope

No package manager, no CI, no tests, no compiled output. `.gitignore` lists generic
node/build patterns but nothing here builds. Do not add tooling the product does not consume.
