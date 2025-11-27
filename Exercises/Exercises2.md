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

### Part E: Performing Code Reviews on Uncommited Changes and Inline Review.
1. Select Source Control in the Left Navigation Bar (Ctrl+Shift+G).
2. Select Code Review - Uncommited Changes (found in the Source Control panel).
3. Navigate through the Code Review comments using the arrows in the top right of the Code Review panel.
4. Do not accept any suggested changes yet.
5. Highlight the whole route that the Code Review is focused on.
6. Open a New Chat in Ask and prompt: `#selection Check for possible errors` 
7. **Reflection**: How does inline review compare to chat-based review?
Inline review focuses on the specific changes, while chat-based allows broader context.
8. Open app.py and highlight the contents of the whole file.
9. Right-click and select Generate Code -> Review.
10. Click the down arrow in the top right of the Code Review panel to navigate through the comments.
11. Compare the number of issues found in this approach vs the Code Review - Uncommited Changes approach.
12. Open a New Chat in Ask and prompt: `#file:server/app.py Check for possible errors`
13. **Reflection**: How does reviewing the whole file compare to the Chat Review approach?
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

### **Part D: Simulating Reviewer-Developer Dialogue**
**Scenario**: You're reviewing a colleague's code and need to provide constructive feedback that explains the reasoning, not just the issue.
1. **Setup**: Open `server/app.py` and locate the `get_dog_breed` route
2. **Act as Reviewer**: Create a New Chat in Ask and prompt:
   ```
   I'm reviewing this code #selection. As a senior developer reviewing a junior's code, 
   explain 3 issues you find and why they matter. Use an educational, mentoring tone.
   ```
3. **Review Copilot's feedback style** - it should explain *why*, not just *what*
4. **Act as Developer**: Now switch perspectives and ask:
   ```
   I'm the developer who wrote this code. I don't understand why [pick one issue from above] 
   is a problem. Can you explain it with a simple example?
   ```
5. **Observe**: How does Copilot adjust its explanation for understanding?
6. **Ask for alternatives**: 
   ```
   What are 2-3 different ways I could fix this issue? Compare their trade-offs.
   ```
7. **Reflection**: 
   - How does explaining reasoning improve learning vs. just stating issues?
   - How can you use this approach in real PR reviews?

---

### **Part E: Using Copilot to Mediate Implementation Disagreements**
**Scenario**: Two developers disagree on the best approach. Use Copilot as a neutral mediator to evaluate options.
1. **Setup the Disagreement**: Create a New Chat with this scenario:
   ```
   Two developers are debating how to handle database queries in our Flask app:
   
   Developer A wants: Direct SQLAlchemy queries in routes for simplicity
   Developer B wants: Repository pattern with service layer for testability
   
   Our codebase context:
   - Small team (3 developers)
   - App has 15 routes currently
   - Moderate complexity, growing
   - Standards in #file:.github/copilot-instructions.md
   
   Analyze both approaches considering our context. What are the pros/cons of each?
   ```
2. **Review the analysis** - Copilot should provide balanced evaluation
3. **Ask for recommendation**:
   ```
   Given our team size and app complexity, which approach would you recommend? 
   Explain your reasoning.
   ```
4. **Explore migration path**:
   ```
   If we chose Developer B's approach, show me a migration strategy that minimizes 
   disruption to ongoing work.
   ```
5. **Get concrete example**: Open `server/app.py`, select `get_dog_breed`, and prompt:
   ```
   Refactor #selection to follow Developer B's repository pattern approach. 
   Show me the complete implementation including the repository class.
   ```
6. **Reflection**:
   - How does having data-driven analysis help resolve disagreements?
   - What questions should you ask Copilot when evaluating competing approaches?

---

### **Part F: Generating Alternative Implementation Suggestions**

**Scenario**: A developer implemented a solution, but you want to explore if there are better alternatives.

1. **Review original implementation**: Open `server/app.py`, select the `get_dogs` route

2. **Request alternatives**: Create a New Chat and prompt:
   ```
   Review #selection and generate 3 alternative implementations with different 
   approaches (e.g., different patterns, libraries, or architectures).
   
   For each alternative:
   - Show the code
   - Explain when it's most appropriate
   - List pros and cons
   - Estimate complexity vs. the current approach
   ```
3. **Compare specific aspects**: Ask follow-up questions:
   ```
   Which alternative has:
   - Best performance for 1000+ records?
   - Easiest testing?
   - Best maintainability for a small team?
   ```
4. **Evaluate against project constraints**:
   ```
   Considering our project standards in #file:.github/copilot-instructions.md 
   and our current architecture in #file:API.md, which alternative fits best?
   ```
5. **Request hybrid approach**:
   ```
   Can you create a hybrid solution that combines the best aspects of alternatives 1 and 3?
   ```
6. **Reflection**:
   - How does exploring alternatives prevent "first solution" bias?
   - When should you invest time in generating alternatives vs. accepting the working solution?

---

### **Part G: Facilitating Team Code Review Meetings**
**Scenario**: Your team is conducting a synchronous code review meeting. Use Copilot to keep discussion focused and productive.
1. **Prepare meeting context**: Before the meeting, create a New Chat:
   ```
   @workspace Generate a code review agenda for the changes in feature/dog-age-api branch.
   
   Include:
   - Summary of changes
   - Key areas requiring team discussion
   - Potential concerns or questions
   - Estimated review time per section
   
   Reference: #file:pr-changes.diff
   ```

2. **During the meeting** - Answer questions in real-time:
   - Developer asks: "Why is input validation important here?"
   - Select get_dog_breed route in `server/app.py`
   - You prompt Copilot: `Explain why input validation is critical for #selection, with a concrete security example`
   - Share the explanation with the team

3. **Capture action items**:
   ```
   Based on our discussion, we identified these issues:
   1. Missing input validation on dog_id parameter
   2. No error handling for database connection failures
   3. Inconsistent response format vs. other endpoints
   
   Generate specific tasks with acceptance criteria for each issue.
   ```
4. **Generate follow-up documentation**:
   ```
   Create a review summary document that includes:
   - Issues discussed
   - Decisions made
   - Action items assigned
   - Timeline for fixes
   
   Format as a markdown file for our team wiki.
   ```

5. **Reflection**:
   - How can Copilot help keep review meetings focused and productive?
   - What types of questions are best answered in real-time during reviews?

---

## **Exercise 5: Integrating Repository Context for  Reviews**

**Overview**: Leverage README, API docs, and other repository files as context to conduct architecture-aware reviews.

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

### Part B: Documentation Lookup for Review Context
1. Reviewing code that uses Flask decorators
2. Create a New Chat. Ask: `What are the best practices for Flask route decorators according to the Flask documentation?`
3. **Copilot should provide:**
   - Summary of Flask decorator patterns
   - Common pitfalls
   - Performance considerations
4. Compare your code against best practices. Open app.py.
5. Ask: `Do the routes in #file:app.py follow these best practices?`
6. **Reflection**: How does on-demand documentation lookup improve review confidence?

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

## **Exercise 8: Multi-Agent Code Reviewer**
**Overview**: Combine multiple specialized review agents to conduct a holistic code review.
1. Create a new agent: `master-reviewer`. Use the information from master-reviewer.agent.md.
2. Open Copilot Chat in master-reviewer mode.
3. Prompt: `Create a review.`
4. The agent will autonomously use the different agents to generate an indvidual report and then combine them into a final report.
5. Review each report.
---

## **Exercise 9: Pull Requests with GitHub MCP Server**

**Overview**: Set up and utilize GitHub MCP Server to enhance PR reviews with Copilot Chat.
### Part A: Set Up GitHub MCP Server
1. Go to the Extensions view in VS Code (Ctrl+Shift+X)
2. Search for "@mcp GitHub" and install the extension
3. Check under MCP Servers - Installed Servers to verify GitHub MCP Server is listed
4. Click the Cog Icon and select Show Configuration (JSON).
5. Verify that the following configuration is present:
```json
{
	"servers": {
		"github": {
			"type": "http",
			"url": "https://api.githubcopilot.com/mcp/"
		}
	},
	"inputs": []
}
```
6. Press the Start Server button to start the MCP server.
7. Login with your GitHub credentials when prompted.
8. Verify the server status shows "Running".
9. Go to Agent mode in Copilot Chat, select configure tools, and verify that the GitHub MCP Server is listed under Available Tools and is checked.
10. Restart VS Code to ensure all settings take effect.
11. Make a sample Prompt: `Are there any PRs open in this repository?`
12. Verify that Copilot Chat returns a list of open PRs.

### Part B: Creating a Pull Request Using MCP Server
1. Ensure you're on the `feature/dog-age-api` branch with changes.
   - If not, switch to it: `git switch feature/dog-age-api`
2. Add files to staging: `git add .`
3. Run the following command in terminal to get the git diff of your changes:
   - `git diff --cached --stat > changes-summary.txt`
   - `git diff --cached > changes-detailed.txt`
4. Prompt in Ask: `Review the files #file:changes-summary.txt and #file:changes-detailed.txt to create a commit Title and Description that follows best practices in #file:.copilot-commit-message-instructions.md`
**Note**: If .copilot-commit-message-instructions.md does not exist, create it in the root directory and add the content from copilot-commit-message-instructions.md.
5. Go to Source Control in VS Code (Ctrl+Shift+G). Copy and paste the generated message in the Commit Message box.
6. Commit & Push the changes.
7. Run the following command in terminal to get the git diff of your changes:
   `git diff main...feature/dog-age-api > pr-changes.diff`
8. Prompt in Agent: `Generate a pull request title and description using the #file:pr-changes.diff and following the template in #file:pull-request-template.md`
9. Prompt in Agent: `Create a pull request using the generated title and description`
10. You maybe prompted to Allow MCP Server to create the PR on your behalf. Click Allow.
11. Verify the PR is created by navigating to the GitHub repository in your web browser.
12. Prompt in Agent: `List all the PRs in this repository and their current status`
13. Verify that the newly created PR is listed with the correct status (e.g., Open).
14. Prompt in Agent: `Provide a summary of the changes made in the PR titled "<your PR title>"`
15. **Reflection:** How does using MCP Server streamline the PR creation and review process?

---

## **Exercise 10: Apply Concepts** *(Bonus)*
**Overview**: You've mastered the basics - now tackle a real-world scenario that combines all code review skills into a complete workflow.

### Scenario: New Feature Implementation Review
A team member has implemented a new "Dog Adoption Status" feature. Your task is to conduct a comprehensive end-to-end code review using all techniques learned.

### Part A: Setup the Feature
1. Create a new branch: `git switch -c feature/adoption-status`
2. Add this route to `server/app.py` (before `if __name__ == '__main__':`):
   ```python
   @app.route('/api/dogs/<int:dog_id>/adoption', methods=['GET', 'POST'])
   def manage_adoption(dog_id):
       dog = Dog.query.get(dog_id)
       if request.method == 'POST':
           data = request.get_json()
           status = data['status']
           adopter = data.get('adopter_name')
           # Update adoption logic here
           return jsonify({"message": "Updated", "status": status})
       return jsonify({"id": dog.id, "name": dog.name, "adoption_status": "available"})
   ```
3. Save the file

### Part B: Multi-Dimensional Review (Exercises 1-2)
1. Ensure the new route is selected in `server/app.py`
2. Conduct a basic review: `Review #selection for potential issues`
3. Review against standards: `Review #selection against #file:.github/copilot-instructions.md`
4. Apply your checklist: `Review #selection using #file:code-review-checklist.md`
5. Document findings in a new file using Agent: `Place findings in a new file called adoption-feature-review.md in the root directory`

### Part C: Specialized Agent Reviews (Exercises 6-8)
1. Ensure the new route is selected in `server/app.py`
2. Switch to `security-reviewer` agent and run: `/security-review-selection`
3. Switch to `performance-reviewer` agent and run: `/performance-review-selection`
4. Use the `master-reviewer` agent for a comprehensive report
5. Compile all agent findings into your review document

### Part D: Interactive Improvement Session (Exercise 4)
1. Ensure the new route is selected in `server/app.py`
2. Using conversational review, progressively improve the code:
   - Start: `How can I improve error handling in #selection?`
   - Continue: `What about input validation for the POST data?`
   - Ask: `Are there any security concerns with the adopter_name field?`
   - Request: `Generate the complete improved version with all fixes`
3. Apply the improved code to `app.py`

### Part E: Generate Tests from Fixed Code
1. Generate comprehensive tests:
   ```
   Generate unit tests for the adoption endpoint including:
   - Happy path for GET and POST
   - Edge cases (missing dog, invalid status)
   - Security tests (SQL injection, XSS in adopter_name)
   ```
2. Step 3-9 are optional if you want to simulate creating your own agent and rules for testing.
3. Create a new agent: `test-generator`. Hint: Use the information from other agent.md files as a reference.
4. Create a test_rules.md file in the root directory and add content. Hint: Use the content from other rules.md files as a reference.
5. Create a prompt file: test-generator-selection.prompt.md. Hint: Use the content from other selection.prompt.md files as a reference.
6. Open Copilot Chat in test-generator mode.
7. Open app.py and select the new adoption route.
8. Prompt: `/test-generator-selection #file:selection ` to generate tests for the new route.
9. Review the generated tests and save them in test_app.py.

### Part F: Documentation Sync (Exercise 5)
1. Verify documentation sync:
   ```
   @workspace Compare the new adoption route with #file:API.md. 
   Generate documentation for the missing endpoint.
   ```
2. Adjusts the API.md file accordingly.

### Part G: PR Workflow (Exercises 3, 9)
1. Stage and commit changes with AI-generated message:
   - `git add .`
   - Follow the commit message generation steps from Exercise 9, Part B (steps 3-6)
2. Push the branch: `git push -u origin feature/adoption-status`
3. Create a diff: `git diff main...feature/adoption-status > adoption-pr.diff`
4. Generate PR description using your template:
   ```
   Generate a pull request description using #file:pull-request-template.md 
   based on #file:adoption-pr.diff
   ```
5. If MCP Server is configured, create the PR via Agent mode: `Create a pull request using the generated title and description`
   - Approve if prompted
6. Verify PR creation on GitHub

### Reflection Questions
After completing this exercise, consider:
1. Which review technique caught the most critical issues?
2. How did combining multiple specialized reviews improve coverage?
3. What would you add to your team's review checklist based on this experience?
4. How much time did using Copilot save compared to manual review?
5. Which agents or prompts would you customize further for your projects?

### Success Criteria
- [ ] All security vulnerabilities identified and fixed
- [ ] Code passes all checklist items
- [ ] Comprehensive tests added (>80% coverage for new code)
- [ ] API documentation updated
- [ ] PR description follows team template
- [ ] Final review report generated and saved.


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

### Team Collaboration
- Generate review reports for consistency
- Use Copilot to explain complex feedback
- Create reusable review templates and agents
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
- `Review #selection for performance issues using #file:.github/performance-review-guide.md`
- `Review #selection for accessibility issues using #file:.github/accessibility-review-guide.md`

### Documentation & Reporting
- `Generate a code review report for #selection`
- `@workspace Check if #file:API.md reflects changes in #file:server/app.py`
- `Generate constructive feedback for #selection`

### Follow-up Questions
- `What are the top 3 improvements needed?`
- `Are there any security concerns?`
- `Is this code testable?`
- `Does this follow our project standards?`
