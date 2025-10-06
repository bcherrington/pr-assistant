# Augment PR Assistant

Comprehensive AI-powered pull request analysis that combines code quality review, issue implementation validation, and test coverage analysis into a single, actionable review.

## Features

The PR Assistant performs a multi-dimensional analysis of your pull requests:

### 🔍 Code Quality Review
- Identifies potential bugs, logic errors, and edge cases
- Checks for security vulnerabilities and best practices violations
- Assesses code clarity, maintainability, and adherence to standards
- Evaluates error handling, resource management, and performance
- Posts specific, actionable inline comments on relevant code

### ✅ Issue Implementation Validation
- Automatically extracts GitHub issue references from commit messages
- Fetches issue details including requirements and acceptance criteria
- Compares PR changes against issue requirements
- Identifies implementation gaps or missing requirements
- Validates that all referenced issues are properly addressed

### 🧪 Test Coverage Analysis
- Analyzes test coverage across multiple levels (unit, functional, integration, E2E)
- Identifies test coverage gaps for new and modified code
- Suggests specific, high-value test cases with clear rationale
- Considers security, performance, error handling, and edge cases
- Prioritizes test suggestions based on risk and impact

### 📋 PR Description Summary
- Generates a clear, concise summary of what the PR accomplishes
- Focuses on business value and technical impact
- Included in the review comment for easy reference

### 🎯 Recommended Course of Action
- Provides a clear recommendation: APPROVE, APPROVE WITH MINOR SUGGESTIONS, REQUEST CHANGES, or NEEDS DISCUSSION
- Includes rationale based on all analysis dimensions
- Helps teams make informed merge decisions

## Usage

### GitHub Actions

Create a workflow file (e.g., `.github/workflows/pr-assistant.yml`):

```yaml
name: PR Assistant

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write
  issues: read

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - name: Comprehensive PR Review
        uses: bcherrington/pr-assistant@v1
        with:
          augment_session_auth: ${{ secrets.AUGMENT_SESSION_AUTH }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          pull_number: ${{ github.event.pull_request.number }}
          repo_name: ${{ github.repository }}
```

### Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `augment_session_auth` | Yes | Augment session authentication JSON containing `accessToken` and `tenantURL`. Store as repository secret. |
| `github_token` | Yes | GitHub token for API access. Must have `repo` scope. Use `${{ secrets.GITHUB_TOKEN }}` in workflows. |
| `pull_number` | Yes | The number of the pull request being reviewed. Use `${{ github.event.pull_request.number }}`. |
| `repo_name` | Yes | The full name (owner/repo) of the repository. Use `${{ github.repository }}`. |
| `custom_guidelines` | No | Optional custom guidelines to include in the review process. |
| `model` | No | Optional model name to use for the AI analysis. |
| `rules` | No | Optional JSON array of rules file paths. |
| `mcp_configs` | No | Optional JSON array of MCP config file paths. |

### Custom Guidelines

You can provide custom guidelines specific to your project:

```yaml
- name: Comprehensive PR Review
  uses: bcherrington/pr-assistant@v1
  with:
    augment_session_auth: ${{ secrets.AUGMENT_SESSION_AUTH }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
    pull_number: ${{ github.event.pull_request.number }}
    repo_name: ${{ github.repository }}
    custom_guidelines: |
      - All API endpoints must include rate limiting
      - Database queries must use parameterized statements
      - All user inputs must be validated and sanitized
      - New features must include integration tests
```

### On-Demand Reviews

You can also trigger reviews on-demand by adding a label to a PR:

```yaml
name: On-Demand PR Assistant

on:
  pull_request:
    types: [labeled]

permissions:
  contents: read
  pull-requests: write
  issues: read

jobs:
  review:
    if: github.event.label.name == 'auggie_review'
    runs-on: ubuntu-latest
    steps:
      - name: Run PR Assistant
        uses: bcherrington/pr-assistant@v1
        with:
          augment_session_auth: ${{ secrets.AUGMENT_SESSION_AUTH }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          pull_number: ${{ github.event.pull_request.number }}
          repo_name: ${{ github.repository }}

      - name: Remove trigger label
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.removeLabel({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              name: 'auggie_review'
            });

      - name: Add completion label
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              labels: ['auto-reviewed']
            });
```

