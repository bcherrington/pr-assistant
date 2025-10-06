# Quick Start Guide

Get the PR Assistant up and running in 5 minutes!

## Prerequisites

- A GitHub repository
- GitHub Actions enabled
- Augment session authentication credentials (contact Augment Code)

## Step 1: Add the Secret

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `AUGMENT_SESSION_AUTH`
5. Value: Paste your Augment session authentication JSON
6. Click **Add secret**

## Step 2: Create the Workflow

Create a new file: `.github/workflows/pr-assistant.yml`

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

## Step 3: Test It

1. Create a new branch
2. Make some changes
3. Open a pull request
4. Wait for the PR Assistant to post a review (usually 1-3 minutes)

## Step 4: Review the Feedback

The PR Assistant will post a comprehensive review with:

### Review Summary
```markdown
## 📋 PR Summary
[Brief description of what the PR does]

## 🔍 Code Quality
[Overview of code quality findings]

## ✅ Issue Validation
[Summary of issue implementation status]

## 🧪 Test Coverage
[Summary of test coverage analysis]

## 🎯 Recommended Action
**[APPROVE / APPROVE WITH MINOR SUGGESTIONS / REQUEST CHANGES / NEEDS DISCUSSION]**
[Rationale for the recommendation]
```

### Inline Comments
Specific, actionable comments on relevant lines of code for:
- Code quality issues
- Issue implementation gaps
- Test coverage suggestions

## Step 5: Provide Feedback

Help improve the PR Assistant by reacting to comments:
- 👍 if the comment is helpful
- 👎 if the comment is not useful

## What's Next?

### Add Custom Guidelines

Customize the review to your project's needs:

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
      - New features must include integration tests
```

### Reference Issues in Commits

To enable issue validation, reference issues in your commit messages:

```bash
git commit -m "Add user authentication - fixes #123"
git commit -m "Update API endpoints - closes #456, resolves #789"
```

The PR Assistant will:
1. Extract issue references from commits
2. Fetch issue details from GitHub
3. Validate that the PR implements the requirements
4. Identify any missing implementations

### Improve Test Coverage

The PR Assistant analyzes test coverage across multiple levels:
- **Unit Tests**: Individual functions and methods
- **Functional Tests**: Complete features and workflows
- **Integration Tests**: Component interactions
- **E2E Tests**: Complete user journeys
- **Additional**: Security, performance, error handling

It will suggest specific test cases with:
- Test type and description
- Rationale for why the test is important
- Priority level (High/Medium/Low)

## Common Scenarios

### Scenario 1: New Feature PR

**What the PR Assistant checks:**
- ✅ Code quality and best practices
- ✅ Issue requirements are met (if referenced)
- ✅ Adequate test coverage for new code
- ✅ Security considerations
- ✅ Error handling

**Expected recommendation:**
- APPROVE WITH MINOR SUGGESTIONS (if tests are good)
- REQUEST CHANGES (if missing critical tests)

### Scenario 2: Bug Fix PR

**What the PR Assistant checks:**
- ✅ Fix addresses the root cause
- ✅ Issue requirements are met
- ✅ Regression tests are added
- ✅ Edge cases are handled
- ✅ No new bugs introduced

**Expected recommendation:**
- APPROVE (if well-tested)
- REQUEST CHANGES (if missing regression tests)

### Scenario 3: Refactoring PR

**What the PR Assistant checks:**
- ✅ Code quality improvements
- ✅ No functionality changes
- ✅ Existing tests still pass
- ✅ Maintainability improvements
- ✅ Performance implications

**Expected recommendation:**
- APPROVE (if tests are maintained)
- APPROVE WITH MINOR SUGGESTIONS (if minor improvements possible)

## Troubleshooting

### No review posted

**Check:**
1. Workflow ran successfully (Actions tab)
2. `AUGMENT_SESSION_AUTH` secret is set correctly
3. Workflow has correct permissions
4. PR number and repo name are correct

### Review is too noisy

**Solution:**
Add custom guidelines to focus on specific areas:

```yaml
custom_guidelines: |
  Focus on:
  - Security vulnerabilities
  - Critical bugs
  - Missing tests for new features
  
  Ignore:
  - Minor style issues
  - Personal preferences
  - Non-critical optimizations
```

### Review misses important issues

**Solution:**
The PR Assistant prioritizes high-confidence suggestions. If it's missing issues:
1. Ensure the code changes are in the PR diff
2. Add custom guidelines for specific patterns to check
3. Provide feedback via 👎 reactions to help improve

### Want to skip review for certain PRs

**Solution:**
Add conditions to the workflow:

```yaml
jobs:
  review:
    runs-on: ubuntu-latest
    # Skip for dependabot PRs
    if: github.event.pull_request.user.login != 'dependabot[bot]'
    steps:
      # ... rest of workflow
```

## Tips for Best Results

1. **Write clear commit messages** with issue references
2. **Include tests** in your PRs
3. **Provide context** in PR descriptions
4. **React to comments** to help improve the assistant
5. **Customize guidelines** for your project's needs
6. **Review the feedback** and address high-priority items

## Getting Help

- 📖 Read the [full README](README.md)
- 🔧 Check [example workflows](example-workflows/)
- 📊 Review the [project summary](PROJECT_SUMMARY.md)
- 🐛 [Open an issue](../../issues) for bugs or questions
- 💡 [Suggest enhancements](../../issues/new)

## Next Steps

1. ✅ Set up the basic workflow
2. ✅ Test with a sample PR
3. ✅ Review the feedback
4. ✅ Add custom guidelines
5. ✅ Reference issues in commits
6. ✅ Improve test coverage
7. ✅ Share with your team

Happy reviewing! 🚀

