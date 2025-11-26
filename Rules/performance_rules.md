# Performance Rules – Focused Performance Reviewer
These rules expand the logic used by `agent.md`. They serve as the authoritative performance checklist and optimization guide.

---

# 1. Database Performance
- Flag queries without pagination or limits.
- Detect N+1 query patterns in ORM usage.
- Identify SELECT * usage; recommend column selection.
- Highlight missing indexes on frequently filtered columns.
- Warn on transactions performing network calls.

**Dangerous Patterns**
```python
# N+1 query example
for user in users:
    posts = db.session.query(Post).filter(Post.user_id == user.id).all()
```

**Safe**
```python
# Use join or eager loading
users = db.session.query(User).options(joinedload(User.posts)).all()
```

---

# 2. Backend Performance (Python/Flask & Node/Express)
- Flag CPU-heavy operations in request handlers.
- Detect blocking I/O in async contexts.
- Identify repeated expensive computations inside loops.
- Warn on missing connection pooling for DB or external services.

**Dangerous Patterns**
```python
# Blocking call in async context
async def handler():
    data = requests.get(url)  # blocks event loop
```

**Safe**
```python
# Use aiohttp for async HTTP
async with aiohttp.ClientSession() as session:
    async with session.get(url) as resp:
        data = await resp.text()
```

---

# 3. API Performance
- Flag endpoints returning unbounded lists.
- Detect missing compression for large JSON payloads.
- Warn on lack of conditional requests (ETags).

**Dangerous Patterns**
```python
@app.route('/users')
def get_users():
    return jsonify(User.query.all())  # No pagination
```

**Safe**
```python
@app.route('/users')
def get_users():
    page = request.args.get('page', 1, type=int)
    users = User.query.paginate(page, per_page=50)
    return jsonify(users.items)
```

---

# 4. Caching
- Flag missing caching for expensive queries.
- Detect redundant recomputation of static data.
- Warn on improper cache key design.

**Dangerous Patterns**
```python
# Recomputing heavy stats every request
stats = compute_heavy_stats()
```

**Safe**
```python
# Cache results with TTL
stats = cache.get('heavy_stats')
if not stats:
    stats = compute_heavy_stats()
    cache.set('heavy_stats', stats, timeout=600)
```

---

# 5. Frontend Performance (React)
- Detect unnecessary re-renders due to unstable props.
- Flag large bundle sizes (>200KB gzipped).
- Warn on missing memoization for expensive computations.

**Dangerous Patterns**
```jsx
function List({ items }) {
  return items.map(item => <Item key={item.id} data={item} />);
}
```

**Safe**
```jsx
const MemoizedItem = React.memo(Item);
function List({ items }) {
  return items.map(item => <MemoizedItem key={item.id} data={item} />);
}
```

---

# 6. Infrastructure & Concurrency
- Flag missing timeouts on external calls.
- Detect lack of autoscaling or health checks.
- Warn on overcommitted worker threads.

---

# 7. Severity Rules
- **Critical**: N+1 queries, blocking I/O in async, unbounded endpoints.
- **High**: Missing caching, large payloads, CPU-heavy loops.
- **Medium**: Missing memoization, inefficient joins.
- **Low**: Minor optimizations, missing compression.

---

# 8. Remediation Requirements
Every fix must:
- Be **specific and actionable**.
- Use **optimized defaults**.
- Include **example patched code**.
- Include recommended **profiling or tests** to prevent regressions.

---
