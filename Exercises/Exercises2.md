# GitHub Copilot Code Review Exercises

## Main Overview
This comprehensive set of exercises guides you through mastering code review workflows using GitHub Copilot Chat. Work through them sequentially to build skills from foundational review concepts to advanced team collaboration techniques.

### Exercise Goals
- Conduct contextual code reviews using Copilot Chat
- Build a custom review agent mode tuned to team workflows and standards
- Integrate repository context (README, style guide, PR templates) into review sessions
- Use Copilot Tools for intelligent search, documentation lookup, and inline review support
- Apply conversational feedback loops to improve review depth and developer engagement

---

## **Prerequisites - Setup for Code Review Session**

**Overview**: Follow these quick setup steps to prepare for code review exercises.

### Part A: Verify Application Setup
1. Check if the application is already running:
   - Frontend: http://localhost:4321
   - Backend: http://localhost:5100/api/dogs
2. If not running, start the application via terminal:
   - PowerShell: `.\scripts\start-app.ps1`
   - Bash: `./scripts/start-app.sh`
3. Verify both frontend and backend are working

### Part B: Create Organizational Standards
**(ONLY FOR THOSE WHO DID NOT ATTEND Session 1)**
1. Create `.github/copilot-instructions.md` if it doesn't exist inside the .github directory. Copy the content from copilot-instructions.md into the new file.
2. Save the file

### Part C: Create a Feature Branch with Sample Code 
**(ONLY FOR THOSE WHO DID NOT ATTEND Session 1)**
1. Create a new branch in the terminal: `git switch -c feature/dog-age-api`
2. Add a sample route to `server/app.py` (add before `if __name__ == '__main__':`):
   ```python
   @app.route('/api/dogs/<int:dog_id>/human-age', methods=['GET'])
   def get_dog_human_age(dog_id):
       dog = Dog.query.get(dog_id)
       if not dog:
           return jsonify({"error": "Dog not found"}), 404
       
       # Calculate human age: First 2 years = 10.5 each, then 4 years per year
       if dog.age <= 2:
           human_age = dog.age * 10.5
       else:
           human_age = 21 + ((dog.age - 2) * 4)
       
       return jsonify({
           "dog_name": dog.name,
           "dog_age": dog.age,
           "human_age": int(human_age)
       })
   ```
3. Save the file
4. Test the endpoint: http://localhost:5100/api/dogs/1/human-age
5. **Note**: This code intentionally has some review points (missing docstring, no type hints, etc.)

---

## **Exercise 1: Foundation - Understanding Code Review Context**

**Overview**: Before conducting reviews, understand how Copilot leverages context to provide meaningful feedback on code changes.

### Part A: Basic Code Review with Copilot Chat
1. Ensure you're on the `feature/dog-age-api` branch
2. Make a small change to `server/app.py`:
   - Add a new route that lacks error handling or proper validation (add before `if __name__ == '__main__':`)
   ```python
   @app.route('/api/dogs/<int:dog_id>/breed', methods=['GET'])
   def get_dog_breed(dog_id):
       dog = Dog.query.get(dog_id)
       return jsonify({"breed": dog.breed.name})
   ```
   - Save the file.
3. Select/highlight the new route. Open Copilot Chat in Ask and prompt: `Review #selection for potential issues`.  
4. **Reflection**: Note what issues Copilot identifies (likely: no null checks, missing error handling)
5. Ask follow-up: `What improvements would you suggest?`
6. Compare the quality of review between free models (GPT-5 mini) and premium models (Claude Sonnet 4.5)

### Part B: Leveraging Repository Standards in Reviews
1. Ensre you have `.github/copilot-instructions.md` from Exercise1.md.
2. Highlight the same code from Part A (get_dog_breed)
3. Create a New Chat and prompt: `Review #selection against the project standards in #file:.github/copilot-instructions.md`
4. **Reflection**: 
   - Does Copilot now check for type hints, docstrings, and error handling?
   - How does explicit reference to standards improve review quality?
