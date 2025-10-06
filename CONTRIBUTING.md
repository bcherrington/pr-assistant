# Contributing to PR Assistant

Thank you for your interest in contributing to the PR Assistant! This document provides guidelines and instructions for contributing.

## How to Contribute

### Reporting Issues

If you encounter a bug or have a feature request:

1. Check if the issue already exists in the [Issues](../../issues) section
2. If not, create a new issue with:
   - A clear, descriptive title
   - Detailed description of the problem or feature
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - Relevant logs or screenshots
   - Your environment details (OS, GitHub Actions runner, etc.)

### Suggesting Enhancements

We welcome suggestions for improvements:

1. Open an issue with the `enhancement` label
2. Describe the enhancement in detail
3. Explain the use case and benefits
4. Provide examples if applicable

### Contributing Code

#### Getting Started

1. Fork the repository
2. Clone your fork locally
3. Create a new branch for your changes:
   ```bash
   git checkout -b feature/your-feature-name
   ```

#### Making Changes

1. **Template Changes**: If modifying prompt templates in `templates/`:
   - Maintain the existing structure (instructions, request, guidelines, formatting, capabilities, limitations)
   - Test your changes with real PRs
   - Ensure the output format remains consistent
   - Update documentation if needed

2. **Action Configuration**: If modifying `action.yml`:
   - Ensure backward compatibility
   - Update the README with any new inputs
   - Test with example workflows

3. **Documentation**: If updating documentation:
   - Keep it clear and concise
   - Include examples where helpful
   - Update all relevant files (README, example workflows, etc.)

#### Code Quality

- Follow the existing code style and conventions
- Keep changes focused and atomic
- Write clear commit messages
- Test your changes thoroughly

#### Commit Messages

Use clear, descriptive commit messages:

```
Add support for custom test coverage thresholds

- Add new input parameter for coverage threshold
- Update template to include threshold validation
- Add example workflow demonstrating usage
- Update README with new parameter documentation
```

#### Testing Your Changes

Before submitting:

1. Test with the example workflows
2. Verify the action works with different PR scenarios:
   - PRs with issues referenced
   - PRs without issues
   - PRs with tests
   - PRs without tests
   - PRs with various code quality issues
3. Check that the review output is well-formatted
4. Ensure inline comments anchor correctly

#### Submitting a Pull Request

1. Push your changes to your fork
2. Create a pull request to the main repository
3. Fill out the PR template with:
   - Description of changes
   - Motivation and context
   - Testing performed
   - Screenshots (if applicable)
4. Link any related issues
5. Wait for review and address feedback

## Development Guidelines

### Template Structure

The prompt templates follow a modular structure:

```
templates/
├── prompt.njk                 # Main template that includes all others
├── capabilities/
│   └── base.njk              # What the AI can do
├── context/
│   ├── pr_or_issue.njk       # PR context wrapper
│   └── pr_details.njk        # PR details (diff, files, etc.)
├── formatting/
│   └── base.njk              # Output formatting requirements
├── guidelines/
│   └── base.njk              # Review guidelines and focus areas
├── instructions/
│   └── base.njk              # Step-by-step instructions for the AI
├── limitations/
│   └── base.njk              # What the AI cannot do
└── request/
    └── base.njk              # Specific task requirements
```

### Template Best Practices

1. **Be Specific**: Provide clear, specific instructions
2. **Use Examples**: Include examples of good output
3. **Set Constraints**: Clearly define what should and shouldn't be done
4. **Prioritize**: Emphasize high-confidence, high-value feedback
5. **Structure Output**: Define clear output format requirements

### Testing Templates

To test template changes:

1. Set up a test repository with various PR scenarios
2. Configure the action with your modified templates
3. Create test PRs covering:
   - Simple changes
   - Complex changes
   - Security issues
   - Missing tests
   - Issue references
   - No issue references
4. Review the output for quality and accuracy

## Review Process

### What We Look For

- **Quality**: Does the change improve the action?
- **Compatibility**: Does it maintain backward compatibility?
- **Testing**: Has it been adequately tested?
- **Documentation**: Is it properly documented?
- **Scope**: Is the change focused and appropriate?

### Review Timeline

- We aim to review PRs within 3-5 business days
- Complex changes may take longer
- We may request changes or clarifications
- Be responsive to feedback

## Community Guidelines

### Code of Conduct

- Be respectful and constructive
- Welcome newcomers
- Focus on the code, not the person
- Assume good intentions
- Help others learn and grow

### Communication

- Use clear, professional language
- Provide context and rationale
- Be patient and helpful
- Ask questions when unclear

## Recognition

Contributors will be:
- Listed in the project's contributors
- Credited in release notes for significant contributions
- Thanked in the community

## Questions?

If you have questions about contributing:
- Open an issue with the `question` label
- Check existing issues and discussions
- Review the README and documentation

Thank you for contributing to PR Assistant! 🎉

