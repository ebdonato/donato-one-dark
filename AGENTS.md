# AGENTS.md

This file provides guidance for AI coding agents working in this repository.

## Project Overview

This is a **VS Code color theme extension** called "donato one dark". It provides two
dark theme variants based on the One Dark color scheme. The project contains **no executable
code** -- it is entirely declarative JSON. There is no TypeScript, JavaScript, build step,
or compilation involved.

### Theme Variants

| Theme | Base Background | Use Case |
|-------|----------------|----------|
| donato one dark flat | `#16191d` | Dark gray, flat/uniform look with no visible panel borders |
| donato one dark oled | `#000000` | Pure black for OLED displays to save power and reduce glow |

Both themes share identical `semanticTokenColors` and `tokenColors` sections (syntax
highlighting). They differ **only** in the `colors` section (VS Code UI chrome colors),
where the OLED variant replaces `#16191d` with `#000000` for background surfaces.

## Repository Structure

```
donato-one-dark/
  package.json                 # Extension manifest (name, version, theme contributions)
  CHANGELOG.md                 # Keep a Changelog format
  README.md                    # Extension readme
  .vscodeignore                # Files excluded from packaged extension
  .vscode/
    launch.json                # F5 launch config for Extension Development Host
  assets/
    icon.png                   # Extension marketplace icon
    icon.svg                   # Source SVG for icon
    screenshoot-flat.png       # Screenshot of flat theme
    screenshoot-oled.png       # Screenshot of OLED theme
  themes/
    donato-one-dark-flat-color-theme.json   # Flat variant (~2124 lines)
    donato-one-dark-oled-color-theme.json   # OLED variant (~2124 lines)
```

## Build / Lint / Test Commands

There are **no build, lint, or test commands** configured for this project. The extension
is purely declarative JSON and requires no compilation.

### Development Workflow

1. Edit theme JSON files in `themes/`.
2. Press **F5** in VS Code to launch the Extension Development Host.
3. In the host window, select the theme via `Preferences: Color Theme`.
4. Changes to JSON files are reflected after reloading the host window (`Ctrl+Shift+P` >
   `Developer: Reload Window`).

### Packaging and Publishing

Publishing is automated via GitHub Actions (`.github/workflows/publish.yml`).
Pushing a tag matching `v*` triggers the workflow, which publishes the extension
to both the **VS Code Marketplace** and the **Open VSX Registry**.

To publish a new version:

1. **Update the version** in `package.json` (e.g., `"version": "1.2.0"`).
2. **Update `CHANGELOG.md`** with the changes for the new version.
3. **Commit** the version bump and changelog changes.
4. **Create a git tag** matching the new version (prefixed with `v`):
   ```bash
   git tag v1.2.0
   ```
5. **Push the commit and tag** to the main branch:
   ```bash
   git push origin main --tags
   ```
6. The GitHub Actions workflow will automatically publish to both marketplaces.

**Summary (quick reference):**

```bash
# After editing theme files and updating CHANGELOG.md:
# 1. Bump version in package.json
# 2. Commit all changes
git add -A && git commit -m "v1.2.0"
# 3. Tag and push — this triggers the publish workflow
git tag v1.2.0
git push origin main --tags
```

### Validating Theme JSON

There is no automated validation. To manually check for JSON syntax errors:

```bash
# Quick JSON syntax check (PowerShell)
Get-Content themes/donato-one-dark-flat-color-theme.json | ConvertFrom-Json | Out-Null
Get-Content themes/donato-one-dark-oled-color-theme.json | ConvertFrom-Json | Out-Null

# Or with Node.js
node -e "require('./themes/donato-one-dark-flat-color-theme.json')"
node -e "require('./themes/donato-one-dark-oled-color-theme.json')"
```

## Code Style Guidelines

### JSON Formatting