5. Ask: `Does this code follow PEP 8 and our documentation standards?`

### Part C: Multi-File Context Reviews
1. Open both `server/app.py` and `server/models/dog.py`
2. Highlight a route such get_dogs() in `app.py` that uses the Dog model
3. Prompt in Ask: `Review #selection considering the model definition in #file:server/models/dog.py`
4. **Reflection**: 
    - How does referencing the model improve Copilot's ability to spot issues?
    - Notice that #selection only works if the file is the active tab and the selection is in the same file.

### Part D: Workspace Code Reviews
#### Full Codebase Standards Compliance
1. Close all tabs and open a New Chat
2. Prompt in Ask:
   ```
   @workspace Review the entire codebase against #file:.github/copilot-instructions.md and provide a compliance report with:
   - Files that follow standards
   - Files with violations
   - Most common violations
   ```
3. **Reflection**: Which standards are most frequently violated?

#### Cross-File Consistency Analysis
1. Prompt in Ask:
   ```
   @workspace Analyze all Python files in the server directory for:
   - Consistent error handling patterns
   - Uniform response formats
   - Type hint coverage
   - Docstring completeness
   ```
2. Review the consistency report
3. Ask follow-up: `Which inconsistencies pose the highest risk?`

#### Test Coverage Gap Analysis
1. Prompt in Ask:
   ```
   @workspace Compare all routes in #file:server/app.py with tests in #file:server/test_app.py and identify:
   - Routes without tests
   - Routes with partial test coverage
   - Edge cases not covered
   ```
2. Generate missing tests: `Generate unit tests for the untested routes following the AAA pattern`

#### Documentation Sync Check
1. Prompt in Ask:
   ```
   @workspace Audit documentation completeness:
   - Compare #file:server/app.py endpoints with #file:API.md
   - Check if README.md reflects current setup steps
   - Identify functions missing Google-style docstrings
   ```
2. Ask: `Generate documentation updates for all gaps found`

#### Generate Workspace Health Report
1. Prompt in Ask:
   ```
   @workspace Generate a comprehensive codebase health report including:
   
   ## Summary
   - Total files analyzed
   - Overall compliance score
   
   ## Critical Issues
   - Security vulnerabilities
   - Missing error handling
   
   ## Code Quality
   - Standards compliance by file
   - Type hint coverage percentage
   
   ## Testing
   - Test coverage gaps
   - Missing edge case tests
   
   ## Documentation
   - Missing/outdated documentation
   
   ## Recommendations
   - Prioritized action items
   ```
2. Save report to: `workspace-review-report.md`

### Part E: Performing Code Reviews on Uncommited Changes.
1. Select Source Control in the Left Navigation Bar (Ctrl+Shift+G).
2. Select Code Review - Uncommited Changes.
3. Explore the different issues identified by Copilot.
4. Do not accept any suggested changes yet.

---

## **Exercise 2: Creating Custom Review Standards**

**Overview**: Build a dedicated review checklist that Copilot can use to conduct systematic code reviews aligned with team practices.

### Part A: Create a Code Review Checklist
1. Create a new file: `code-review-checklist.md`
2. Add content from code-review-checklist.md.
3. Save the file

### Part B: Test the Review Checklist
1. Open `server/app.py` and add a problematic route (before if __name__ == '__main__':):
   ```python
   @app.route('/api/search', methods=['GET'])
   def search_dogs():
       query = request.args.get('q')
       dogs = Dog.query.filter(Dog.name.like(f'%{query}%')).all()
       return jsonify([{"id": d.id, "name": d.name} for d in dogs])
   ```
2. Highlight the code
3. Prompt in Ask: `Review #selection using the checklist in #file:code-review-checklist.md`
4. **Expected Issues** (results may vary):
   - No input validation (query could be None)
   - No error handling
   - Missing docstring
   - Missing type hints
   - No tests mentioned
   - Potential SQL injection via like query
