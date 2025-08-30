# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Action for AI-powered code review using Claude. It's designed to be published to GitHub Marketplace as a reusable composite action that integrates with Anthropic's claude-code-action.

## Core Architecture

### Action Structure
- **`action.yml`**: Main composite action definition that wraps claude-code-action@v1 with custom configurations
- **Review Types**: Four review modes (comprehensive, security, performance, custom) implemented via prompt engineering
- **Composite Pattern**: Uses GitHub's composite action pattern to wrap and configure the underlying claude-code-action

### Key Components
- **Action Inputs**: Configurable through `action.yml` - review type, custom prompts, file patterns, severity labels
- **Review Templates**: Located in `.github/review-templates/` - define review criteria for different scenarios
- **Example Workflows**: In `examples/` directory - demonstrate different usage patterns for end users

## Testing & Validation Commands

```bash
# Validate action.yml syntax (requires yq)
yq eval '.' action.yml

# Test workflow syntax for all examples
for workflow in examples/*.yml; do yq eval '.' "$workflow"; done

# Validate all workflows in .github/workflows
for workflow in .github/workflows/*.yml; do yq eval '.' "$workflow"; done

# Test markdown files (requires markdownlint-cli)
npm install -g markdownlint-cli
markdownlint --config .markdownlint.json --ignore node_modules --ignore .git *.md .github/**/*.md
```

## Release & Deployment

```bash
# Create a new release (triggers marketplace deployment)
git tag v1.0.0
git push origin v1.0.0

# The release workflow will automatically:
# 1. Validate the release
# 2. Create GitHub release with changelog
# 3. Update major version tag (v1 -> v1.0.0)
```

## Development Workflow

When modifying the action:
1. Update `action.yml` for input/output changes
2. Test changes using the `.github/workflows/test.yml` workflow
3. Update examples if new features are added
4. Ensure README documentation matches the implementation

## Important Implementation Details

### Review Type Handling
The action determines review behavior through the `review_type` input in `action.yml`. The prompt is dynamically constructed based on this type (see the "Determine Review Prompt" step in action.yml).

### Integration with claude-code-action
This action is a wrapper around `anthropics/claude-code-action@v1`. It passes through the Anthropic API key and configures specific tools and prompts based on the review type.

### GitHub Permissions Required
- `contents: read` - To checkout and read code
- `pull-requests: write` - To comment on PRs
- `id-token: write` - For claude-code-action authentication

### Marketplace Metadata
The action uses composite runs (`runs.using: 'composite'`) and includes branding for marketplace visibility (blue code icon).