# Security Guidelines

## Credential Management

### If Credentials Were Exposed

If you believe any API tokens or credentials from this repository have been exposed:

1. **Immediately revoke** the exposed token in your Invoice Ninja instance:
   - Log in to Invoice Ninja
   - Navigate to Settings > Account Management > API Tokens
   - Delete or revoke the compromised token

2. **Rotate credentials:**
   - Generate a new API token
   - Update your `.env` file with the new token
   - Restart the MCP server

3. **Review access logs** in your Invoice Ninja instance for unauthorized API activity

4. **Check Git history** to ensure no credentials were previously committed:
   ```bash
   git log --all --oneline -- .env .claude/settings.local.json
   ```

### Best Practices

- **Never commit secrets** to version control, even in example files or test configurations
- **Use .env and .env.local** (listed in `.gitignore`) for all sensitive values
- **Use a restricted API token:**
  - Create a token with minimal necessary scopes in Invoice Ninja
  - Avoid using admin or super-user tokens for MCP integrations
  - Use separate tokens for different environments (dev, test, prod)

- **Secure your local environment:**
  - Restrict file permissions on `.env`: `chmod 600 .env`
  - Do not share `.env` files across systems
  - Store in a password manager or secrets vault for team collaboration

- **Installation security:**
  - Pin exact package versions when installing from npm
  - Verify the package contents match reviewed source code
  - Use `npm audit` and `pnpm audit` to check for dependency vulnerabilities
  - Consider installing from a local build or verified commit hash rather than latest npm

## Code Review Recommendations

This MCP server exposes powerful Invoice Ninja operations:
- Creating, updating, and deleting invoices and quotes
- Sending invoice emails
- Marking invoices as paid
- Converting quotes to invoices

**Mitigation:**
- Use an API token with minimal required permissions
- Review Invoice Ninja audit logs regularly
- Use this MCP server only with trusted AI models or clients
- Consider implementing confirmation prompts for destructive operations

## Reporting Security Issues

If you discover a security vulnerability in this project, please follow the original repository's security policy. Do not disclose vulnerabilities publicly.