5. Ask: `Generate an improved version that passes all checklist items`

### Part C: Review Existing Features Against Checklist
1. Close all tabs and open New Chat
2. Prompt in Ask: `@workspace Review all routes in #file:server/app.py against #file:code-review-checklist.md and create a summary report`
3. Review the generated report
4. Ask follow-up: `Which routes have the most critical issues?`
5. **Goal**: Identify systematic problems across the codebase

---

## **Exercise 3: Conducting Pull Request Reviews**

**Overview**: Simulate a PR review workflow using Copilot to provide comprehensive feedback on code changes.

### Part A: Review a Feature Branch
1. Ensure you're on `feature/dog-age-api` branch with changes
2. Get a diff of your changes: `git diff main...feature/dog-age-api > pr-changes.diff`
3. Open the diff file in editor
4. Prompt in Ask: `Review the changes in #file:pr-changes.diff for potential issues, considering #file:.github/copilot-instructions.md and #file:code-review-checklist.md`
5. **Reflection**: How comprehensive is the review?
6. Ask follow-up: `What are the top 3 improvements needed before merging?`

### Part B: Create a PR Review Template
1. Create file: `pull-request-template.md` and place it in the root directory.
2. Add content from this url: https://docs.github.com/en/copilot/tutorials/customization-library/custom-instructions/pull-request-assistant
3. Save the file

### Part C: AI-Assisted PR Description Generation
1. Stage your changes: `git add .`
2. Create a commit message following the steps in **Exercises1.md - Exercise 9: Commit & Collaboration**
3. Commit the message. Push the branch via the terminal: `git push -u origin feature/dog-age-api`
4. Select the GitHub Tab and Click Create a New Pull Request in VS Code. (Must have GitHub Pull Requests extension installed)
5. Select the Generate Description option and review the generated description.
6. Do not create the PR yet.

### Part D: Standardize PR Description using Pull Request Template
1. Generate a diff of all changes between the main branch and the feature/dog-age-api branch and save the output to a file named pr-changes.diff:
- Run in terminal: `git diff main...feature/dog-age-api > pr-changes.diff`
2. Prompt in Ask: `Generate a pull request description using this template #file:pull-request-template.md based on changes in #file:pr-changes.diff`
3. Review the generated PR description
4. **Reflection**: `Is this PR description clearer for reviewers vs Part C?`
5. **Goal**: Generate comprehensive PR descriptions in under 2 minutes
---

## **Exercise 4: Interactive Review Sessions with Colleagues**

**Overview**: Use conversational feedback loops to deepen review insights and catch subtle issues.

### Part A: Progressive Review Depth
1. Create a New Chat. Highlight a get_dog function in `server/app.py` 
2. Start with broad review: `Review #selection for issues`
3. Ask follow-up questions:
   - `Are there any security concerns?`
   - `How is the error handling?`
   - `Could this cause performance problems?`
   - `Is this testable?`
4. **Reflection**: How does asking focused questions reveal deeper issues?

### Part B: Conversational Code Improvement
1. Create a New Chat. Take a function that Copilot flagged with issues (get_dog_breed). Close all tabs and open New Chat. Select the function.
2. Have a conversation to improve it:
   - You: `#selection How can I improve the error handling here?`
   - Review Copilot's suggestion
   - You: `What about input validation?`
   - Review next suggestion
   - You: `Generate the complete improved version`
3. Compare original vs. final version
4. **Goal**: Understand how iterative questioning produces better results

### Part C: Team Standards Clarification
1. Create a New Chat. Prompt: `@workspace In #file:.github/copilot-instructions.md, we require Google-style docstrings. Show me an example for a Flask route that queries the database`
2. Review the example
3. Open app.py. Select get_dog_breed route, prompt: `Add a Google-style docstring to #selection`
4. Ask clarifying questions if needed: `Should the docstring include database query details?`
5. **Goal**: Use Copilot as a style guide interpreter

