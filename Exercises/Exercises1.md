# GitHub Copilot Advanced Exercises

## Main Overview
This comprehensive set of exercises follows a logical development workflow to help you master advanced GitHub Copilot concepts. Work through them sequentially to build your skills from foundation to advanced techniques.

### Exercise Goals
Create and apply Copilot Instructions for organisational style and standards.
Understand inline, chat, and contextual scopes in Copilot.
Optimise Copilot performance by managing file- and repo-level context.
Use Copilot Chat commands (/fix, /explain, /tests, /optimise) for debugging and refactoring.
Apply Copilot for inline documentation, test coverage, and clean commit hygiene.


---
## **Prerequisites - Setup**
1. Visit url: https://github.com/github-samples/pets-workshop
2. Click on **Use this template** to create a new repository in your own GitHub account. Name the repository `pets-workshop`.
3. Clone the new repository to your local machine. Open VS Code and Run in terminal:
   - `git clone https://github.com/your-username/pets-workshop.git`
4. Open the project in VS Code

---

## **Exercise 1: Foundation - Copilot Instructions & Standards**

**Overview**: Before writing any code, developers must understand a project. In addition, they must establish standards that sets guardrails for all subsequent exercises.

### Part A: Analyze Existing Project Standards and Getting Started with a Project
1. Explore the workspace structure
2. Perform the prompt using a free model such as GPT-5 mini vs. a premium model such as Claude Sonnet 4 or 4.5
    - Ask: `@workspace What coding patterns and conventions are used in this project?`
    - Notice the length and detailed response between the two models.
    - Identify Python (Flask/SQLAlchemy) and Svelte/Astro patterns
3. Ask: `@workspace How to run this application?`
4. Command to run the app will be given: `./scripts/start-app.ps1 `.  Run it in terminal:
    - PowerShell: `.\scripts\start-app.ps1`
    - Bash: `./scripts/start-app.sh`
5. See if the following are loaded and working:
    - Frontend (Astro/Svelte): http://localhost:4321
    - Backend (Dogs API endpoint): http://localhost:5100/api/dogs

### Part B: Create Organizational Instructions
1. Open server/app.py. Prompt in Ask:
   - Ask: `Create a Flask route that returns all breeds (used by the frontend filter). It should query the Breed model and returns JSON.`
   - Note the output quality.
   - Make sure you are in Ask mode, not Agent mode so that the file is not modified yet.
2. Create a new file in the **.github** folder called **copilot-instructions.md**.
3. Add standards for:
   - Python backend (type hints, PEP 8, docstrings)
   - Frontend (TypeScript, Svelte patterns, accessibility)
   - Testing (AAA pattern, 80% coverage, mocking)

Paste content below and SAVE the file:
````markdown
# Dog Shelter Application

## Organizational Standards

### Code Quality
- All functions must have docstrings following Google style
- Maximum function length: 50 lines
- Use descriptive variable names (no single letters except loop counters)

### Python Backend
- Use type hints for all function parameters and return values
- Follow PEP 8 strictly
- All database queries must use SQLAlchemy ORM (no raw SQL)
- All routes require corresponding unit tests in `test_*.py`

### Frontend
- Use Svelte components with TypeScript
- All API calls must include error handling
- Use arrow functions consistently
- Accessibility: All interactive elements need aria-labels

### Testing
- Test coverage must exceed 80%
- Mock all external dependencies
- Tests should follow AAA pattern (Arrange, Act, Assert)
````

4. Create a New Chat in Ask  and Ask Copilot for the same prompt:
    - Ask: `Create a Flask route that returns all breeds (used by the frontend filter). It should query the Breed model and returns JSON.`
    - Compare with result in Part A and verify if new result follows your standards
    - Check if **References** shows `copilot-instructions.md`

---

## **Exercise 2: Understanding Context & Scopes**

**Overview**: Now that standards are set, understand how to control what Copilot "sees" to get better suggestions.

### Part A: Inline Suggestions (File-level Context)
1. Open `server/app.py`
2. Go the the end of the file but before 'if __name__ == '__main__':'
   Add comment: `# Route to get dogs by size (small, medium, large)`
3. Accept the suggested completion by pressing the Tab key
4. **Reflection**: 
   - When is inline sufficient? (Answer: For simple, file-local tasks)
   - Did it follow the instructions in `copilot-instructions.md`? 
5. Remove the newly added route to keep the code clean

### Part B: Chat Context (Multi-file Awareness)
1. Close all tabs. Open GitHub Copilot Chat
2. Try in Ask: `@workspace I want to create a Flask route that returns all breeds (used by the frontend filter). It should query the Breed model and returns JSON. I should also be able to display this in the front-end as well. What files I need to modify?`
3. Expand the **References** section - note multiple files used
4. Compare between premium and non-premium models: Note the depth of understanding and quality of suggestions
5. **Reflection**: 
   - How does chat differ from inline? (Answer: Chat can access multiple files and provide explanations)
   - Compare how different models handle multi-file context and compare the length and quality of responses

### Part C: Contextual Scope Experiments
1. Close all tabs, open only `client/src/components/DogList.svelte`
2. Create a New Chat use a **free model**. Ask in chat: `How does this component fetch data?`
3. Note the response, then open `server/app.py` in a tab
4. Ask the same question again with app.py as the selected tab.
5. **Reflection**: How did adding context change the answer quality? 
   - First query was limited to DogList.svelte only
   - Second query would describe app.py and may reference DogList.svelte
6. Ask new prompt: `How does the frontend #file:DogList.svelte  fetch dog data from the backend #file:app.py ?`   
7. **Reflection**: Last query used explicit file references. How did that affect the response?
8. Perform Step 1-7 again but this time using a premium model such as Claude Sonnet 4.5
---

## **Exercise 3: Managing Context for Performance**

**Overview**: You understand scopes; now optimize them for better performance and accuracy.

### Part A: Using `@workspace` vs `#file:`
1. Closed all tabs: Compare these queries:
   - `@workspace How are dogs filtered?`
   - `#file:server/app.py How are dogs filtered?`
2. **Rule of thumb**: 
   - Use `@workspace` for discovery/architecture questions
   - Use `#file:` for implementation questions

### Part B: Context Narrowing vs. Broadening Strategies
1. Plan to add a new API endpoint
2. Open ONLY: `server/app.py`, `server/models/dog.py`. Close unrelated frontend files
3. Ask: `How should I add a new endpoint for dog age calculation?`
4. Observe the response
5. Close all files. Create a New Chat.
6. Ask: `@workspace How should I add a new endpoint for dog age calculation?`
7. Observe the response
8. Close all files again. Create a New Chat.
9. Ask: `@workspace How should I add a new endpoint for dog age calculation? Create a plan first with no code yet.`
10. Observe the response
11. **Reflection**:  
    - How did managing open files affect the responses?
    - How did @workspace change the result.
    - How did creating a plan first help?

---

## **Exercise 4: Reading & Understanding Code**

**Overview**: Before modifying code, developers read and understand existing implementation.

### Part A: `/explain` for Understanding
1. Close all tabs. Open `client/src/components/DogList.svelte`
2. Highlight the `onMount` function
3. Go to the Copilot Chat pane
4. Type: `/explain`
5. Ask: `What does this do in the component lifecycle?`

### Part B: Exploring Architecture
1. Ask: `@workspace Explain the data flow from database to frontend`
2. Review the explanation
3. Follow up: `/explain #file:server/models/dog.py the Dog model relationships`

### Part C: Understanding Patterns
1. Close all files and open `server/app.py`. Highlight any route in `server/app.py`
2. Use `/explain`
3. Ask: `Are all routes following the same pattern?`
4. **Learning**: Identify consistency in the codebase

**Reflection**: 
   - How did `/explain` help in understanding code?
   - How did workspace-level context improve architectural understanding?
   - How did file-level context help in understanding specific implementations?

---

## **Exercise 5: Feature Development Workflow**

**Overview**: Now you understand the codebase - time to build! This is the core development activity.

