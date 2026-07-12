# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A personal recipe collection kept as an [Obsidian](https://obsidian.md) vault (`.obsidian/` with Sync enabled), stored in Dropbox. It is a content repository — there is no build, test, lint, or run step. "Working in this repo" means authoring and editing recipe markdown (`.md`) files.

## Recipe markdown conventions

`Chili.md` is the reference for the current preferred format — match it when creating or editing recipes:

- **The filename is the title.** Title Case with spaces, e.g. `Braised Tarragon Chicken Thighs.md`. Do **not** add an H1/`#` title inside the file — Obsidian shows the filename as the title. Files start with a couple of blank lines.
- **Top-level sections are `##` headings**, in this order: `## Ingredients`, `## Instructions`, then optional `## Suggestions` and `## Inspirations`.
- **Group ingredients and steps by component.** Under a section, use a plain bold-style label line (e.g. `Beans`, `Chili Paste`, `Chili`) followed by its list, with a blank line between groups. `Ingredients` and `Instructions` use the *same* group labels so steps map to ingredients.
- **Ingredients** are a bulleted list (`-`), quantity first. Units are metric-forward (g, kg, cups, tbsp, tsp) and approximate amounts are normal — `20g`, `Garlic Cloves` (no amount), `to taste`.
- **Steps** are a numbered list (`1.`, `2.`, …). Use indented sub-items for detail: a sub-bullet (`- `) for a clarification or a spice sub-list, or a sub-number (`1.`) for a step-within-a-step.
- **Bold ingredient references** inside steps (`**Pasta**`, `**Sauce**`) are used in some recipes to tie steps back to the ingredient list. Keep this when a file already does it.
- **Oven temps are in °F**, written like `300°F`.
- **`## Suggestions`** holds trailing bold-labeled groups such as `Suggestions` (things to try next time) and `Notes` (what went wrong / lessons). When the user reports how a cook turned out, add it here rather than silently changing the recipe.
- **`## Inspirations`** is a bulleted list of source links (e.g. the YouTube video the recipe came from).

Older files are looser (bare labels instead of `##` headings, `*` bullets, mixed degree notation); leave them as-is unless asked, and follow the `Chili.md` style for anything new.

## Git workflow

- Per global instruction, **never stage files without an explicit request from the user.**
- History shows one commit per recipe. Commit subjects are short and name the recipe, often `Add <Recipe> recipe`. There is no branch protection; commits go to `main`.
