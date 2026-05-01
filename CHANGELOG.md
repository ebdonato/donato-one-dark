# Changelog

All notable changes to the "donato-one-dark" extension will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.1.3] - 2026-05-01

### Added

- Google Antigravity IDE compatibility — the theme now explicitly supports Antigravity IDE
- Added `antigravity` and `google antigravity` keywords to `package.json` for discoverability on Open VSX
- Added `antigravity` engine entry in `package.json`
- Updated README with Antigravity IDE installation instructions and compatibility note
- Updated extension description to mention Google Antigravity IDE

### Fixed

- Lowered `engines.vscode` minimum from `^1.110.0` to `^1.60.0` — fixes "not compatible" error when installing on Antigravity IDE (v1.107.0). Color themes are purely declarative and don't require recent VS Code APIs.

## [1.1.2] - 2026-04-11

### Fixed

- Fixed visible borders in OLED theme variant by changing the following border colors from `#181a1f` to `#000000`:
  - `tab.border` — eliminated visible tab borders
  - `editorHoverWidget.border` — eliminated hover tooltip borders
  - `editorSuggestWidget.border` — eliminated autocomplete widget borders
  - `input.border` — eliminated input field borders
  - `notificationCenter.border` — eliminated notification center borders
- OLED theme now maintains consistent borderless aesthetic matching the flat variant

## [1.1.0] - 2026-04-04

### Added

- Author section in README with avatar image

## [1.0.1] - 2026-04-02

### Fixed

- Corrected version number for marketplace publishing

## [1.0.0] - 2026-04-02

### Added

- GitHub Actions CI/CD workflow for automatic publishing to the VS Code Marketplace
- `.vscodeignore` configuration for cleaner extension packaging
- `$schema` field in theme JSON files for editor validation
- MIT LICENSE file
- New UI color tokens: breadcrumbs, diff editor removed text highlight, dropdown foreground, editor inactive selection background, editor ghost text, editor group border and drop background, editor hint/info foreground, editor overview ruler (border, error, info, warning), git decorations (conflicting, deleted, modified resources)

### Fixed

- Token name corrected from "support php constants" to "support rust constants"
- Normalized all hex color values to lowercase (e.g. `#BE5046` → `#be5046`)

### Changed

- Renamed screenshot assets (`screenshoot-flat.png` → `screenshot-flat.png`, `screenshoot-oled.png` → `screenshot-oled.png`)

## [0.0.1] - 2025-01-01

### Added

- Initial release with two dark theme variants:
  - **donato one dark flat** — dark gray (`#16191d`) flat/borderless theme
  - **donato one dark oled** — pure black (`#000000`) theme for OLED displays
- Syntax highlighting based on the One Dark color palette
- Semantic token colors for Dart, Rust, and TOML
