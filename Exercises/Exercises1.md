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
1. Open app.py. Prompt in Ask:
   - Ask: `Create a Flask route that returns all breeds (used by the frontend filter). It should query the Breed model and returns JSON.`
   - Note the output quality.
2. Create a new file in the **.github** folder called **copilot-instructions.md**.
3. Add standards for:
   - Python backend (type hints, PEP 8, docstrings)
   - Frontend (TypeScript, Svelte patterns, accessibility)
   - Testing (AAA pattern, 80% coverage, mocking)

Paste content below:
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

4. Create a New Chat and Ask Copilot for the same prompt:
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
4. **Reflection**: When is inline sufficient? (Answer: For simple, file-local tasks)
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
4. Close all files. Create a New Chat.
5. Ask: `@workspace How should I add a new endpoint for dog age calculation?`
6. Close all files again. Create a New Chat.
7. Ask: `@workspace How should I add a new endpoint for dog age calculation? Create a plan first with no code yet.`
8. **Reflection**:  
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
1. Highlight any route in `server/app.py`
2. Use `/explain`
3. Ask: `Are all routes following the same pattern?`
4. **Learning**: Identify consistency in the codebase

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
2. Go to Plan mode in Copilot Chat
3. Prompt:
   @workspace  Create a plan to implement an API endpoint: GET /api/dogs/<id>/human-age
   Requirements:
   - Returns JSON: {"dog_name": "Max", "dog_age": 3, "human_age": 21}
   - Formula: First 2 years = 10.5 human years each, then 4 years per year after
   - Must be shown in the front end
   - Add tests
4. Review Copilot's architectural suggestion
5. Go to Agent mode in Copilot Chat
6. Prompt:
`Save the plan above for implementing the dog age calculator endpoint as a checklist in dog-age-plan.md file in the root directory.`
7. Save the file.

### Part B: Implementation of Changes
1. Close all tabs and open  `dog-age-plan.md`
2. Go to aAgent mode in Copilot Chat
3. Select the first step in the plan which is to modify `server/app.py`
4. Prompt: `Implement the steps in #selection  one by one in #file:app.py. Place it at the bottom of the file`
5. Review the generated code for the new endpoint and accept.


### Part C: Documentation as You Code
1. Place cursor inside get_dog function
2. Press Ctrl+I
3. Use `/doc` to generate docstring
4. Verify it matches your Google-style standard from instructions.

### Part D: Validation
1. Open a new chat and select the new function created.
2. Prompt in Ask: `#selection How can I test this endpoint?`
   - Copilot should suggest either curl commands or a URL to test the endpoint
3. Run the app and verify the new endpoint works (the endpoint URL will vary based on your implementation):
   - `curl http://localhost:5100/api/dogs/1/human-age`

---

## **Exercise 6: Testing & Quality Assurance**

**Overview**: Feature is built - now ensure quality through comprehensive testing.

### Part A: Generate Tests with `/tests`
1. Open `server/app.py`
2. Highlight your `get_dog_human_age()` function (or whatever you named it)
3. Create a New Chat, type: `/tests`
4. **Verify**: 
   - AAA pattern (Arrange, Act, Assert)?
   - Proper mocking of database calls?
5. Select the new tests created in `test_app.py` and prompt in Ask:
   `Add tests cases to #selection for edge cases, boundaries, and constraints`
6. Review and accept additional test cases
7. Verify if testing follows your testing standards in `dog-age-plan.md`? 
8. Prompt: verify if #file:test_app.py matches the criteria in #file:dog-age-plan.md:30-39 (select the testing standards section of `dog-age-plan.md`) 
9. Run tests in terminal: `cd server && python -m pytest test_app.py -v`
10. If tests fail, highlight the failing test
11. Use `/fix` to correct issues
12. **Goal**: All tests passing, 80%+ coverage

### Part B: Test Coverage Analysis
1. Create New Chat and close all tabs. Ask: `@workspace Which routes in app.py lack test coverage?`
2. Review the list of untested routes
3. For each untested route:
   - Highlight the route in `server/app.py` such as get_dogs()
   - Use `/tests` command
   - Prompt to add edge cases, boundaries, and constraints in the tests created.
   - Copy generated tests to `server/test_app.py`
4. Run tests in terminal: `cd server && python -m pytest test_app.py -v`
5. If tests fail, highlight the failing test
6. Use `/fix` to correct issues
7. **Goal**: All tests passing, 80%+ coverage

### Part C: Generate Test Data (Optional)
1. Create New Chat and close all tabs. Ask: `Create realistic test fixtures for dog data following the Dog model schema`
2. Use Agent. Prompt: create a test_fixtures.py file in the same directory as test_app.py
3. Use Agent. Prompt: refactor #file:test_app.py to use #file:test_fixtures.py for consistency
4. Again verify all tests pass with `cd server && python -m pytest test_app.py -v`
5. Use `/fix` if any tests fail
---

