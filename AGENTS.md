# Agent Guidelines

## Primary Agent Modes
- Default coding agent: implementation, debugging, and delivery tasks.
- business-analyst agent: requirement analysis, epic/story scoping, and acceptance criteria authoring.

## Scoping Workflow
For requirement scoping tasks, use the requirements-scoping skill in `.github/skills/requirements-scoping/SKILL.md` and produce:
- Numbered epic directories
- Numbered story files
- User stories in As a / I want / so that format
- Acceptance criteria in Given / When / Then format

## Artifact Conventions
- Epic folders: zero-padded sequence with slug, e.g. `001_discovery`.
- Story files: zero-padded global sequence, e.g. `014_gather_requirements.md`.
- Story numbering starts at `(epic_count + 1)` and increments globally.

## Quality Gate
- Each story includes one clear role, action, and business outcome.
- Each story includes at least one testable acceptance criterion.
- Avoid ambiguous wording and non-testable outcomes.
