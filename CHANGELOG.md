# Changelog

All notable changes to Classroom Seating Planner are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Version numbers follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

---

## [0.6.0] — 2026-05-08

### Added
- Multi-class library: multiple named classes can now be stored side-by-side in localStorage and switched via a compact selector in Step 1.
- Auto-save: every change (layout, students, constraints, current plan) is saved automatically — no save button needed.
- New JSON schema v1.0 with a `version` field, a `meta` block (`name`, `created`, `modified`), students as a plain string array, and clean typed constraint objects.
- Import validation: the app checks imported files for schema errors and surfaces warnings for unrecognised fields or mismatched names.
- Migration path: old localStorage data from previous versions is detected and converted to the v1.0 format on first load.

### Changed
- Export now writes v1.0 schema. Files exported from earlier versions are still importable via the migration path.

---

## [0.5.0] — 2026-05-08

### Added
- **Reshuffle similarity metric** — after reshuffling, the UI shows "X% of seats changed" so the teacher can judge how much variation occurred.
- **Plan flexibility indicator** — a live coloured bar above the grid shows the percentage of students who are constrained (green / amber / red thresholds).

### Changed
- Layout restructure: Generate, Print, and reshuffle feedback moved from the sidebar to a toolbar in the main area above the grid.
- Step 3 sidebar now contains constraints only; the main area handles plan actions.

---

## [0.4.0] — 2026-05-08

### Changed
- *Sit at Front* placement algorithm rewritten: students are now placed starting from row 0 with explicit row-by-row fallback rather than a front-half heuristic. Separation constraints are respected when choosing a fallback row.
- Scoring now reflects the actual row a student was placed in, not a binary front-half flag.

### Fixed
- Front-placed students could previously land outside row 0 without feedback. The fallback logic now handles this gracefully and is reflected in the score.

---

## [0.3.0] — 2026-05-08

### Added
- **Projection mode** — a hide/reveal toggle collapses the constraint list to a neutral "Constraints are set" label, hiding all names and counts so students cannot see who is constrained when the plan is displayed on a projector.

### Changed
- Constraint UI redesigned: card-based type picker replaces the dropdown selector.
- Constraints are now grouped by type (Front / Separated) for easier scanning.

---

## [0.2.0] — 2026-05-08

### Changed
- Full UI redesign: dark sidebar with Inter font throughout.
- Seat styles updated with gradient fills and a cleaner board indicator.
- Step navigation, buttons, and form inputs polished for a more consistent visual language.

---

## [0.1.0] — 2026-05-08

### Added
- Single-file React 18 app (`index.html`) with no build step — runs directly in the browser via CDN (React, Tailwind CSS, Babel standalone).
- **Grid painter** — click or drag to mark which cells in the room are active seats; configurable rows and columns.
- **Student list** — paste comma- or newline-separated names; duplicate detection flags repeated entries.
- **Monte Carlo plan generation** — 200 candidate layouts scored and ranked; best layout selected automatically.
- **Constraint system** — two types: *Sit at Front* (one student) and *Keep Separated* (two students, distance maximised).
- **Drag-and-drop seat swapping** — manually swap two students on the grid after generation.
- **Print layout** — hides sidebar, renders grid with student names for clean printing.
- **Save / Load JSON** — export the current class as a JSON file; import a file to restore it.
- Deployed to GitHub Pages at https://vcanela.github.io/seating-planner/

---

[Unreleased]: https://github.com/vcanela/seating-planner/compare/v0.6.0...HEAD
[0.6.0]: https://github.com/vcanela/seating-planner/compare/v0.5.0...v0.6.0
[0.5.0]: https://github.com/vcanela/seating-planner/compare/v0.4.0...v0.5.0
[0.4.0]: https://github.com/vcanela/seating-planner/compare/v0.3.0...v0.4.0
[0.3.0]: https://github.com/vcanela/seating-planner/compare/v0.2.0...v0.3.0
[0.2.0]: https://github.com/vcanela/seating-planner/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/vcanela/seating-planner/releases/tag/v0.1.0