- **Indentation**: 2 spaces (no tabs).
- **Trailing commas**: None (standard JSON, not JSON5/JSONC).
- **Key ordering** in theme files:
  - Top-level: `name`, `type`, `semanticHighlighting`, `semanticTokenColors`,
    `tokenColors`, `colors` (in that order).
  - `semanticTokenColors`: Keys sorted alphabetically.
  - `tokenColors`: Array of objects, each with `scope` (or `name` + `scope`) and
    `settings` containing `foreground` and/or `fontStyle`.
  - `colors`: Keys sorted alphabetically by VS Code's UI scope name
    (e.g., `activityBar.background` before `editor.background`).

### Color Values

- All colors use **lowercase hex** format: `#rrggbb` or `#rrggbbaa` (with alpha).
- No named colors (e.g., "red") or `rgb()`/`hsl()` notation.
- Hex digits use lowercase letters: `#16191d`, not `#16191D`.

### Color Palette (One Dark Based)

When adding or modifying token colors, use this established palette:

| Role | Hex | Usage |
|------|-----|-------|
| Foreground | `#abb2bf` | Default text, plain variables |
| Red | `#e06c75` | Variables, HTML tags, error-related |
| Green | `#98c379` | Strings, inserted text |
| Yellow | `#e5c07b` | Classes, types, support types |
| Blue | `#61afef` | Functions, methods, attribute names |
| Purple | `#c678dd` | Keywords, storage types, modifiers |
| Cyan | `#56b6c2` | Operators, constants, enum members |
| Orange | `#d19a66` | Constants, numbers, object properties |
| Comments | `#7f848e` | Comments, documentation |
| Accent blue | `#528bff` | Cursor, active elements |
| Badge blue | `#4d78cc` | Activity bar badge |

### Theme Consistency Rules

- Both theme files MUST share identical `semanticTokenColors` and `tokenColors` sections.
  Only the `colors` section should differ between variants.
- In the **flat** variant, the primary background is `#16191d`.
- In the **OLED** variant, the primary background is `#000000`.
- When adding a new UI color to one theme, always add it to the other as well.
- Borders and separators in the flat variant should use the same background color
  (`#16191d`) to maintain the flat/borderless appearance.
- Borders and separators in the OLED variant should use `#000000` for the same reason.

### Naming Conventions

- Theme file names follow the pattern: `donato-one-dark-{variant}-color-theme.json`.
- Theme `name` field: `"donato one dark {variant}"` (lowercase, space-separated).
- Theme `label` in `package.json`: `"donato one dark {variant}"` (must match).

## Adding a New Theme Variant

1. Copy an existing theme JSON file and rename following the naming convention.
2. Update the `name` field inside the JSON.
3. Modify the `colors` section for the new variant's look.
4. Keep `semanticTokenColors` and `tokenColors` identical to the other themes.
5. Register the new theme in `package.json` under `contributes.themes`.
6. Add a screenshot to `assets/`.
7. Update `CHANGELOG.md`.

## Adding a New Token Color Rule

1. Add the rule to the `tokenColors` array in **every** theme file.
2. Use the established color palette above. Do not introduce new colors without a reason.
3. Use TextMate scope names. Reference:
   https://macromates.com/manual/en/language_grammars#naming-conventions
4. Include a `name` field for non-obvious scopes to document intent.
5. Test the change with multiple languages using the Extension Development Host (F5).

## Key Files for Common Tasks

| Task | File(s) |
|------|---------|
| Change syntax highlighting | `themes/*-color-theme.json` (`tokenColors` section) |
| Change semantic tokens | `themes/*-color-theme.json` (`semanticTokenColors` section) |
| Change UI chrome colors | `themes/*-color-theme.json` (`colors` section) |
| Change extension metadata | `package.json` |
| Add extension icon | `assets/icon.png` + `assets/icon.svg` |
| Configure extension packaging | `.vscodeignore` |
| Update version/changelog | `package.json` + `CHANGELOG.md` |

## VS Code Theme Color Reference

The full list of customizable VS Code UI color keys is documented at:
https://code.visualstudio.com/api/references/theme-color

The TextMate token scope reference for syntax highlighting:
https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide
