# Claude Code Review Action 🤖

AI-powered comprehensive code review using Claude with customizable review types and automated feedback for GitHub Pull Requests.

[![GitHub Super-Linter](https://github.com/your-username/claude-code-review/workflows/Test/badge.svg)](https://github.com/your-username/claude-code-review/actions)
[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Claude%20Code%20Review-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4n3NQ+4QWY+8PlOINNjCh/JfMgr7HlhEOW1dLHBNLhlRWYj5hvAUDfK6r3r6vH3N1ixh+TrJ2L8AXdKa5u3kJdz8W0PGGbm3pTrAQ+I8+7+8D+ATn7+4kBrz6F+9qeG3YR5QGBJuJH1BUUITyJhGRlTF2/vGwQ8wy6kzAacEUDLRIGsC8eWJY5qEuMQ2fN1F0PuP4JD4G8YMCgARBBD2KGjxCbQKhGIQEIrOgPgO8xhOCQdMHY5zs/jfQITt8uBgVFYDwGEJBXCiRGJcQOIZhDFaOcJogYSDhJCRXbGQYULLSRSO0lF4oTmHGK9zAAAAAElFTkSuQmCC)](https://github.com/marketplace/actions/claude-code-review)

## ✨ Features

- **🔍 Multiple Review Types**: Comprehensive, Security, Performance, and Custom reviews
- **📊 Progress Tracking**: Real-time progress with visual checkboxes
- **🏷️ Smart Labeling**: Automatic severity-based PR labels
- **💬 Inline Comments**: Contextual feedback directly on code changes
- **🎯 Customizable**: Flexible prompts and file filtering
- **⚡ Fast & Efficient**: Optimized for quick feedback cycles
- **🔒 Secure**: No data retention, runs entirely on your GitHub runner

## 🚀 Quick Start

Add this workflow to your repository at `.github/workflows/pr-review.yml`:

```yaml
name: Claude PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Important for getting full PR context
      
      - uses: your-org/claude-code-review@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          review_type: 'comprehensive'
```

## 📋 Prerequisites

1. **Anthropic API Key**: Get yours at [Anthropic Console](https://console.anthropic.com/)
2. **Repository Secret**: Add your API key as `ANTHROPIC_API_KEY` in repository secrets
3. **Permissions**: Ensure the workflow has `pull-requests: write` permission

## 🔧 Configuration

### Basic Configuration

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `anthropic_api_key` | Your Anthropic API key | ✅ Yes | - |
| `review_type` | Type of review (comprehensive/security/performance/custom) | No | `comprehensive` |
| `progress_tracking` | Enable progress tracking | No | `true` |
| `severity_labels` | Add severity labels to PR | No | `true` |

### Advanced Configuration

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `custom_prompt` | Custom review prompt | No | - |
| `files_to_review` | Specific files or patterns to review | No | All changed files |
| `exclude_paths` | Comma-separated paths to exclude | No | `node_modules,dist,build,.git` |
| `max_review_comments` | Maximum inline comments | No | `50` |
| `github_token` | GitHub token for API access | No | `${{ github.token }}` |

## 🔍 How It Works

Claude Code Review uses GitHub's native PR review API to provide:

1. **Inline Comments**: Line-specific feedback directly on the code changes
2. **Summary Review**: Overall assessment with categorized findings  
3. **Severity Labels**: Automatic labeling based on issue severity
4. **Progress Tracking**: Real-time visual progress indicators

The action integrates with [claude-code-action](https://github.com/anthropics/claude-code-action) to leverage Claude's advanced code understanding capabilities while maintaining security and privacy.

## 🎯 Review Types

### 📊 Comprehensive Review
Perfect for general code quality assessment:

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'comprehensive'
```

**Focuses on:**
- Code quality and maintainability
- Best practices and conventions
- Security considerations
- Performance implications
- Testing coverage
- Documentation quality

### 🔒 Security Review
Specialized security vulnerability assessment:

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'security'
    max_review_comments: 50
```

**Focuses on:**
- OWASP Top 10 vulnerabilities
- Authentication and authorization
- Input validation and sanitization
- Data exposure and privacy
- Dependency vulnerabilities

### ⚡ Performance Review
Optimized for performance analysis:

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'performance'
```

**Focuses on:**
- Algorithm efficiency and complexity
- Database query optimization
- Memory usage and potential leaks
- Caching strategies
- Resource utilization

### 🔧 Custom Review
Tailored to your specific needs:

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'custom'
    custom_prompt: |
      Focus on React best practices:
      1. Component structure and reusability
      2. Hook usage patterns
      3. State management
      4. Performance optimization
      5. Accessibility compliance
```

## 📁 Real-World Examples

### Path-Specific Review for TypeScript Project

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'comprehensive'
    files_to_review: 'src/**/*.{ts,tsx}'  # Only TypeScript files
    exclude_paths: 'src/**/*.test.ts,src/**/*.spec.ts,src/**/*.d.ts'
    max_review_comments: 40
```

### Security-Focused Workflow

```yaml
name: Security Review

on:
  pull_request:
    paths:
      - '**/*.js'
      - '**/*.py'
      - '**/*.go'
      - '**/Dockerfile'

jobs:
  security-review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      security-events: write
    
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/claude-code-review@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          review_type: 'security'
          severity_labels: true
```

### Multi-Stage Review Process

```yaml
jobs:
  initial-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Important for getting full PR context
      
      - uses: your-org/claude-code-review@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          review_type: 'comprehensive'
          max_review_comments: 25
  
  security-check:
    needs: initial-review
    runs-on: ubuntu-latest
    if: contains(github.event.pull_request.labels.*.name, 'security-sensitive')
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/claude-code-review@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          review_type: 'security'
```

## 📊 Outputs

The action provides detailed outputs for further automation:

```yaml
- uses: your-org/claude-code-review@v1
  id: review
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}

- name: Check Review Results
  run: |
    echo "Issues found: ${{ steps.review.outputs.issues_found }}"
    echo "Critical issues: ${{ steps.review.outputs.critical_issues }}"
    echo "High issues: ${{ steps.review.outputs.high_issues }}"
```

### Available Outputs

| Output | Description |
|--------|-------------|
| `review_summary` | Summary of review results |
| `issues_found` | Total number of issues found |
| `critical_issues` | Number of critical severity issues |
| `high_issues` | Number of high severity issues |
| `medium_issues` | Number of medium severity issues |
| `low_issues` | Number of low severity issues |

## 🏷️ Automatic Labels

The action automatically adds labels to PRs based on findings:

- `claude-reviewed` - Added to all reviewed PRs
- `security-review` - Added when security review is performed
- `performance-review` - Added when performance review is performed
- `has-critical-issues` - Added when critical issues are found
- `has-high-issues` - Added when high-priority issues are found
- `needs-attention` - Added when multiple issues are found

## ⚙️ Advanced Usage

### Conditional Reviews

```yaml
- uses: your-org/claude-code-review@v1
  if: |
    !contains(github.event.pull_request.labels.*.name, 'skip-review') &&
    github.event.pull_request.draft == false
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'comprehensive'
```

### Language-Specific Reviews

```yaml
- name: Determine review type
  id: review-type
  run: |
    if [[ "${{ github.event.pull_request.title }}" == *"security"* ]]; then
      echo "type=security" >> $GITHUB_OUTPUT
    elif [[ "${{ github.event.pull_request.title }}" == *"performance"* ]]; then
      echo "type=performance" >> $GITHUB_OUTPUT
    else
      echo "type=comprehensive" >> $GITHUB_OUTPUT
    fi

- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: ${{ steps.review-type.outputs.type }}
```

### Integration with Other Actions

```yaml
- uses: your-org/claude-code-review@v1
  id: claude-review
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'comprehensive'
    severity_labels: true

- name: Block merge if critical issues
  if: steps.claude-review.outputs.critical_issues > 0
  run: |
    echo "::error::Critical issues found. Please address before merging."
    echo "Critical: ${{ steps.claude-review.outputs.critical_issues }}"
    echo "High: ${{ steps.claude-review.outputs.high_issues }}"
    echo "Medium: ${{ steps.claude-review.outputs.medium_issues }}"
    echo "Low: ${{ steps.claude-review.outputs.low_issues }}"
    exit 1

- name: Request changes if high issues
  if: steps.claude-review.outputs.high_issues > 5
  run: |
    gh pr review ${{ github.event.pull_request.number }} \
      --request-changes \
      --body "Multiple high-priority issues found. Please review Claude's feedback."
  env:
    GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

- name: Post Review Summary to PR
  if: always()
  run: |
    echo "## 📊 Claude Review Summary" > review-summary.md
    echo "" >> review-summary.md
    echo "- **Total Issues**: ${{ steps.claude-review.outputs.issues_found }}" >> review-summary.md
    echo "- **Critical**: ${{ steps.claude-review.outputs.critical_issues }}" >> review-summary.md
    echo "- **High**: ${{ steps.claude-review.outputs.high_issues }}" >> review-summary.md
    echo "- **Medium**: ${{ steps.claude-review.outputs.medium_issues }}" >> review-summary.md
    echo "- **Low**: ${{ steps.claude-review.outputs.low_issues }}" >> review-summary.md
    echo "" >> review-summary.md
    echo "${{ steps.claude-review.outputs.review_summary }}" >> review-summary.md
    
    gh pr comment ${{ github.event.pull_request.number }} \
      --body-file review-summary.md
  env:
    GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 📚 Review Templates

The action includes built-in templates for different review types:

- **[General Review Template](.github/review-templates/general.md)** - Comprehensive code quality guidelines
- **[Security Review Template](.github/review-templates/security.md)** - Security-focused review criteria
- **[Performance Review Template](.github/review-templates/performance.md)** - Performance optimization guidelines

## 🔒 Security & Privacy

- **No Data Retention**: Code is not stored by Claude or Anthropic
- **Secure Transmission**: All communications use encrypted channels
- **Minimal Permissions**: Only requires necessary GitHub permissions
- **Runs on Your Infrastructure**: Executes entirely on GitHub's runners

## 🛠️ Troubleshooting

### Common Issues

#### Authentication Error
```
Error: Invalid API key
```
**Solution**: 
- Ensure your Anthropic API key is correctly set in repository secrets
- Verify the key starts with `sk-ant-` 
- Check the key hasn't expired in your [Anthropic Console](https://console.anthropic.com/)

#### Permission Denied
```
Error: Insufficient permissions to comment on PR
```
**Solution**: Ensure your workflow has all required permissions:
```yaml
permissions:
  contents: read
  pull-requests: write
  id-token: write
```

#### No Review Comments Appearing
**Possible Causes**:
- PR is in draft mode (add condition to skip drafts)
- Files are excluded by `exclude_paths`
- `max_review_comments` limit reached
- No issues found in the code

#### Rate Limiting
```
Error: API rate limit exceeded
```
**Solution**: 
- Reduce `max_review_comments` to limit API calls
- Add file filters to review fewer files
- Consider running on specific paths only

### Debug Mode

Enable debug logging for detailed troubleshooting:

```yaml
- uses: your-org/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
  env:
    ACTIONS_STEP_DEBUG: true
    ACTIONS_RUNNER_DEBUG: true
```

### Testing Your Configuration

Test your setup with a simple workflow:

```yaml
name: Test Claude Review
on:
  workflow_dispatch:
  pull_request:
    types: [opened]

jobs:
  test:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: your-org/claude-code-review@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          review_type: 'comprehensive'
          max_review_comments: 5  # Start small for testing
```

## 📈 Best Practices

### 1. **Start Small**
Begin with basic comprehensive reviews before moving to specialized reviews.

### 2. **Use Path Filtering**
Focus reviews on relevant files to reduce noise:
```yaml
files_to_review: 'src/**/*.{js,ts,py}'
exclude_paths: 'tests/**,docs/**,*.md'
```

### 3. **Combine with Other Tools**
Use alongside linters, formatters, and security scanners:
```yaml
- name: Lint
  run: npm run lint
- name: Security Scan
  uses: securecodewarrior/github-action-security-scan@v1
- name: Claude Review
  uses: your-org/claude-code-review@v1
```

### 4. **Set Reasonable Limits**
Adjust comment limits based on your team's bandwidth:
```yaml
max_review_comments: 20  # For busy teams
max_review_comments: 50  # For thorough reviews
```

### 5. **Use Conditional Reviews**
Skip reviews for draft PRs or documentation changes:
```yaml
if: |
  !github.event.pull_request.draft &&
  !contains(github.event.pull_request.labels.*.name, 'documentation')
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

1. Fork and clone the repository
2. Create a feature branch
3. Make your changes
4. Test with the provided examples
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/your-username/claude-code-review/issues)
- 💡 **Feature Requests**: [GitHub Discussions](https://github.com/your-username/claude-code-review/discussions)
- 📚 **Documentation**: [Wiki](https://github.com/your-username/claude-code-review/wiki)

## 🙏 Acknowledgments

- Built on top of [Claude Code Action](https://github.com/anthropics/claude-code-action)
- Powered by [Anthropic's Claude AI](https://www.anthropic.com/claude)
- Inspired by the GitHub Actions community

---

**Made with ❤️ by the Claude Code Review Team**

*Enhance your code quality with AI-powered reviews!*