---
name: recipe-review
description: Reviews staged recipe markdown files for grammar, house style, ingredient/cookware cross-references, units, and markdown formatting, then applies the fixes to the working tree without staging or committing. Use when asked to review, proofread, or clean up staged recipes.
argument-hint: "[recipe.md ...]"
allowed-tools: Read Edit Write Bash(git status *) Bash(git diff *)
---

# Recipe review

Review every staged recipe and fix it in the working tree. Leave the index alone, so `git diff` shows exactly your fixes and `git diff --cached` still shows the author's version.

## Hard rules

- Edit working-tree files only. Never run a command that changes the index or history: no `git add`, `commit`, `stash`, `restore`, `reset`, or `checkout`.
- Never invent or change a quantity, temperature, or time. You may reformat one (`500g` → `500 g`) but never alter its value.
- Notes and Suggestions are the author's voice. Fix spelling and grammar there; don't reword.
- Baseline conventions are in [CLAUDE.md](../../../CLAUDE.md) at the repo root. The rules below override it wherever they conflict. For example, quantity-first ingredient lines like those in `Chili.md` are outdated.

## Staged changes

Status:
!`git status --short`

Staged diff:
!`git diff --cached`

## Workflow

1. **Pick targets.** These are staged `.md` files with status `A`, `M`, or `R` (use the new path), excluding `CLAUDE.md`, `.claude/`, and `.obsidian/`. If `$ARGUMENTS` names files, review only those. If nothing qualifies, say so and stop.
2. **Guard.** Skip any target that also has unstaged changes (a second-column `M` in the status above), and say why: your fixes would mix with the author's unstaged edits. Ask the author to stage or stash first.
3. **Review.** Read each target in full and run passes 1–8 below, in order, on the whole file, even if only one line was staged. Use Edit for scattered fixes, or Write when the file needs a broad rewrite.
4. **Self-check.** Run `git diff -- "<file>"`. Confirm that every original ingredient, step, note, and link is still there, that no amount changed value, and that each pass's rules hold. Fix anything you missed.
5. **Report** in the format at the end, then stop.

## Passes

### 1. Structure

- The sections, in order, are `## Ingredients`, `## Cookware`, `## Instructions`, `## Suggestions`, and `## Inspirations`. Map old bare labels onto them: Steps or Directions → Instructions, Utensils → Cookware, and Next Time or Optional extras → a group under Suggestions.
- Component groups are `###` subheadings (`### Chili Paste`), not bare label lines. This includes `### Suggestions` and `### Notes` under `## Suggestions`.
- Group ingredients only when components have distinct ingredients; otherwise a flat list is fine. When Ingredients are grouped, Instructions use the same labels.
- `### Mise en Place` is an optional first group under Instructions. It holds prep that can happen in any order, so it uses `-` bullets instead of numbers.
- Keep existing empty placeholder sections, but don't add new ones.

### 2. Ingredient lines

Format: `- **<Name> <Modifiers>** (Optional) - <Quantity>`

- The name comes first: it's the core noun a reader scans for. Descriptors (color, size, variety, form, prep state) follow it, with no commas. Noun compounds stay together (Chicken Broth, Soy Sauce, Cocoa Powder, Garam Masala).
- Bold the name and modifiers. `(Optional)` and the quantity stay plain.
- If no amount is known, drop the ` - <Quantity>` part. Keep hedges and ranges as written (`(ish)`, `450-500 g`, `750 ml or 3 cups`).
- Order each group from most central to most likely already at home. The star of the dish goes first (the **Beef** in a beef stew), and pantry staples like **Water**, **Olive Oil**, and **Salt** go last. This is a loose ordering: move lines that are clearly out of place, not ones that only differ slightly.

| Before | After |
|---|---|
| `- 3 Yellow Onions` | `- **Onions Yellow** - x3` |
| `- 3 tbsp Salted Butter` | `- **Butter Salted** - 3 tbsp` |
| `- 2 Minced Shallots` | `- **Shallots Minced** - x2` |
| `- 20g Dried Ancho Chili` | `- **Chili Ancho Dried** - 20 g` |
| `- 2 cups Chicken Broth` | `- **Chicken Broth** - 2 cups` |
| `- Salt (to taste)` | `- **Salt** - to taste` |
| `- Mushrooms (Optional)` | `- **Mushrooms** (Optional)` |