---

## **Exercise 5: Integration of Repository Documentation**

**Overview**: Leverage README, API docs, and other repository context to conduct architecture-aware reviews.

### Part A: Architecture Consistency Reviews
1. Open `API.md` in tabs.
   Note: If you didn't perform Exercise 1, copy the content from API.md into a new file named API.md and save it in the root directory.
2. Highlight a new API route (get_dog_breed) in `server/app.py`
3. Prompt: `Review #selection for consistency with the architecture described in #file:API.md`
4. **Reflection**: Does Copilot catch architectural deviations?
5. Ask: `Does this endpoint follow the same patterns as existing APIs?`

### Part B: Documentation Completeness Check
1. Add a new route to `server/app.py` without updating `API.md`
`python`
@app.route('/api/dogs/<int:id>/age', methods=['GET'])
def get_dog_age(id: int):
    dog = Dog.query.get(id)
    if not dog:
        return jsonify({"error": "Dog not found"}), 404
    return jsonify({"age": dog.age})

2. Close all tabs and create New Chat
3. Prompt: `@workspace Compare routes in #file:server/app.py with documented endpoints in #file:API.md. What's missing?`
4. Copilot should identify undocumented routes
5. Ask: `Generate API documentation for the missing endpoints`

### Part C: Cross-Reference Testing Documentation
1. Open `server/test_app.py`
2. Prompt: `@workspace Which routes in #file:server/app.py lack corresponding tests in #file:test_app.py?`
3. Review the gap analysis
4. For each missing test:
   - Highlight the route (example highlight get_dog_breed)
   - Prompt: `/tests #selection following patterns in #file:test_app.py`
   - Follow up prompt: `Add additional edge cases, boundary conditions, constraints, and error scenarios`
   - Compare how generated tests increase coverage and robustness
5. **Goal**: Ensure test coverage matches implementation
6. **Reflection**: How does follow up questioning improve test completeness?

---

## **Exercise 6: Using Copilot Tools for Enhanced Reviews**

**Overview**: Leverage Copilot's search, lookup, and analysis tools to perform deeper code reviews.

### Part A: Intelligent Code Search During Review
1. Open app.py. Select get_dogs function, notice it uses a pattern
2. Create a New Chat. Prompt: `@workspace Find all functions that use similar database query patterns to #selection`
3. Review results - are patterns consistent?
4. Ask: `Are there any inconsistencies in how we handle database queries across these functions?`

### Part B: Documentation Lookup
1. Reviewing code that uses Flask decorators
2. Create a New Chat. Ask: `What are the best practices for Flask route decorators according to the Flask documentation?`
3. Compare your code against best practices. Open app.py.
4. Ask: `Do the routes in #file:app.py follow these best practices?`

### Part C: Code Reviewer Assistant for Deep Code Analysis
1. Create a code-reviewer agent.
   - Select Cog -> Custom Agents -> + Create new custom agent -> .github\agents
   - Name it: code-reviewer
   - Copy and paste the content from code-reviewer.agent.md into the new agent.md file.
2. . Open Copilot Chat in code-reviewer mode
3. Prompt: `@workspace Analyze all routes in #file:app.py and check for issues against guidelines specified in #file:code-review-checklist.md Report any issues with specific examples.`
4. **Expected behavior**: Agent will:
   - Autonomously search through the file
   - Identify issues based on the checklist
   - Compare them for consistency against the checklist
   - Generate a structured report
