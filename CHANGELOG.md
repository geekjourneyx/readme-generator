# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog, and this project follows semantic versioning.

## [Unreleased]

## [1.1.0] - 2026-06-02

### Added

- Added portfolio-grade README generation guidance focused on project story, visual quality, and reduced noise.
- Added Codex Image Gen / gpt-image-2 guidance for README cover assets, with clear boundaries for when HTML screenshots remain the better choice.
- Added GitHub Description and Topics recommendation workflow with scoring, star ratings, and optional `gh repo edit` guidance.
- Added `npm run showcase` to regenerate this repository's README visual assets from the templates.

### Changed

- Reworked the README from a feature-heavy explanation into a portfolio-style project introduction.
- Redesigned the banner, features, and workflow templates as large-type editorial posters.
- Regenerated `assets/banner.png`, `assets/features.png`, and `assets/workflow.png` using the new visual system.
- Updated the Skill workflow around story-first README design, visual mode selection, and metadata recommendations.

## [1.0.0] - 2026-04-20

### Added

- Initial README generator Skill.
- Added HTML templates for banner, features, and workflow infographics.
- Added Playwright-based screenshot script for generating 16:9 PNG assets.
- Added README structure guidance, MIT license handling, and GitHub metadata suggestions.
