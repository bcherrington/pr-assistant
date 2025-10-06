# Example Workflows

This directory contains example GitHub Actions workflow configurations for the PR Assistant.

## Available Examples

### 1. Basic PR Assistant (`basic-pr-assistant.yml`)

The simplest configuration that provides comprehensive PR analysis with default settings.

**Features:**
- Code quality review
- Issue implementation validation
- Test coverage analysis
- PR description summary
- Recommended course of action

**Use this when:**
- You want to get started quickly
- Default analysis is sufficient for your needs
- You don't need custom guidelines

### 2. PR Assistant with Custom Guidelines (`pr-assistant-with-custom-guidelines.yml`)

Advanced configuration that includes project-specific guidelines and requirements.

**Features:**
- All features from the basic configuration
- Custom security requirements
- Project-specific code quality standards
- Custom testing requirements
- Performance considerations
- Accessibility guidelines (for UI projects)

**Use this when:**
- You have specific coding standards or requirements
- You need to enforce security policies
- You have custom testing requirements
- You want to guide the AI to focus on specific areas

## How to Use

1. Choose the example that best fits your needs
2. Copy the workflow file to `.github/workflows/` in your repository
3. Rename it as desired (e.g., `pr-assistant.yml`)
4. Customize as needed for your project
5. Ensure you have set up the `AUGMENT_SESSION_AUTH` secret in your repository settings

## Customization Tips

### Trigger Events

You can customize when the PR Assistant runs:

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]
    # Only run on specific branches
    branches:
      - main
      - develop
    # Only run when specific paths change
    paths:
      - 'src/**'
      - 'tests/**'
```

### Custom Guidelines

Tailor the guidelines to your project's needs:

```yaml
custom_guidelines: |
  ### Our Team's Standards:
  
  **Code Style:**
  - Use TypeScript strict mode
  - Prefer functional programming patterns
  - Use async/await over promises
  
  **Testing:**
  - All business logic must have unit tests
  - Use Jest for testing
  - Mock external dependencies
  
  **Documentation:**
  - Update README for new features
  - Add inline comments for complex logic
  - Update API documentation
```

### Conditional Execution

Run the assistant only for certain conditions:

```yaml
jobs:
  review:
    runs-on: ubuntu-latest
    # Only run for PRs not created by bots
    if: github.event.pull_request.user.type != 'Bot'
    steps:
      - name: Run PR Assistant
        uses: bcherrington/pr-assistant@v1
        # ... rest of configuration
```

### Multiple Jobs

You can split the analysis into multiple jobs if needed:

```yaml
jobs:
  quick-review:
    runs-on: ubuntu-latest
    # Run on every push
    steps:
      - name: Quick Code Quality Check
        uses: bcherrington/pr-assistant@v1
        # ... configuration
  
  full-review:
    runs-on: ubuntu-latest
    # Only run when PR is marked as ready for review
    if: github.event.pull_request.draft == false
    steps:
      - name: Comprehensive Review
        uses: bcherrington/pr-assistant@v1
        # ... configuration with more detailed guidelines
```

## Required Permissions

All workflows need these permissions:

```yaml
permissions:
  contents: read        # To checkout the code
  pull-requests: write  # To post review comments
  issues: read         # To fetch issue details for validation
```

## Secrets Setup

Before using any workflow, set up the required secret:

1. Go to your repository → Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `AUGMENT_SESSION_AUTH`
4. Value: Your Augment session authentication JSON (contact Augment Code to obtain)
5. Click "Add secret"

## Testing Your Workflow

1. Create a test branch
2. Make some changes
3. Open a pull request
4. The workflow should trigger automatically
5. Check the Actions tab to see the workflow run
6. Review the PR comment posted by the assistant

## Troubleshooting

### Workflow doesn't trigger
- Check that the workflow file is in `.github/workflows/`
- Verify the trigger conditions match your PR
- Check the Actions tab for any errors

### Permission errors
- Ensure the workflow has the required permissions
- Verify `GITHUB_TOKEN` has the necessary scopes

### Authentication errors
- Verify `AUGMENT_SESSION_AUTH` secret is set correctly
- Contact Augment Code if you need new credentials

### No review comment posted
- Check the Actions tab for workflow logs
- Verify the PR number and repo name are correct
- Ensure the workflow completed successfully

## Best Practices

1. **Start Simple**: Begin with the basic configuration and add custom guidelines as needed
2. **Iterate**: Refine your custom guidelines based on the feedback you receive
3. **Team Input**: Involve your team in defining custom guidelines
4. **Monitor**: Review the assistant's suggestions and provide feedback
5. **Adjust**: Fine-tune guidelines to reduce noise and increase value

## Support

For issues or questions:
- Check the main README.md
- Review the workflow logs in the Actions tab
- Open an issue in the repository