### 3. Bolding

- In the Cookware list, bold each whole item (`- **Rondeau**`).
- Everywhere else (Instructions, including Mise en Place and sub-lists, and Suggestions), bold every mention of a listed ingredient or cookware item in Title Case. A short form is fine (**Onions** for Onions Yellow). Keep articles outside the bold: `the **Beef**`.
- Don't bold appliances (oven, stove), unlisted intermediate products (roux, stew), or loose groupings (veggies).

### 4. Cross-reference

- Every ingredient and cookware item used in a step must be listed. Add any missing ones to the right group, with a quantity only if a step states one ("Add 3 tbsp of salt" → `- **Salt** - 3 tbsp`). Water counts only when it stays in the dish, not when it's drained. Cooking oil counts.
- Every listed ingredient must be used in some step. If a step clearly implies it (a roux uses the **Butter** and **Flour**), name it there; otherwise flag it.
- Step references use the same names as the lists.
- Flag, don't fix, any quantity in a step that conflicts with the list.

### 5. Units and numbers

- Put a space between number and unit: `500 g`, `1.5 kg`, `1 tsp`, `750 ml`.
- Use lowercase standard abbreviations (g, kg, ml, l, tsp, tbsp, cups, oz, lb) and fix shorthand: `6c` → `6 cups`, `1 TSP` → `1 tsp`, `1 kilo` → `1 kg`.
- Write counts in the quantity slot as `x3`.
- Write temperatures as `300°F` with the degree sign, not `300f`, `300 F`, or `300ºF` (the ordinal º). A bare oven number (`in the oven at 350`) becomes `350°F`.
- Keep time units consistent within a file; don't mix `mins` and `minutes`.
- Never convert between units.

### 6. Grammar and spelling

- Fix typos, spelling, grammar, and punctuation.
- Steps are imperative sentences ending in a period. Single-word sub-bullets (spice lists) don't need one.
- Spell cooking terms correctly, accents included: sauté and sautéed (not "sautée"), chickpeas.
- Fix, don't rewrite. Keep the author's wording and emphasis ("DON'T"), and leave double spaces between sentences.

### 7. Markdown lint

- Start with exactly two blank lines, and no H1.
- Use `-` bullets, never `*`. Numbered steps count up (`1.`, `2.`, `3.`) instead of repeating `1.`.
- Indent sub-items with one tab.
- Put one blank line after each heading and between groups, with no other runs of blank lines.
- Leave no trailing whitespace, and end the file with a single newline.

### 8. Completeness (report only)

Don't edit anything for these. List them under **Needs your input**:

- Vague or missing oven temperatures ("bake at a low temperature").
- Steps with no time or doneness cue where one matters.
- Ingredients that likely need an amount but have none.
- Anything else only the cook can answer.

## Example

Before:

```markdown
## Ingredients
- Beef - 1.5 kg
- Onions Yellow - x3
- Mushrooms (Optional)

## Cookware
- Rondeau

## Instructions

### Beef Stew
1. Add 3 tbsp of salt to the beef chunks and let dry-brine.
2. Sauté onions and mushrooms until brown
```

After:

```markdown
## Ingredients

- **Beef** - 1.5 kg
- **Onions Yellow** - x3
- **Mushrooms** (Optional)
- **Salt** - 3 tbsp

## Cookware

- **Rondeau**

## Instructions

### Beef Stew

1. Add 3 tbsp of **Salt** to the **Beef** chunks and let dry-brine.
2. Sauté **Onions** and **Mushrooms** until brown.
```

## Report format

Keep it short. Skip passes that made no changes, and list skipped files with the reason.

```text
### Beef Stew.md
Applied
- Structure: Suggestions/Notes labels → ### headings
- Ingredients: bolded names
- Cross-reference: added Garlic, Salt - 3 tbsp, Olive Oil; Cookware: Rack
- Grammar: added missing period (Beef Fat step 4)
Needs your input
- Beef Fat step 4: "bake at a low temperature" — what temperature?
```

End with: "Fixes are unstaged, so `git diff` shows only these changes. `git add <file>` accepts them; `git restore <file>` discards them."