**To use:** Add the `auggie_review` label to any PR to trigger a review

## Review Output

The PR Assistant posts a single comprehensive review comment with:

### Review Summary
A structured summary including:
- **PR Summary**: Brief description of what the PR does
- **Code Quality**: Overview of code quality findings
- **Issue Validation**: Summary of issue implementation status
- **Test Coverage**: Summary of test coverage analysis
- **Recommended Action**: Clear recommendation with rationale

### Inline Comments
Specific, actionable comments on relevant lines of code for:
- Code quality issues and improvements
- Issue implementation gaps
- Test coverage suggestions

Each inline comment includes:
- Clear description of the issue or suggestion
- Rationale explaining why it matters
- Concrete examples or code suggestions
- Priority level (for test suggestions)

## Philosophy

The PR Assistant follows these principles:

### Quality Over Quantity
- **High confidence only**: Only suggests changes with high confidence
- **Less noise**: Better to miss some issues than create noise with low-confidence suggestions
- **Actionable feedback**: Every suggestion is specific and actionable

### Comprehensive Analysis
- **Multi-dimensional**: Covers code quality, requirements, and testing
- **Context-aware**: Considers issue requirements and acceptance criteria
- **Risk-based**: Prioritizes critical issues and high-value tests

### Developer-Friendly
- **Constructive tone**: Focuses on helping developers improve
- **Clear rationale**: Explains why each suggestion matters
- **Practical recommendations**: Balances thoroughness with practicality

## Issue Reference Patterns

The assistant automatically detects issue references in commit messages using these patterns:

- Simple reference: `#123`
- Closing keywords: `fixes #123`, `closes #456`, `resolves #789`
- Alternative forms: `fix #123`, `close #456`, `resolve #789`
- Multiple issues: `fixes #123, closes #456`
- Case insensitive matching

## Test Coverage Framework

The assistant analyzes test coverage across multiple levels:

- **Unit Testing**: Individual functions, methods, classes; edge cases; error handling
- **Functional Testing**: Complete features, user workflows, business logic
- **Integration Testing**: Component interactions, API contracts, database interactions
- **System/E2E Testing**: Complete user journeys, system-level requirements
- **Additional Areas**: Security, performance, error handling, edge cases, regression

## Recommended Actions

The assistant provides one of four recommendations:

- **✅ APPROVE**: No significant issues, all requirements met, adequate test coverage
- **✅ APPROVE WITH MINOR SUGGESTIONS**: Minor improvements suggested but not blocking
- **⚠️ REQUEST CHANGES**: Significant issues, missing requirements, or critical test gaps
- **💬 NEEDS DISCUSSION**: Complex issues or trade-offs requiring team discussion

## Setup

### 1. Get Augment Session Authentication

Contact Augment Code to obtain your session authentication credentials.

### 2. Add Repository Secret

Add `AUGMENT_SESSION_AUTH` as a repository secret:
1. Go to your repository settings
2. Navigate to Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `AUGMENT_SESSION_AUTH`
5. Value: Your Augment session authentication JSON

### 3. Configure Workflow

Create or update your workflow file with the PR Assistant action.

### 4. Set Permissions

Ensure your workflow has the necessary permissions:
```yaml
permissions:
  contents: read
  pull-requests: write
  issues: read
```

## Examples

See the `example-workflows` directory for additional workflow examples and configurations.

## License

MIT License - see LICENSE file for details

## Troubleshooting

### IDE Warnings in action.yml

If you see warnings about "undefined parameter" in your IDE when viewing `action.yml`, these are false positives. See [docs/IDE_WARNINGS.md](docs/IDE_WARNINGS.md) for a detailed explanation.

### Common Issues

- **No review posted**: Check the Actions tab for workflow logs and verify secrets are set correctly
- **Permission errors**: Ensure the workflow has `pull-requests: write` and `issues: read` permissions
- **Authentication errors**: Verify the `AUGMENT_SESSION_AUTH` secret is set correctly

## Support

For issues, questions, or feature requests, please open an issue in this repository.

