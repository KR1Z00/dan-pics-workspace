# Engineering Conventions

This directory contains opinionated conventions for each major layer of the PeakReps stack.

These docs are based on:

- existing repository guidance such as `AGENTS.md`, READMEs, and dependency-rule docs
- structural analysis of the current codebases and Terraform configuration
- a best-case default for how new work should be implemented without inventing one-off patterns

## Available Convention Sets

- `flutter/` for the Melos-managed Flutter monorepo
- `backend-nestjs/` for the NestJS and Prisma backend
- `aws-infra/` for Terraform-managed AWS infrastructure
- `next-react-website/` for the Next.js marketing website and shared `core` package

## Intended Use

Use these docs when:

- starting a new feature
- refactoring an older area toward current standards
- reviewing pull requests for architectural consistency
- deciding where logic should live before writing code

If a live code path conflicts with these docs, treat the docs as the target convention unless there is an explicit reason that the existing implementation must differ.