5. Review the agent's findings
6. Ask follow-up: `Which route should be addressed first and why?`
7. **Advanced task**: Prompt: `@workspace Research all authentication and authorization patterns across the entire codebase. Are there any security gaps?`
8. Agent autonomously searches across multiple files and reports findings
9. Open app.py and select get_dog_breed route
10. Prompt: `Make a detailed review of #selection and report any findings based on the guidelines in #file:code-review-checklist.md`
11. Create a prompt file: code-review-selection.prompt.md
   - Select Cog -> Custom Prompt -> + New prompt file -> .github\agents
   - Name it: code-reviewer-selection
   - Copy and paste the content from code-reviewer-selection.prompt.md into the new prompt file.
12. Open Copilot Chat in code-reviewer mode
13. Open app.py and select get_dog_breed route
14. Prompt: `/code-review-selection #file:selection `
15. **Reflection**: 
   - How does agent research differ from direct chat queries?
   - Note the breadth and depth of analysis for the detailed review of the selected route
   - How does the prompt file simplify repeated review tasks?

### Part D: Agent-Assisted Pattern Analysis
1. Use the code-reviewer for cross-file pattern detection
2. Prompt: `@workspace Find all database query patterns in the server directory and identify which ones could cause N+1 query problems`
3. Agent searches across models and routes
4. Review the comprehensive analysis
5. Prompt: `Generate a refactoring plan to fix the N+1 issues you found`
6. **Goal**: Leverage agent autonomy for complex, multi-file analysis tasks

---

## **Exercise 7: Building Custom Review Modes**

**Overview**: Create specialized review agents for different types of considerations.

### Part A: Security-Focused Review Mode
1. Create a new agent: `security-reviewer`. Use the information from security-reviewer.agent.md.
2. Create a prompt file: security-reviewer-selection.prompt.md. Use the content from security-review-selection.prompt.md.
3. Create a security_rules.md file in the root directory and add the content from security_rules.md.
4. Open app.py, highlight get_dog_breed route and test with the route: `/security-review-selection #file:selection`

### Part B: Performance Review Mode
1. Create a new agent: `performance-reviewer`. Use the information from performance-reviewer.agent.md.
2. Create a prompt file: performance-reviewer-selection.prompt.md. Use the content from performance-review-selection.prompt.md.
3. Create a performance_rules.md file in the root directory and add the content from performance_rules.md.
4. Open app.py, highlight get_dog_breed route and test with the route: `/performance-review-selection #file:selection`

### Part C: Accessibility Review Mode
1. Create a new agent: `accessibility-reviewer`. Use the information from accessibility-reviewer.agent.md.
2. Create a prompt file: accessibility-reviewer-file.prompt.md. Use the content from accessibility-review-file.prompt.md.
3. Create a accessibility_rules.md file in the root directory and add the content from accessibility_rules.md.
4. Open client/src/components/DogList.svelte. Prompt using the new agent: `/accessibility-review-file #file:DogList.svelte`

**Reflection**:
   - How does tailoring the agent to specific review types improve focus?
   - What unique insights does each specialized review mode provide?
   - How can these modes be integrated into a comprehensive review workflow?

---

## **Exercise 8: Automated Review Checklists**

**Overview**: Create workflows where Copilot systematically checks code against multiple dimensions.

### Part A: Comprehensive Multi-Checklist Review
1. Create a function that you intentionally make problematic
2. Run it through multiple checklists:
   ```
   Review #selection against these guides:
   - #file:.github/copilot-instructions.md
   - #file:.github/code-review-checklist.md
   - #file:.github/security-review-guide.md
   - #file:.github/performance-review-guide.md
   ```
3. Collect all issues found
4. Ask: `Prioritize these issues by severity`

### Part B: Create a Master Review Command
1. Create file: `.github/master-review-process.md`
2. Add content:
````markdown
# Master Review Process

When reviewing code, check in this order:

1. **Functionality**: Does it work? Does it meet requirements?
2. **Security**: Run through security-review-guide.md
3. **Code Quality**: Check copilot-instructions.md compliance
4. **Testing**: Verify code-review-checklist.md testing section
5. **Performance**: Review using performance-review-guide.md
6. **Documentation**: Check docstrings and API docs
7. **Accessibility** (if frontend): Use accessibility-review-guide.md

