# Security Code Review Template

This template focuses specifically on security aspects of code changes using Claude Code Review Action.

## Security Review Focus Areas

### 1. OWASP Top 10 Vulnerabilities
- [ ] **Injection** - SQL, NoSQL, OS, LDAP injection vulnerabilities
- [ ] **Broken Authentication** - Session management, password policies
- [ ] **Sensitive Data Exposure** - Encryption, data protection
- [ ] **XML External Entities (XXE)** - XML parsing vulnerabilities
- [ ] **Broken Access Control** - Authorization and permission checks
- [ ] **Security Misconfiguration** - Default configurations, unnecessary features
- [ ] **Cross-Site Scripting (XSS)** - Input validation and output encoding
- [ ] **Insecure Deserialization** - Object deserialization vulnerabilities
- [ ] **Components with Known Vulnerabilities** - Dependency security
- [ ] **Insufficient Logging & Monitoring** - Security event tracking

### 2. Input Validation & Sanitization
- [ ] All user inputs are validated and sanitized
- [ ] File upload restrictions are in place
- [ ] SQL parameters are properly escaped/parameterized
- [ ] Regular expressions are safe from ReDoS attacks
- [ ] Size limits are enforced for inputs

### 3. Authentication & Authorization
- [ ] Strong password requirements
- [ ] Multi-factor authentication support
- [ ] Session management is secure
- [ ] JWT tokens are properly validated
- [ ] Role-based access control is implemented
- [ ] Privilege escalation is prevented

### 4. Data Protection
- [ ] Sensitive data is encrypted at rest
- [ ] Data is encrypted in transit (HTTPS/TLS)
- [ ] Personal data handling complies with privacy laws
- [ ] Secrets are not hardcoded in source code
- [ ] Environment variables are used for sensitive configuration

### 5. API Security
- [ ] Rate limiting is implemented
- [ ] API authentication is required
- [ ] CORS is properly configured
- [ ] Request size limits are enforced
- [ ] Proper HTTP methods are used

### 6. Cryptography
- [ ] Strong encryption algorithms are used
- [ ] Keys are properly managed and rotated
- [ ] Random number generation is cryptographically secure
- [ ] Hashing algorithms are appropriate (bcrypt, scrypt, Argon2)
- [ ] Certificates are properly validated

### 7. Error Handling & Information Disclosure
- [ ] Error messages don't reveal sensitive information
- [ ] Stack traces are not exposed in production
- [ ] Debug information is disabled in production
- [ ] Logging doesn't contain sensitive data

### 8. Infrastructure Security
- [ ] Container security best practices
- [ ] Secure network configuration
- [ ] Resource limits are set
- [ ] Least privilege principle is followed

## Security Severity Guidelines

### Critical 🔴
- Remote code execution vulnerabilities
- SQL injection vulnerabilities
- Authentication bypass
- Sensitive data exposure
- Privilege escalation

### High 🟡
- Cross-site scripting (XSS)
- Cross-site request forgery (CSRF)
- Insecure direct object references
- Security misconfiguration
- Insecure cryptographic storage

### Medium 🟠
- Information disclosure
- Insecure communications
- Missing security headers
- Weak password requirements
- Insufficient logging

### Low 🟢
- Security recommendations
- Defense in depth improvements
- Security documentation
- Non-critical configuration issues

## Security Review Checklist

### Before Deployment
- [ ] All dependencies are updated
- [ ] Security scanning tools have been run
- [ ] Secrets are properly managed
- [ ] Security headers are configured
- [ ] Error handling is secure

### Code Changes
- [ ] No hardcoded credentials
- [ ] Input validation is comprehensive
- [ ] Authentication checks are present
- [ ] Authorization is properly implemented
- [ ] Logging is security-aware

## Common Security Anti-Patterns to Avoid

```javascript
// ❌ Bad: SQL Injection vulnerability
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Good: Parameterized query
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);
```

```python
# ❌ Bad: Hardcoded secrets
API_KEY = "sk-1234567890abcdef"

# ✅ Good: Environment variables
API_KEY = os.environ.get('API_KEY')
```

```php
// ❌ Bad: Direct user input in HTML
echo "<h1>Welcome " . $_GET['name'] . "</h1>";

// ✅ Good: Escaped output
echo "<h1>Welcome " . htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8') . "</h1>";
```

## Security Review Comment Template

```markdown
**Security Issue**: [Brief description of the vulnerability]

**Risk Level**: [Critical/High/Medium/Low]

**OWASP Category**: [Relevant OWASP Top 10 category if applicable]

**Impact**: [Potential consequences of this vulnerability]

**Recommendation**: [Specific steps to fix the issue]

**Example Fix**: 
```[language]
[Secure code example]
```

**References**: [Links to security resources or documentation]
```

## Example Usage

```yaml
- name: Run Security Review
  uses: your-username/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'security'
    track_progress: true
    severity_labels: true
    max_review_comments: 50
```