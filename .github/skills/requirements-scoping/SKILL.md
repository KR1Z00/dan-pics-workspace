---
name: requirements-scoping
description: 'Scope product requirements into numbered epics and user stories with Given/When/Then acceptance criteria. Use when creating project scope docs, Jira-ready stories, or milestone backlogs.'
argument-hint: 'Provide business context docs, epic list, and numbering rules for epic/story file generation.'
user-invocable: true
---

# Requirements Scoping

## Purpose
Convert business requirements into a structured, implementation-ready scope package with:
- Numbered epic directories
- Numbered user-story markdown files
- Consistent user story and acceptance criteria format

## When To Use
- Creating initial project scope from discovery notes
- Converting client emails/workshops into Jira-ready stories
- Standardizing backlog artifacts for quoting and delivery planning

## Required Formats
Use this user story format exactly:
- As a {role}, I want to {do some action}, so that I can {achieve some outcome}

Use this acceptance criteria format exactly:
- Given {precondition}, when I {do some input}, then {expected output}

## Procedure
1. Collect context and identify the definitive epic list.
2. Create epic directories with zero-padded numbering, e.g. `001_discovery`.
3. Set the first story number to `(epic_count + 1)`.
4. Create story files in each epic directory using zero-padded global story numbering.
5. In each story file, include one user story and one or more acceptance criteria.
6. Validate naming consistency, numbering continuity, and criteria completeness.

## Quality Checklist
- Epic numbering is contiguous and zero-padded.
- Story numbering is contiguous and starts at `epic_count + 1`.
- Every story includes role, action, and outcome.
- Every story has at least one valid Given/When/Then criterion.
- Story language is testable and avoids ambiguous outcomes.
