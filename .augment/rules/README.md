# Augment Rules

This directory contains Augment-specific rules that guide AI behavior when working on this project.

## Available Rules

### documentation.md
Guidelines for documentation standards and practices.

### git.md
Git commit message format and version control policies.

### planning.md
Structured planning approach for complex, multi-file changes.

### preferences.md
General coding preferences including SOLID and DRY principles.

### testing.md
Testing format, standards, and policy for the project.

## Purpose

These rules are automatically loaded by Augment when working on this repository. They ensure consistent behavior and adherence to project standards.

## Customization

You can add additional rules by creating new `.md` files in this directory. Each rule should:
- Have a clear, descriptive name
- Include specific, actionable guidelines
- Be focused on a single concern
- Provide examples where helpful

## Usage

These rules are referenced in the main system prompt and influence how Augment:
- Writes code
- Creates documentation
- Plans complex changes
- Writes tests
- Formats commit messages

## Note

These rules apply to Augment's behavior when working on the PR Assistant codebase itself, not to the PR reviews performed by the action.