Generate a structured report with:
- Summary of findings
- Critical issues (must fix)
- Suggestions (nice to have)
- Positive observations
````

3. Test it: `Review #selection following the process in #file:.github/master-review-process.md`

### Part C: Review Report Generation
1. After running master review, prompt: `Generate a code review report in markdown format with sections for each review area`
2. Review the generated report
3. Save to file: `code-review-report.md`
4. **Goal**: Generate consistent, thorough review reports

---

## **Exercise 9: Team Collaboration & Review Feedback**

**Overview**: Use Copilot to facilitate better communication during code review discussions.

### Part A: Generating Constructive Feedback
1. Find a code issue
2. Instead of just identifying it, prompt: `Review #selection and provide constructive feedback that explains the issue, why it matters, and how to fix it`
3. Compare:
   - Direct: "Missing error handling"
   - Constructive: "This route lacks error handling, which could cause the API to return 500 errors when the dog doesn't exist. Consider adding a try-except block or checking if the result is None before accessing attributes."
4. **Goal**: Generate reviewer feedback that educates developers

### Part B: Explaining Review Comments
1. Simulate receiving review feedback: Create a comment like "This has an N+1 query problem"
2. Prompt: `Explain what an N+1 query problem is and how to fix it in #selection`
3. Use Copilot as a learning tool during reviews
4. Ask: `Show me an example of how to fix this specific case`

### Part C: Review Discussion Threads
1. Start a simulated review discussion:
   - You: `Why is error handling important in this route?`
   - Copilot provides explanation
   - You: `What's the best way to handle database errors here?`
   - Copilot provides options
   - You: `Which approach is most consistent with our codebase?`
2. **Reflection**: How does conversational review improve understanding?

---

## **Exercise 10: End-to-End Review Workflow**

**Overview**: Conduct a complete code review from initial scan to final approval using all learned techniques.

### Scenario: Review a Complete Feature
You're reviewing the dog age calculator feature from Exercises1.md

### Part A: Initial Scan
1. Get the diff: `git diff main...feature/dog-age-api > feature-review.diff`
2. Broad review: `@workspace Provide an initial assessment of changes in #file:feature-review.diff`
3. Note the high-level feedback
4. Ask: `What are the major areas that need deeper review?`

### Part B: Systematic Deep Review
For each changed file:

1. **Backend Code Review** (`server/app.py`):
   ```
   Review changes to #file:server/app.py against:
   - #file:.github/copilot-instructions.md
   - #file:.github/code-review-checklist.md
   - #file:.github/security-review-guide.md
   - #file:.github/performance-review-guide.md
   
   Provide findings in priority order.
   ```

2. **Test Review** (`server/test_app.py`):
   ```
   Review #file:server/test_app.py for:
   - Coverage of new functionality
   - Edge case testing
   - Proper mocking
   - AAA pattern compliance
   ```

3. **Documentation Review**:
   ```
   @workspace Check if #file:API.md is updated to reflect new endpoints in #file:server/app.py
   ```

### Part C: Generate Review Summary
1. Prompt:
   ```
   Generate a comprehensive PR review summary that includes:
   - Overview of changes
   - Critical issues found (must fix before merge)
   - Minor suggestions (nice to have)
   - Questions for the author
   - Positive feedback
   - Final recommendation (Approve/Request Changes/Needs Discussion)
   ```
2. Review the generated summary
3. Save to: `pr-review-summary.md`

### Part D: Track Review Iterations
1. Assume author made fixes, prompt:
   ```
   Compare the original issues identified with current code in #file:server/app.py
   Have all critical issues been addressed?
   ```
2. Generate follow-up comments if needed
3. Final approval: `Generate an approval message summarizing improvements made`

---