### Scenario: Add Dog Age Calculator
Create an API endpoint: `GET /api/dogs/<id>/human-age`

**Requirements**:
- Returns JSON: `{"dog_name": "Max", "dog_age": 3, "human_age": 21}`
- Formula: First 2 years = 10.5 human years each, then 4 years per year after

### Part A: Planning with Context
1. Close all tabs.
2. Go to Ask mode in Copilot Chat
3. Prompt:
```
   @workspace  Create a plan to implement an API endpoint: GET /api/dogs/<id>/human-age
   Requirements:
   - Returns JSON: {"dog_name": "Max", "dog_age": 3, "human_age": 21}
   - Formula: First 2 years = 10.5 human years each, then 4 years per year after
   - Must be shown in the front end
   - Add tests
```
4. Review Copilot's architectural suggestion
5. Go to Agent mode in Copilot Chat
6. Prompt:
`Save the plan above for implementing the dog age calculator endpoint as a checklist in dog-age-plan.md file in the root directory.`
7. Save the file.

### Part B: Implementation of Changes
1. Close all tabs and open  `dog-age-plan.md`
2. Go to Agent mode in Copilot Chat
3. Select the first step in the plan which is to modify `server/app.py`
4. Prompt: `Implement the steps in #selection  one by one in #file:app.py. Place it at the bottom of the file`
5. Review the generated code for the new endpoint and accept. Close app.py.
6. Select the step in the plan to modify `src/components/DogList.svelte`.
7. Prompt: `Implement the steps in #selection one by one in #file:DogList.svelte.`
8. Review the generated code for the new endpoint and accept.

### Part C: Documentation as You Code
1. Open app.py Place cursor inside get_dog function
2. Press Ctrl+I
3. Use `/doc` to generate docstring
4. Verify it matches your Google-style standard from instructions.
5. Accept the docstring and save the file.

### Part D: Validation
1. Open a new chat and select the new function get_dog_human_age created in app.py.
2. Prompt in Ask: `#selection How can I test this endpoint?`
   - Copilot should suggest either curl commands or a URL to test the endpoint
3. Run the app and verify the new endpoint works (the endpoint URL will vary based on your implementation):
   - Run in New Terminal: `curl http://localhost:5100/api/dogs/1/human-age`
   - Browswer: `http://localhost:5100/api/dogs/1/human-age`
4. Open DogDetails.svelte and Prompt in Ask: `#file:DogDetails.svelte How can I verify the human age is displayed correctly?`
5. Follow the instructions to verify the frontend displays the human age correctly.

**Reflection**: 
   - How did planning first improve implementation?
   - Did Copilot follow your organizational standards during implementation?
---

## **Exercise 6: Testing & Quality Assurance**

**Overview**: Feature is built - now ensure quality through comprehensive testing.

### Part A: Generate Tests with `/tests`
1. Open `server/app.py`
2. Highlight your `get_dog_human_age()` function (or whatever you named it)
3. Create a New Chat, type: `/tests`
4. If prompted to Configure Test Framework, select Accept
   - Choose pytest as the framework
   - Choose server as the test folder
   - You would then need to re-highlight the function and use `/tests` again in the new chat.
5. Select the new tests created in `test_app.py` and prompt in Ask:
   `Add tests cases to #selection for edge cases, boundaries, and constraints`
6. Review and accept additional test cases
   - Click Allow to have Agent test the unit tests.
7. Verify if testing follows your testing standards in `dog-age-plan.md`.
8. Open dog-age-plan.md and select the backend testing standards section.
9. Prompt: `verify if #file:test_app.py matches the criteria in #selection for get_dog_human_age.`
10. Prompt a follow-up: `Do we have any missing tests to #file:test_app.py that we should be adding?`
   - Accept the new scenarios suggested by Copilot if any.
   - Have Copilot add them to test_app.py and retest.
11. Prompt in Agent:
   `Add the new tests in #file:test_app.py  to the #file:dog-age-plan.md  testing checklist that are missing.`   
