# Code Review Checklist

## Security
- [ ] No hardcoded secrets or API keys
- [ ] Input validation on all user-provided data
- [ ] SQL injection protection (use ORM, no raw SQL)
- [ ] Authentication/authorization checks where needed

## Code Quality
- [ ] Follows organizational standards from copilot-instructions.md
- [ ] No code duplication (DRY principle)
- [ ] Functions are single-purpose (max 50 lines)
- [ ] Descriptive variable and function names
- [ ] Proper error handling with meaningful messages

## Testing
- [ ] Unit tests for new functionality (AAA pattern)
- [ ] Edge cases and error conditions covered
- [ ] Tests use mocking for external dependencies
- [ ] Test coverage meets 80% threshold

## Documentation
- [ ] All functions have Google-style docstrings
- [ ] Complex logic has inline comments
- [ ] API changes reflected in API.md
- [ ] README updated if setup steps change

## Performance
- [ ] No N+1 query problems
- [ ] Database queries are efficient
- [ ] Appropriate use of pagination for large datasets
- [ ] No unnecessary loops or redundant operations

## Frontend (if applicable)
- [ ] Error handling for all API calls
- [ ] Loading states for async operations
- [ ] Accessibility: aria-labels on interactive elements
- [ ] TypeScript types properly defined
