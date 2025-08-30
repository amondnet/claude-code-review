# Performance Code Review Template

This template focuses on performance optimization and efficiency in code reviews using Claude Code Review Action.

## Performance Review Focus Areas

### 1. Algorithm Efficiency
- [ ] Time complexity is optimal for the use case
- [ ] Space complexity is reasonable
- [ ] Appropriate data structures are used
- [ ] Nested loops are minimized or optimized
- [ ] Recursive functions have proper termination conditions

### 2. Database Performance
- [ ] Queries are optimized and indexed properly
- [ ] N+1 query problems are avoided
- [ ] Database connections are properly pooled
- [ ] Transactions are used efficiently
- [ ] Batch operations are preferred over individual queries

### 3. Memory Management
- [ ] Memory leaks are prevented
- [ ] Objects are properly disposed/cleaned up
- [ ] Large objects are handled efficiently
- [ ] Caching strategies are appropriate
- [ ] Memory usage patterns are optimized

### 4. Network & I/O Operations
- [ ] API calls are batched when possible
- [ ] Appropriate timeout values are set
- [ ] Connection pooling is used
- [ ] File I/O is optimized
- [ ] Asynchronous operations are used where appropriate

### 5. Caching Strategies
- [ ] Appropriate caching mechanisms are implemented
- [ ] Cache invalidation strategies are in place
- [ ] Cache keys are well-designed
- [ ] Memory vs. persistent cache trade-offs are considered
- [ ] Cache hit/miss ratios are optimized

### 6. Frontend Performance
- [ ] Bundle sizes are minimized
- [ ] Lazy loading is implemented where appropriate
- [ ] Image optimization is in place
- [ ] Critical rendering path is optimized
- [ ] JavaScript execution is efficient

### 7. Scalability Considerations
- [ ] Code can handle increased load
- [ ] Resource usage scales linearly
- [ ] Bottlenecks are identified and addressed
- [ ] Horizontal scaling is considered
- [ ] Performance monitoring is in place

## Performance Metrics & Benchmarks

### Time Complexity Guidelines
- **O(1)** - Constant time ✅
- **O(log n)** - Logarithmic time ✅
- **O(n)** - Linear time ⚠️ (acceptable for most cases)
- **O(n log n)** - Linearithmic time ⚠️ (sorting algorithms)
- **O(n²)** - Quadratic time 🔴 (avoid when possible)
- **O(2ⁿ)** - Exponential time 🔴 (avoid)

### Database Query Guidelines
- **< 100ms** - Excellent ✅
- **100-500ms** - Good ⚠️
- **500ms-1s** - Acceptable for complex queries ⚠️
- **> 1s** - Needs optimization 🔴

### Memory Usage Guidelines
- Monitor heap usage and garbage collection
- Avoid memory leaks in long-running processes
- Use appropriate data structures for memory efficiency
- Consider memory vs. CPU trade-offs

## Performance Anti-Patterns to Avoid

### Database Anti-Patterns
```sql
-- ❌ Bad: N+1 Query Problem
SELECT * FROM orders;
-- Then for each order:
SELECT * FROM customers WHERE id = ?;

-- ✅ Good: Join Query
SELECT o.*, c.* 
FROM orders o 
JOIN customers c ON o.customer_id = c.id;
```

### Algorithm Anti-Patterns
```javascript
// ❌ Bad: Nested loops for search
function findCommon(arr1, arr2) {
    const common = [];
    for (let i = 0; i < arr1.length; i++) {
        for (let j = 0; j < arr2.length; j++) {
            if (arr1[i] === arr2[j]) {
                common.push(arr1[i]);
            }
        }
    }
    return common;
}

// ✅ Good: Using Set for O(1) lookups
function findCommon(arr1, arr2) {
    const set = new Set(arr2);
    return arr1.filter(item => set.has(item));
}
```

### Memory Anti-Patterns
```python
# ❌ Bad: Loading all data into memory
def process_large_file(filename):
    with open(filename, 'r') as f:
        data = f.read()  # Loads entire file
    return process_data(data)

# ✅ Good: Streaming/chunked processing
def process_large_file(filename):
    with open(filename, 'r') as f:
        for line in f:  # Process line by line
            yield process_line(line)
```

## Performance Review Severity Guidelines

### Critical 🔴
- Exponential time complexity
- Memory leaks in production code
- Blocking operations in critical paths
- Infinite loops or recursion
- Resource exhaustion vulnerabilities

### High 🟡
- Quadratic time complexity in frequently called code
- Unoptimized database queries
- Large objects loaded unnecessarily
- Missing indexes on frequent queries
- Inefficient API usage patterns

### Medium 🟠
- Suboptimal algorithm choices
- Missing caching opportunities
- Inefficient data structure usage
- Unnecessary object creation
- Non-critical performance improvements

### Low 🟢
- Minor optimization opportunities
- Code style that could be more efficient
- Potential future performance considerations
- Documentation for performance-critical sections

## Performance Testing Recommendations

### Load Testing
```yaml
# Example load testing configuration
- name: Performance Test
  run: |
    # Use tools like k6, JMeter, or Artillery
    k6 run --vus 100 --duration 30s performance-test.js
```

### Profiling
```bash
# Example profiling commands
node --prof app.js                    # Node.js profiling
python -m cProfile -o profile.prof    # Python profiling
go tool pprof cpu.prof                # Go profiling
```

### Monitoring
- Set up application performance monitoring (APM)
- Track key metrics (response time, throughput, error rate)
- Monitor resource usage (CPU, memory, disk I/O)
- Set up alerting for performance degradation

## Performance Review Comment Template

```markdown
**Performance Issue**: [Brief description of the performance concern]

**Impact**: [How this affects application performance]

**Current Complexity**: [Time/space complexity if applicable]

**Recommendation**: [Specific optimization suggestion]

**Improved Implementation**:
```[language]
[Optimized code example]
```

**Expected Improvement**: [Quantified improvement if possible]

**Benchmark**: [Performance metrics if available]

**Severity**: [Critical/High/Medium/Low]
```

## Example Usage

```yaml
- name: Run Performance Review
  uses: your-username/claude-code-review@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    review_type: 'performance'
    progress_tracking: true
    severity_labels: true
    max_review_comments: 40
    custom_prompt: |
      Focus on algorithmic efficiency, database query optimization, 
      and memory usage patterns. Provide specific performance metrics 
      and benchmarks where possible.
```

## Tools & Resources

### Profiling Tools
- **JavaScript**: Chrome DevTools, Node.js built-in profiler
- **Python**: cProfile, py-spy, memory_profiler
- **Java**: JProfiler, VisualVM, JVM built-in tools
- **Go**: pprof, go tool trace
- **Database**: EXPLAIN plans, query analyzers

### Performance Monitoring
- Application Performance Monitoring (APM) tools
- Database query analyzers
- Memory profilers
- Load testing frameworks