12. Manually run tests in terminal in server folder: `python -m pytest test_app.py -v`
13. If tests fail, highlight the failing test
14. Use `/fix` to correct issues
15. **Goal**: All tests passing, 80%+ coverage
**Reflection**: 
   - How effective was `/tests` in generating initial test cases?
   - How did asking for additional test cases improve coverage?

### Part B: Create a Test Agent for Coverage Analysis
1. Create a test agent.
   - Select Cog -> Custom Agents -> + Create new custom agent -> .github\agents
   - Name it: test
   - Copy and paste the content from test-agent.md found in the Agent folder of the exercise repository into the new agent.md file.
2. Save the agent file.

### Part C: Test Coverage Analysis
1. Create New Agent Chat and close all tabs. Ask: `@workspace Which routes in app.py lack test coverage?`
2. Review the list of untested routes
3. Highlight the get_dogs route in `server/app.py`
4. Select **test** agent created in Part B
5. Prompt: `Create tests for the #selection`
6. Repeat steps 3-5 for get_dog route.
7. Review and accept the generated tests in `test_app.py`
8. Verify all tests pass with `cd server && python -m pytest test_app.py -v`
**Reflection**: 
   - How did the test agent help identify test scenarios?
   - How did the test agent reduce manual effort in test creation?

### Part D: Generate Test Data
1. Create New Chat and close all tabs. Ask: `@workspace Create realistic test fixtures for dog data following the Dog model schema`
2. Use Agent. Prompt: `create a test_fixtures.py file in the same directory as test_app.py`
3. Use Agent. Prompt: `refactor #file:test_app.py to use #file:test_fixtures.py for testing`
4. Manually verify all tests pass with `cd server && python -m pytest test_app.py -v`
5. Use `/fix` if any tests fail
6. **Reflection**: How did using test fixtures impact your testing process?

---

## **Exercise 7: Debugging & Refactoring**

**Overview**: Code works and is tested - now optimize and fix issues.

### Part A: `/fix` for Debugging
1. Open `server/app.py`
2. Temporarily introduce a bug: Remove a closing parenthesis from a route decorator
3. Highlight the broken code
4. Go to Agent in Chat and Prompt: `/fix fix the error`
5. Review and modify the fix
6. Introduce another bug: Change `@app.route('/api/dogs', methods=['GET'])` to `@app.route('/api/dogs', methods=['POST'])`. Save the file. 
7. Run the app and test the endpoint via browser: `http://localhost:5100/api/dogs`
8. You should get a 405 Method Not Allowed error
9. Take a screenshot of the error including the URL bar.
10. Paste the screenshot into Copilot Chat and prompt in Ask: `@workspace help solve this issue`
11. Copilot should be able to read the screenshot and suggest the fix.
12. Fix the code and re-run the test to verify the endpoint works again.
**Reflection**: 
   - How effective was `/fix` in resolving issues?
   - How did a screenshot input help in debugging?

### Part B: Code Optimization with `/optimize`
1. Highlight the `get_dogs()` route function
2. Create a New Chat in Copilot Chat, then type: `/optimize #selection`
3. Review suggestions (might include and will vary based):
   - Added error handling
   - Added docstring
4. Apply improvements that make sense.
5. Re-run tests to ensure nothing broke.  
6. Use the /fix command if errors are introduced
**Reflection**: 
   - What optimizations were suggested?
   - Did they improve code quality?

### Part C: Refactoring with Context
1. Close all tabs. Open  `server/app.py`
2. Open a new chat in Ask: `How can I reduce code duplication across these routes in #file:app.py?`
3. Review Copilot's refactoring suggestions
4. Do not implement any refactoring
5. Close all tabs.
6. Open a new chat in Ask mode. Prompt:
```
   @workspace Analyze the codebase and identify the best candidate for 
   refactoring based on:
   
   - Code complexity
   - Standards violations (check against #file:.github/copilot-instructions.md)
   - Missing documentation
   - Lack of tests
   - Duplicate code
   
   Provide:
   1. Top 3 refactoring candidates
   2. Reasoning for each
   3. Estimated effort
   4. Expected quality improvement
   5. Risk assessment
```
7. Review the suggestions.
**Reflection**: 
   - How did workspace-level analysis differ from file-level?
   - Which approach provided more actionable insights?
