Setup Guide

## Manual Setup (Direct API)

**Requirements**: You must be a repository admin to complete these steps.

1. Install the Claude GitHub app to your repository: https://github.com/apps/claude
2. Add authentication to your repository secrets ([Learn how to use secrets in GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)):
    - Either `ANTHROPIC_API_KEY` for API key authentication
    - Or `CLAUDE_CODE_OAUTH_TOKEN` for OAuth token authentication (Pro and Max users can generate this by running `claude setup-token` locally)
3. Copy the workflow file from [`examples/basic-review.yml`](../examples/basic-review.yml) into your repository's `.github/workflows/`

## Security Best Practices

**⚠️ IMPORTANT: Never commit API keys directly to your repository! Always use GitHub Actions secrets.**

To securely use your Anthropic API key:

1. Add your API key as a repository secret:

    - Go to your repository's Settings
    - Navigate to "Secrets and variables" → "Actions"
    - Click "New repository secret"
    - Name it `ANTHROPIC_API_KEY`
    - Paste your API key as the value

2. Reference the secret in your workflow:
   ```yaml
   anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
   ```

**Never do this:**

```yaml
# ❌ WRONG - Exposes your API key
anthropic_api_key: "sk-ant-..."
```

**Always do this:**

```yaml
# ✅ CORRECT - Uses GitHub secrets
anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```

This applies to all sensitive values including API keys, access tokens, and credentials.
We also recommend that you always use short-lived tokens when possible

## Setting Up GitHub Secrets

1. Go to your repository's Settings
2. Click on "Secrets and variables" → "Actions"
3. Click "New repository secret"
4. For authentication, choose one:
    - API Key: Name: `ANTHROPIC_API_KEY`, Value: Your Anthropic API key (starting with `sk-ant-`)
    - OAuth Token: Name: `CLAUDE_CODE_OAUTH_TOKEN`, Value: Your Claude Code OAuth token (Pro and Max users can generate this by running `claude setup-token` locally)
5. Click "Add secret"

### Best Practices for Authentication

1. ✅ Always use `${{ secrets.ANTHROPIC_API_KEY }}` or `${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}` in workflows
2. ✅ Never commit API keys or tokens to version control
3. ✅ Regularly rotate your API keys and tokens
4. ✅ Use environment secrets for organization-wide access
5. ❌ Never share API keys or tokens in pull requests or issues
6. ❌ Avoid logging workflow variables that might contain keys