## **Exercise 11: Advanced Review Scenarios** *(Bonus)*

### Part A: Reviewing Refactoring PRs
1. Create a refactoring change (extract a helper function)
2. Review with: `Review #selection to verify refactoring maintains functionality and improves code quality`
3. Ask: `Are there any behavior changes introduced by this refactoring?`
4. Verify tests still pass

### Part B: Breaking Change Reviews
1. Simulate a breaking API change
2. Prompt: `@workspace Identify all places that would be affected by changing the endpoint /api/dogs to /api/v2/dogs`
3. Review impact analysis
4. Ask: `What migration strategy would you recommend?`

### Part C: Large PR Review Strategy
1. For PRs with many files, prompt:
   ```
   @workspace Analyze the changes across all files and:
   1. Group related changes
   2. Identify the logical sequence to review
   3. Highlight areas of highest risk
   4. Suggest if this should be split into smaller PRs
   ```
2. Follow the suggested review order

---

## **Assessment Checklist**

After completing all exercises, you should be able to:

- [ ] Conduct systematic code reviews using Copilot Chat
- [ ] Create and maintain custom review checklists and guides
- [ ] Integrate project documentation into review context
- [ ] Use conversational feedback loops for deeper review insights
- [ ] Generate constructive, educational review feedback
- [ ] Review code against multiple dimensions (security, performance, quality)
- [ ] Create comprehensive review reports and summaries
- [ ] Leverage repository context (README, standards, templates) in reviews
- [ ] Use inline and chat-based review workflows appropriately
- [ ] Facilitate team learning through review discussions
- [ ] Track review iterations and verify fixes
- [ ] Handle complex review scenarios (refactors, breaking changes, large PRs)

---

## **Best Practices Learned**

### Review Workflow
1. **Start Broad, Then Focus**: Initial scan → identify areas → deep dive
2. **Use Multiple Perspectives**: Security, performance, quality, testing
3. **Be Constructive**: Explain why, not just what's wrong
4. **Leverage Context**: Always reference project standards and patterns
5. **Iterate**: Use follow-up questions to deepen insights

### Context Management for Reviews
- Open relevant files before starting review
- Reference standards documents explicitly
- Use `@workspace` for cross-file impact analysis
- Keep review guides in `.github/` for easy reference

### Team Collaboration
- Generate review reports for consistency
- Use Copilot to explain complex feedback
- Create reusable review templates
- Document common review patterns

---

## **Next Steps**

After completing these exercises, consider:

1. **Integrate into Team Workflow**: Adopt these review practices in your team
2. **Customize Review Guides**: Tailor checklists to your tech stack and standards
3. **Measure Review Quality**: Track how Copilot-assisted reviews catch more issues
4. **Train Team Members**: Share these techniques with fellow reviewers
5. **Automate Where Possible**: Consider CI/CD integration for automated checks
6. **Continuous Improvement**: Update review guides based on lessons learned
7. **Create Domain-Specific Reviews**: Build specialized reviews for your industry (e.g., HIPAA compliance, financial regulations)

---

## **Appendix: Quick Reference Commands**

### Basic Review Commands
- `Review #selection for issues`
- `Review #selection against #file:.github/copilot-instructions.md`
- `/explain #selection`

### Comprehensive Review
- `Review #selection using checklist in #file:.github/code-review-checklist.md`
- `@workspace Review all changes in #file:pr-changes.diff`

### Specialized Reviews
- `Review #selection for security issues using #file:.github/security-review-guide.md`
- `Review #selection for performance issues`
- `Review #selection for accessibility issues`

### Documentation & Reporting
- `Generate a code review report for #selection`
- `@workspace Check if #file:API.md reflects changes in #file:server/app.py`
- `Generate constructive feedback for #selection`

### Follow-up Questions
- `What are the top 3 improvements needed?`
- `Are there any security concerns?`
- `Is this code testable?`
- `Does this follow our project standards?`