---

## **Exercise 8: Documentation & Code Hygiene**

**Overview**: Before committing, ensure code is well-documented and clean.

### Part A: Documentation Sweep
1. Close all tabs and open New Chat in Agent.
2. Prompt in Agent: `@workspace Search for all files to see if they follow documentation standards as specified in the instructions file`
   - Type `yes` if agent asks for confirmation to create a script to scan files.
3. Review the list of undocumented functions
4. Prompt in Agent: `fix #file:dog.py to meet documentation standards`
5. Review and accept generated docstrings
6. Repeat for other files with missing documentation [Optional]

### Part B: API Documentation
1. Create new Chat. Use Agent and propmpt: `@workspace Generate API documentation for all APIs in this app including: curl examples, request format, response schema, and error cases.`
2. Review the generated documentation format
3. Save it in docs\api-documentation.md

### Part C: Code Comments for Complex Logic
1. Identify complex sections (algorithms, business logic) in your code
2. Create New Chat. Prompt in Agent: `@workspace search for the most complex part of the app logic`
3. Open the file and select the complex section mentioned in #2.
4. Ask: `Add inline comments to the #selection explaining this logic step by step to guide developers in the future on the logic`
5. Review generated comments

**Reflection**: 
   - How did Copilot assist in improving documentation?
   - Did it follow your organizational standards?
   - How did inline comments enhance code readability for complex logic?
---

## **Exercise 9: Commit & Collaboration**

**Overview**: Feature is complete, tested, optimized, and documented - ready to commit!

### Part A: Generate Meaningful Commit Messages
1. Create a new branch: `git switch -c feature/version2`
2. Stage your changes: `git add .`
3. Open the Source Control panel (Ctrl+Shift+G)
4. Click the **sparkles** icon (✨) next to the commit message box
5. Select **Generate Commit Message**
6. **Verify**: 
   - Does it capture the intent of your changes?
   - Does it have a format that follows best practices (e.g., concise summary, detailed body)?
7. Copy the generated message in a notepad for comparison later
8. Do not perform the commit yet.

### Part B: Standardize Commit Messages using Commit Message Instructions
1. Click Ctrl+, (Ctrl and comma) to open Settings
2. Search for: Commit Message Generation: Instructions
3. Click Edit in settings.json
4. Select "file": ".copilot-commit-message-instructions.md"
5. Create a new file in the root directory called `.copilot-commit-message-instructions.md`
6. Copy and paste the content from this url: https://github.com/SimonSkoda13/Copilot-commit-message-instructions/blob/main/.copilot-commit-message-instructions.md
7. Save the file
8. Open the Source Control panel (Ctrl+Shift+G)
9. Click the **sparkles** icon (✨) next to the commit message box
10. Select **Generate Commit Message**
11. **Verify**: 
   - Does it now follow the new commit message standards?
   - Compare with previous commit message

### Part C: Standardize Commit Messages using Git Diff
1. Run the following command in terminal to get the git diff of your changes:
   - `git diff --cached --stat > changes-summary.txt`
   - `git diff --cached > changes-detailed.txt`
2. The first file contains a summary of changes, while the second file contains detailed diffs.
3. Open a new chat in Copilot Chat
4. Create a prompt in Ask:
   - `@workspace Generate a meaningful commit message based on the following summary of changes: #file:changes-summary.txt and detailed diffs: #file:changes-detailed.txt using the organizational commit message standards in #file:.copilot-commit-message-instructions.md`
5. Review and compare with previous commit messages.
6. Use the best commit message for your commit.

**Reflection**: 
   - How did commit message instructions improve the quality of messages?
   - How did having a commit message standard help in consistency?
   - How did using git diff enhance the context for generating commit messages?

---