## **Exercise 7: Debugging & Refactoring**

**Overview**: Code works and is tested - now optimize and fix issues.

### Part A: `/fix` for Debugging
1. Open `server/app.py`
2. Temporarily introduce a bug: Remove a closing parenthesis from a route decorator
3. Highlight the broken code
4. Go to Ask in Chat and Prompt: `/fix`
5. Review and accept the fix
6. Introduce another bug: Change `@app.route('/api/dogs', methods=['GET'])` to `@app.route('/api/dogs', methods=['POST'])` 
7. Run the backend and test the endpoint via browser: `http://localhost:5100/api/dogs`
8. You should get a 405 Method Not Allowed error
9. Take a screenshot of the error including the URL bar.
10. Paste the screenshot into Copilot Chat and prompt in Ask: `@workspace help solve this issue`
11. Copilot should be able to read the screenshot and suggest the fix.
12. Fix the code and re-run the test to verify the endpoint works again.

### Part B: Code Optimization with `/optimize`
1. Highlight the `get_dogs()` route function
2. Create a New Chat in Copilot Chat, then type: `/optimize #selection`
3. Review suggestions (might include and will vary based):
   - Added error handling
   - Added docstring
4. Apply improvements that make sense.
5. Re-run tests to ensure nothing broke.  
6. Use the /fix command if errors are introduced

### Part C: Refactoring with Context
1. Open  `server/app.py`
2. Agent: `How can I reduce code duplication across these routes?`
3. Review Copilot's refactoring suggestions
4. Accept and implement refactoring
5. Re-run tests to ensure refactoring didn't break functionality

---

## **Exercise 8: Documentation & Code Hygiene**

**Overview**: Before committing, ensure code is well-documented and clean.

### Part A: Documentation Sweep
1. Close all tabs and open New Chat.
2. Attach the server folder in Chat.
3. Prompt in Agent: `Search for all files in the folder to see if they follow documentation standards as specified in the instructions file`
4. Review the list of undocumented functions
5. Prompt in Agent: `fix the documents with missing docstrings`
6. Run tests to ensure no functionality is broken.
7. **Goal**: Document an entire module in under 5 minutes

### Part B: API Documentation
1. Create new Chat. Use Agent and propmpt: `@workspace Generate API documentation called API.md in root directory for all APIs in this app including: curl examples, request format, response schema, and error cases`
2. Review the generated documentation format

### Part C: Code Comments for Complex Logic
1. Identify complex sections (algorithms, business logic) in your code
2. Prompt in Agent: `@workspace search for the most complex part of the app logic`
3. Highlight any complex section mentioned in #2.
4. Ask: `Add inline comments explaining this logic step by step`
5. Review generated comments

---

## **Exercise 9: Commit & Collaboration**

**Overview**: Feature is complete, tested, optimized, and documented - ready to commit!

### Part A: Generate Meaningful Commit Messages
1. Create a new branch: `git switch -c feature/dog-age-api`
2. Stage your changes: `git add .`
3. Open the Source Control panel (Ctrl+Shift+G)
4. Click the **sparkles** icon (✨) next to the commit message box
5. Select **Generate Commit Message**
6. **Verify**: 
   - Does it capture the intent of your changes?
7. If not, copy the generated message and place in a notepad. 
8. Prompt in Ask: `@workspace Improve this commit message to better reflect the changes made, following best practices for clarity and detail.` Then paste the copied message before sending the request to Copilot Chat.
9. Prompt for any missing details or improvements.
10. Use the improved message for your commit.

### Part B [Optional]: Standardize Commit Messages using Commit Message Instructions
1. Go to Manage Settings (gear icon) in Source Control panel
2. Select **GitHub Copilot Settings**. Search for Commit Message Generation: Instructions
3. Click Edit in settings.json
4. Select "file": ".copilot-commit-message-instructions.md"
5. Create a new file in the root directory called `.copilot-commit-message-instructions.md`
6. Copy and paste the content from this url: https://github.com/SimonSkoda13/Copilot-commit-message-instructions/blob/main/.copilot-commit-message-instructions.md
7. Save the file
8. Repeat steps 3-10 in Part A to generate a new commit message. However, this time add the new instructions file as reference to the prompt. `@workspace Improve this commit message to better reflect the changes made, following best practices for clarity and detail. Use this file as reference: #file:.copilot-commit-message-instructions.md`
9. **Verify**: 
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

## **Exercise 10: Advanced Context Mastery** *(Bonus)*

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
