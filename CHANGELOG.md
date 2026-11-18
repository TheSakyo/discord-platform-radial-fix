# Changelog

All notable changes to this project are documented in this file.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.2.5] - 2026-11-18

### Changed
- Typing pill slightly smaller (69% x 40% of the avatar).

## [1.2.4] - 2026-11-18

### Changed
- Typing pill resized to an intermediate size (72% x 42%).

## [1.2.3] - 2026-11-18

### Changed
- Typing pill enlarged (78% x 46%) and moved further to the bottom-right corner.

## [1.2.2] - 2026-11-18

### Added
- `--vc-typing-width` and `--vc-typing-height` variables to resize the typing pill.

### Changed
- Typing pill enlarged (66% x 38%).

## [1.2.1] - 2026-11-18

### Fixed
- Typing pill not showing: the "typing" state is now detected through `rect[mask*="typing"]`, `rect[x="14.5"]` or any `dots_` element.

## [1.2.0] - 2026-11-18

### Changed
- File reorganized into numbered sections with a table of contents and detailed comments.

## [1.1.0] - 2026-11-18

### Added
- Custom typing indicator: default Discord pill with three animated dots, replacing RadialStatus' misplaced gray circle.

### Removed
- Duplicated `fill: transparent` rules on the typing background.

## [1.0.0] - 2026-11-18

### Added
- Initial release: PlatformIndicators layout fixes and hidden duplicate mobile phone icon.