## **Exercise 10: Front-End Testing using Playwright MCP server** *(Bonus)*
**Overview**: Ensure front-end components work as expected using Playwright MCP server.
### Part A: Test Generation for Frontend Components
1. Create a New Chat in Copilot Chat
2. Prompt in Agent: `Create a test plan for #file:DogDetails.svelte and #file:DogList.svelte`
3. Prompt in Agent: `Save the generated tests in client/tests/ folder`
4. Prompt in Agent: `Read through the files and create me a user readable list of tests`
5. Save the list in `frontend-test-plan.md` in the client/tests folder

### Part B: Playwright MCP Server Setup
1. Go to Extensions in VS Code (Ctrl+Shift+X)
2. Search for "@mcp Playwright" by Microsoft.com and install it
3. Click the Cog icon next to the extension and select Show Configuration (JSON)
4. Click the Start MCP Server button

### Part C: Playwright MCP Testing
1. Open a new chat in Agent.
2. Prompt in Agent: 
```
Run the #file:frontend-test-plan.md using the Playwright MCP Server and provide a summary of results.
The Frontend can be access via this url: http://localhost:4321
```
3. Notes: 
- You may be prompted to install Playwright. Click Yes to install.
- You will be asked for permissions to run the tests. Click Allow.
4. Observe how Playwright MCP server runs the tests and provides results.
5. Save the test results in `frontend-test-results.md` in the client/tests folder
**Reflection**: 
   - How did Playwright MCP server simplify front-end testing?
   - How effective was Copilot in generating front-end tests?
---

## **Exercise 11: Advanced Context Mastery** *(Bonus)*

**Overview**: You've mastered the basics - now tackle on your own.

### Part A: Cross-Stack Feature
**Task**: Add breed filter to both API and frontend

1. **Backend Phase**:
   - Open backend files: `server/app.py`, `server/models/breed.py`
   - Close all frontend files
   - Ask: `Add a breed filter parameter to the dogs endpoint`
   - Implement with full backend context
   - Generate tests

2. **Frontend Phase**:
   - Close backend files
   - Open frontend files: `client/src/components/DogList.svelte`
   - Ask: `Add a breed dropdown filter that calls the updated API`
   - Implement with frontend context

3. **Integration Phase**:
   - Open both `server/app.py` and `DogList.svelte`
   - Ask: `Verify the frontend and backend integration for breed filtering`

4. **Commit**: 
   - Create a new branch: `git switch -c feature/breed-filter`
   - Add the changes: `git add .`
   - Generate a meaningful commit message using Copilot


### Part B: Complex Refactoring
1. Ask: `@workspace Suggest architecture improvements for better scalability of the pets application`
2. Evaluate suggestions (might include: caching, pagination, service layer)
3. Choose one improvement to implement
4. Use `/optimize` on multiple affected files
5. Ensure all tests still pass after refactoring

---

## **Assessment Checklist**

After completing all exercises, you should be able to:

- [ ] Create and maintain a Copilot instructions file that enforces team standards
- [ ] Explain the difference between inline, chat, and workspace context
- [ ] Deliberately manage context by opening/closing relevant files
- [ ] Use `/explain` to understand unfamiliar code
- [ ] Implement new features using chat guidance
- [ ] Generate comprehensive tests using `/tests` command
- [ ] Debug code efficiently with `/fix` command
- [ ] Optimize existing code using `/optimize` command
- [ ] Generate high-quality documentation with `/doc` command
- [ ] Create meaningful commit messages with Copilot assistance
- [ ] Validate that all generated code follows organizational standards
- [ ] Switch context effectively between different parts of a stack
- [ ] Iterate on instructions based on real usage patterns


## **Next Steps**

After completing these exercises, consider:

1. **Apply to Real Projects**: Use these techniques in your actual work
2. **Share with Team**: Train teammates on effective Copilot usage
3. **Refine Instructions**: Continuously improve your `.github/copilot-instructions.md`
4. **Explore Advanced Features**: Try Copilot in PR reviews, issue triage, and more
5. **Measure Impact**: Track productivity improvements and code quality metrics
