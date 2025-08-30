# General Code Review Template

This template provides comprehensive guidelines for general code reviews using Claude Code Review Action.

## Review Focus Areas

### 1. Code Quality & Maintainability
- [ ] Code follows established patterns and conventions
- [ ] Functions and classes have single responsibilities
- [ ] Code is readable and well-structured
- [ ] Complex logic is properly commented
- [ ] Magic numbers and strings are avoided

### 2. Best Practices & Standards
- [ ] Naming conventions are consistent and descriptive
- [ ] Code follows language-specific best practices
- [ ] Dependencies are properly managed
- [ ] Code is DRY (Don't Repeat Yourself)
- [ ] SOLID principles are applied where appropriate

### 3. Error Handling & Robustness
- [ ] Proper error handling is implemented
- [ ] Edge cases are considered and handled
- [ ] Input validation is performed where necessary
- [ ] Graceful degradation is implemented
- [ ] Logging is appropriate and informative

### 4. Performance Considerations
- [ ] No obvious performance bottlenecks
- [ ] Efficient algorithms and data structures
- [ ] Resource usage is optimized
- [ ] Database queries are efficient
- [ ] Memory leaks are avoided

### 5. Security Awareness
- [ ] No sensitive data exposure
- [ ] Input sanitization is performed
- [ ] Authentication/authorization is proper
- [ ] No obvious security vulnerabilities
- [ ] Dependencies are up to date

### 6. Testing & Documentation
- [ ] Code is testable
- [ ] Test coverage is adequate
- [ ] Documentation is clear and complete
- [ ] API documentation is updated
- [ ] README is current

### 7. Architecture & Design
- [ ] Code follows project architecture
- [ ] Design patterns are used appropriately
- [ ] Separation of concerns is maintained
- [ ] Interfaces are well-defined
- [ ] Code is modular and reusable

## Severity Guidelines

### Critical 🔴
- Security vulnerabilities
- Data loss potential
- System crashes
- Breaking changes without migration

### High 🟡
- Performance issues
- Logic errors
- Missing error handling
- Accessibility problems

### Medium 🟠
- Code quality issues
- Minor performance concerns
- Style inconsistencies
- Missing documentation

### Low 🟢
- Suggestions for improvement
- Alternative implementations
- Code organization
- Minor optimizations

## Review Comments Template

When providing feedback, use this structure:

```markdown
**Issue**: [Brief description of the problem]

**Impact**: [Explain why this matters]

**Suggestion**: [Provide specific recommendation]

**Example**: [Show corrected code if applicable]

**Severity**: [Critical/High/Medium/Low]
```

## Example Usage

```yaml
- name: Run General Code Review
  uses: your-username/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'comprehensive'
    progress_tracking: true
    severity_labels: true
```