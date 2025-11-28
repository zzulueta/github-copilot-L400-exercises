# GitHub Copilot Governance & Documentation Exercises

## Main Overview
This comprehensive set of exercises guides you through establishing lightweight governance processes, conducting peer reviews, automating documentation, and implementing end-to-end quality workflows using GitHub Copilot Chat. Work through them sequentially to build skills from basic documentation to advanced governance patterns.

### Exercise Goals
- Establish lightweight governance processes using Copilot
- Conduct systematic peer reviews with conversational tools
- Automate documentation generation and maintenance
- Generate comprehensive PR summaries for knowledge sharing
- Execute end-to-end quality workflows (refactor → review → document)
- Demonstrate Copilot's role in organizational governance
- Leverage GitHub MCP Server for automated governance enforcement

---

## **Prerequisites - Setup for Governance Session**

**Overview**: Follow these quick setup steps to prepare for governance and documentation exercises.
### Part A: Clone the Sample Repository (if you haven't done any exercise)
1. Visit url: https://github.com/github-samples/pets-workshop
2. Click on **Use this template** to create a new repository in your own GitHub account. Name the repository `pets-workshop`.
3. Clone the new repository to your local machine. Open VS Code and Run in terminal:
   - `git clone https://github.com/your-username/pets-workshop.git`
4. Open the project in VS Code

### Part B: Verify Application Setup
1. Start the application via terminal:
   - PowerShell: `.\scripts\start-app.ps1`
   - Bash: `./scripts/start-app.sh`
2. Verify both frontend and backend are working
   - Frontend: http://localhost:4321
   - Backend: http://localhost:5100/api/dogs

### Part C: Verify Organizational Standards Exist
1. Copy the  `.github/copilot-instructions.md` from exercises repository to the `.github/` folder in your cloned repository
2. Copy the `code-review-checklist.md` from exercises repository to the root directory in your cloned repository
3. Copy the `pull-request-template.md` from exercises repository to the root directory in your cloned repository

### Part D: Create Governance Documentation Folder
1. Create folder structure: `docs/governance/`
2. Create folder: `docs/api/`
3. Create folder: `docs/architecture/`
4. Create folder: `docs/governance/mcp-reports/`
5. This structure will hold governance artifacts generated during exercises

---

## **Exercise 1: Establishing Lightweight Governance**

**Overview**: Create governance documentation that defines standards, processes, and review criteria for the team.

### Part A: Generate Team Coding Standards
1. Create a New Chat in Copilot
2. Prompt:
   ```
   @workspace Based on the code in this repository and standards in 
   #file:.github/copilot-instructions.md, generate a comprehensive 
   Team Coding Standards document that includes:
   
   ## Python Backend Standards
   - Code style and formatting
   - Error handling patterns
   - Database query best practices
   - API endpoint conventions
   
   ## Frontend Standards
   - Component structure
   - State management
   - Accessibility requirements
   - TypeScript conventions
   
   ## Testing Standards
   - Test coverage requirements
   - Test naming conventions
   - Mock usage guidelines
   
   ## Documentation Standards
   - Docstring requirements
   - API documentation format
   - Code comment guidelines
   ```
3. Review the generated standards
4. Save to: `docs/governance/coding-standards.md`
5. Ask follow-up: `Are these standards consistent with existing code patterns in the repository?`
6. **Reflection**:
   - How do these standards align with our current practices?
   - What areas need the most improvement?
   - How can we ensure team adoption of these standards?

### Part B: Create Change Management Process
1. Create a New Chat
2. Prompt:
   ```
   Generate a lightweight Change Management Process document suitable 
   for a small development team. Include:
   
   ## Change Types
   - Minor changes (bug fixes, small features)
   - Major changes (architecture, breaking changes)
   - Emergency changes (security patches, critical bugs)
   
   ## Review Requirements by Change Type
   - Who must review
   - Required approvals
   - Testing requirements
   
   ## Documentation Requirements
   - When documentation must be updated
   - What documentation is required
   
   ## Communication Requirements
   - Team notification process
   - Stakeholder communication
   
   Base this on GitHub flow principles and reference our standards in 
   #file:.github/copilot-instructions.md
   ```
3. Review and refine the process
4. Save to: `docs/governance/change-management.md`
5. **Reflection**:
   - How does this process balance agility with control?
   - What challenges might arise in following this process?
   - How can we streamline communication around changes?

### Part C: Define Code Review Governance
1. Create a New Chat
2. Prompt:
   ```
   Create a Code Review Governance document that defines:
   
   ## Review Objectives
   - What reviews are designed to catch
   - Quality goals and metrics
   
   ## Review Process
   - When reviews are required
   - Review timeline expectations
   - Escalation process for disagreements
   
   ## Reviewer Responsibilities
   - What reviewers must check
   - How to provide constructive feedback
   - When to approve vs. request changes
   
   ## Author Responsibilities
   - Pre-review checklist
   - How to respond to feedback
   - When to merge
   
   Reference #file:code-review-checklist.md and #file:pull-request-template.md
   ```
3. Save to: `docs/governance/review-governance.md`
4. **Reflection**:
   - How does this governance promote effective reviews?
   - What responsibilities are most critical for reviewers?
   - How can we foster a positive review culture?

### Part D: Create Documentation Standards
1. Create a New Chat
2. Prompt:
   ```
   Generate a Documentation Standards document covering:
   
   ## Code Documentation
   - When inline comments are required
   - Docstring requirements for functions, classes, modules
   - Complex logic documentation
   
   ## API Documentation
   - Endpoint documentation format
   - Request/response examples
   - Error code documentation
   
   ## Architecture Documentation
   - When architecture docs must be updated
   - Diagram requirements
   - Decision log format
   
   ## User Documentation
   - Setup instructions
   - Feature documentation
   - Troubleshooting guides
   
   Get sample code in #file:server/app.py
   ```
3. Save to: `docs/governance/documentation-standards.md`
4. **Reflection**:
   - How do these standards improve knowledge sharing?
   - What documentation types are most critical for our team?
   - How can we ensure documentation stays current?

### Part E: Define Quality Gate Checkpoints
1. Create a New Chat
2. Prompt:
   ```
   Based on the governance documents we've created, define Quality Gate 
   Checkpoints that developers must pass at each workflow stage:
   
   ## Pre-Commit Gate
   - Self-review checklist
   - Standards compliance verification
   - Required evidence
   
   ## Pre-Review Gate
   - Documentation completeness
   - Test coverage requirements
   - Traceability requirements
   
   ## Pre-Merge Gate
   - Review approval criteria
   - Final compliance check
   - Audit evidence requirements
   
   For each gate, include:
   - Pass/fail criteria
   - Copilot prompts for verification
   - Evidence to capture for traceability
   ```
3. Save to: `docs/governance/quality-gates.md`
4. Prompt: `When are the 3 gates performed?`
4. **Reflection**:
   - How do quality gates enforce governance without CI/CD?
   - Which gates provide the most value?
   - How can we keep gates lightweight yet effective?

---

## **Exercise 2: Conversational Peer Review Workflows**

**Overview**: Use Copilot to facilitate structured, conversational peer reviews that promote learning and knowledge sharing.

### Part A: Structured Review Conversation
**Scenario**: You are reviewing a new feature added to the backend API.
1. Open `server/app.py` and select the `get_dogs` route
2. Create a New Chat
3. Start a review conversation:
   ```
   I'm conducting a peer review of #selection. Let's have a structured 
   conversation following our governance in 
   #file:docs/governance/review-governance.md
   
   First, what are the critical review areas I should focus on?
   ```
4. After response, continue:
   ```
   Let's examine each area systematically. Start with security - 
   what specific security concerns should I check?
   ```
5. Continue through each area:  
   - Code Quality
   - Testability
   - Performance
   - Documentation
6. Ask for summary:
   ```
   Summarize all findings in a structured review report following our 
   team standards.
   Save the report to: docs/reviews/get_dogs-review-YYYY-MM-DD.md
   ```
7. Review the report for completeness and clarity.

### Part B: Educational Review Feedback
**Scenario**: You need to provide feedback on a junior developer's code addition.
1. Open `server/app.py` and add a new route:
```python
@app.route('/api/dogs/<int:dog_id>/breed', methods=['GET'])
def get_dog_breed(dog_id: int) -> Response | tuple[Response, int]:
    dog = Dog.query.get(dog_id)
    if not dog:
        return jsonify({"error": "Dog not found"}), 404
    return jsonify({"breed": dog.breed.name})
```
2. Save the file
3. Create a New Chat
4. Select the new route code and prompt:
   ```
   I need to provide peer review feedback on #selection to a junior 
   developer. Following our governance in 
   #file:docs/governance/review-governance.md, generate:
   
   1. Issue identification with clear explanations of WHY each matters
   2. Educational examples showing correct vs. incorrect approaches
   3. Links to relevant sections in our standards docs
   4. Suggested learning resources
   5. Positive feedback on what they did well
   
   Make the tone constructive and mentoring-focused.
   ```
5. Review the feedback - it should be educational, not just critical
6. Ask follow-up:
   ```
   How can I explain the security issue in simpler terms with a 
   real-world analogy?
   ```

### Part C: Review Escalation Handling
**Scenario**: Two reviewers disagree on an implementation approach
1. Open app.py. Create a New Chat
2. Set up the scenario:
   ```
   Two reviewers have conflicting opinions on a code change:
   
   Reviewer A says: "Use direct SQLAlchemy queries - it's simpler"
   Reviewer B says: "Use repository pattern - it's more testable"
   
   The code in question: #file:server/app.py (get_dogs function)
   
   According to our governance in #file:docs/governance/review-governance.md 
   and standards in #file:docs/governance/coding-standards.md:
   
   1. What does our governance say about resolving this?
   2. Evaluate both approaches against our documented standards
   3. Provide a recommendation with reasoning
   4. Suggest compromise approaches if applicable
   ```
3. Review the mediation response
4. Use Copilot Agent to generate an escalation document:
   ```
   Create a Decision Log entry documenting:
   - The disagreement
   - Arguments from both sides
   - Standards considered
   - Final decision and reasoning
   - Action items
   
   Format: docs/governance/decisions/YYYY-MM-DD-query-pattern.md
   ```
5. Review the document for clarity.


### Part D: Cross-Team Review Coordination
**Scenario**: A change affects multiple teams and requires coordinated review.
1. Create a New Chat
2. Prompt:
   ```
   Generate a Cross-Team Review Request template for when changes 
   affect multiple teams. Include:
   
   ## Change Summary
   - What changed
   - Why it changed
   - Which teams are affected
   
   ## Review Requirements
   - What each team should review
   - Specific concerns for each team
   - Timeline and deadlines
   
   ## Communication Plan
   - How to coordinate feedback
   - Meeting requirements
   - Decision-making process
   
   ## Documentation Updates Required
   - Which docs need updates
   - Who is responsible
   
   Reference our governance in #file:docs/governance/change-management.md
   ```
3. Save to: `docs/governance/templates/cross-team-review-request.md`
4. Review and refine the template for clarity

### Part E: Review Retrospective Generation
**Scenario**: After multiple reviews, generate a retrospective to analyze review effectiveness.
1. After completing several reviews, prompt:
   ```
   Based on review documents in docs/reviews/, generate a Review 
   Retrospective that analyzes:
   
   ## Common Issues Found
   - Most frequent problems
   - Root causes
   - Trends over time
   
   ## Review Process Effectiveness
   - Average review time
   - Issues caught vs. missed
   - Feedback quality
   
   ## Improvement Opportunities
   - Process improvements
   - Training needs
   - Tool enhancements
   
   ## Governance Updates Needed
   - Standards to clarify
   - Process changes to consider
   
   ## Action Items
   - Prioritized improvements
   - Owners and timelines

   Save to: docs/governance/retrospectives/YYYY-MM-review-retro.md
   ```
2. Review the retrospective for actionable insights

**Reflection:**
- How does structured conversation improve review quality?
- What elements make peer review feedback more actionable?
- How can retrospectives drive continuous improvement?

---

## **Exercise 3: Auto-Documentation and Knowledge Sharing**

**Overview**: Leverage Copilot to automatically generate and maintain various types of documentation, ensuring knowledge is captured and shared.

### Part A: Automated API Documentation Generation
**Scenario**: You need to generate comprehensive API documentation for the backend.
1. Create a New Chat
2. Prompt:
   ```
   @workspace Analyze all routes in #file:server/app.py and generate 
   comprehensive API documentation in OpenAPI 3.0 format including:
   
   - All endpoints with methods
   - Request parameters and body schemas
   - Response schemas with examples
   - Error codes and messages
   - Authentication requirements (if any)
   
   Follow documentation standards in 
   #file:docs/governance/documentation-standards.md
   ```
3. Review the generated documentation
4. Save to: `docs/api/openapi.yaml`
5. Ask follow-up:
   ```
   Generate a human-readable API Guide (markdown) from the OpenAPI spec 
   with examples and common use cases
   ```
6. Save to: `docs/api/api-guide.md`
7. Review both documents.

### Part B: Architecture Documentation Generation
**Scenario**: You need to create architecture documentation for the system.
1. Create a New Chat
2. Prompt:
   ```
   @workspace Analyze the entire codebase and generate Architecture 
   Documentation including:
   
   ## System Overview
   - High-level architecture
   - Key components and their responsibilities
   - Technology stack
   
   ## Backend Architecture
   - Folder structure explanation
   - Data models and relationships
   - API layer organization
   
   ## Frontend Architecture
   - Component hierarchy
   - State management approach
   - Routing structure
   
   ## Data Flow
   - Request/response flow
   - Database interactions
   - External dependencies
   
   ## Deployment Architecture
   - Environment setup
   - Dependencies
   - Configuration management
   
   Reference code patterns and follow standards in 
   #file:docs/governance/documentation-standards.md
   ```
3. Review and refine the documentation
4. Save to: `docs/architecture/system-architecture.md`

### Part C: Code-to-Documentation Sync Check
**Scenario**: Ensure documentation stays in sync with code changes.
1. Add a new route:
```python
@app.route('/api/dogs/<int:dog_id>/adopt', methods=['POST'])
def adopt_dog(dog_id: int) -> Response | tuple[Response, int]:
    dog = Dog.query.get(dog_id)
    if not dog:
        return jsonify({"error": "Dog not found"}), 404
    
    if dog.status == 'ADOPTED':
        return jsonify({"error": "Dog is already adopted"}), 400
    
    dog.status = 'ADOPTED'
    db.session.commit()
    
    return jsonify({"message": f"Dog {dog.name} has been adopted."})
```
2. Save the file. Create a New Chat
3. Prompt:
   ```
   @workspace Perform a documentation audit:
   
   1. Compare all routes in #file:server/app.py with #file:docs/api/api-guide.md
   2. Identify undocumented endpoints
   3. Find outdated documentation
   4. Check if response examples match actual code
   5. Verify error codes are documented
   
   Generate a Documentation Sync Report with:
   - Missing documentation items
   - Outdated sections needing updates
   - Inconsistencies between code and docs
   - Recommended actions
   ```
3. Review the sync report
4. For each discrepancy found, ask:
   ```
   Generate  documentation for adopt_dog in #file:api-guide.md and #file:openapi.yaml that matches the current documentation for each.
   ```
5. Check if the documents have been updated correctly.

### Part D: Inline Documentation Enhancement
**Scenario**: Improve inline code documentation for better maintainability.
1. Open `server/app.py`.
2. Select the whole function adopt_dog (do not include the route above).
3. Click Ctrl+I to open Copilot Chat Inline.
4. Prompt:
   ```
   Enhance the documentation following Google-style 
   docstring standards from #file:docs/governance/documentation-standards.md
   
   Include:
   - Comprehensive description
   - Args with types and descriptions
   - Returns with type and description
   - Raises with exception types
   - Example usage
   - Notes on complex logic
   ```
5. Apply the generated docstring
6. Run a workspace-wide check via Chat:
   ```
   @workspace Identify all functions in server/ directory missing or 
   having incomplete docstrings. Prioritize by function complexity and 
   public API exposure.
   ```

### Part E: Knowledge Base Article Generation
**Scenario**: Create a knowledge base article explaining a new feature.
1. Create a New Chat. 
2. Select the new adopt_dog route.
3. Prompt:
   ```
   Generate a Knowledge Base article explaining the Dog Adoption feature #selection:
   
   ## Overview
   - What the feature does
   - Why it exists
   
   ## User Perspective
   - How users interact with it
   - Step-by-step workflows
   
   ## Technical Implementation
   - Code components involved (#file:server/app.py routes)
   - Database models used
   - API endpoints
   
   ## Common Issues & Troubleshooting
   - Known edge cases
   - Error scenarios and resolutions
   
   ## Maintenance Notes
   - What to check during updates
   - Related tests in #file:server/test_app.py
   
   Write for both technical and non-technical audiences with separate 
   sections as needed.
   ```
4. Save to: `docs/knowledge-base/dog-adoption-feature.md`

### Part F: Automated Change Documentation
**Scenario**: Document code changes automatically as part of the development workflow.
1. Create a feature branch: `git switch -c feature/dog-adoption`
2. Stage the change: `git add server/app.py`
3. Generate a diff: `git diff --cached > recent-change.diff`
4. Create a New Chat
5. Prompt:
   ```
   Based on #file:recent-change.diff, generate:
   
   1. **Change Summary**: What changed and why
   2. **Impact Analysis**: What systems/features are affected
   3. **Breaking Changes**: Any API changes requiring updates
   4. **Migration Guide**: Steps for users/developers to adapt
   5. **Documentation Updates Required**: Which docs need updating
   6. **Testing Notes**: What should be tested
   
   Format as a Change Log entry following our governance standards
   ```
6. Save to: `docs/changelog/YYYY-MM-DD-change-description.md`
7. Review the change documentation for completeness.

**Reflection:**
- How does automated documentation reduce manual effort?
- What documentation types benefit most from AI generation?
- How can we ensure generated documentation stays current?
---

## **Exercise 4: Applying Quality Gates**

**Overview**: Implement quality gates at key workflow stages to enforce governance without CI/CD tools.

### Part A: Comprehensive Commit Summary Generation
**Scenario**: You are preparing to commit a new feature and need to ensure it meets quality standards.
1. **Execute Pre-Commit Quality Gate**:
   ```
   Execute Pre-Commit Quality Gate on #file:recent-change.diff:
   
   Verify against #file:docs/governance/quality-gates.md:
   1. Code follows #file:docs/governance/coding-standards.md
   2. Inline documentation complete
   3. No security vulnerabilities
   4. No TODO/FIXME without linked issues
   
   Provide GO/NO-GO recommendation with evidence.
   ```
2. Address any NO-GO items before proceeding
3. Save gate evidence to: `docs/governance/audit-trail/YYYY-MM-DD-pre-commit-gate.md`
3. Generate a commit message using the recent-change.diff. Prompt:
   `@workspace Generate a meaningful commit message based on the file #recent-change.diff using the standards in #file:documentation-standards.md`
4. Click Ctrl+Shift+G to open source control and paste the generated commit message and press Commit.
5. Push the branch: `git push -u origin feature/dog-adoption`


### Part B: Comprehensive PR Summary Generation
**Scenario**: You need to create a pull request with a comprehensive summary that meets governance standards.
1. Use the existing `feature/dog-adoption`.
2. Generate a diff: `git diff main...feature/dog-adoption > feature-pr.diff`
3. Create a New Chat
4. Prompt:
   ```
   Generate a comprehensive PR summary for #file:feature-pr.diff 
   following #file:pull-request-template.md
   
   Fill in all sections with specific details from the diff.
   
   Reference our standards in #file:.github/copilot-instructions.md and 
   #file:docs/governance/documentation-standards.md
   ```
5. Review and refine the summary
6. Save to: `docs/pr-summaries/PR-dog-adoption-summary.md`
7. Create the PR using the generated summary:
   - PowerShell:
     ```powershell
     gh pr create --title "feat: Add dog adoption feature" --body-file docs/pr-summaries/PR-dog-adoption-summary.md --base main
     ```
   - Bash:
     ```bash
     gh pr create --title "feat: Add dog adoption feature" --body-file docs/pr-summaries/PR-dog-adoption-summary.md --base main
     ```
8. Verify the PR is created on GitHub. See if the summary is well formatted.
9. Get the PR number and store it for later use:
   - PowerShell:
     ```powershell
     $prNumber = gh pr view --json number --jq '.number'
     ```
   - Bash:
     ```bash
     prNumber=$(gh pr view --json number --jq '.number')
     ```
10. Rename your summary file with the PR number:
    - PowerShell:
      ```powershell
      Move-Item docs/pr-summaries/PR-dog-adoption-summary.md "docs/pr-summaries/PR-$prNumber-summary.md"
      ```
    - Bash:
      ```bash
      mv docs/pr-summaries/PR-dog-adoption-summary.md "docs/pr-summaries/PR-$prNumber-summary.md"
      ```
11. Get the PR diff from GitHub:
    - PowerShell/Bash:
      ```bash
      gh pr diff $prNumber > feature-pr.diff
      ```
12. **Execute Pre-Review Quality Gate**:
    ```
    Execute Pre-Review Quality Gate for PR #$prNumber based on 
    #file:feature-pr.diff:
    
    Verify against #file:docs/governance/quality-gates.md:
    1. PR description follows template completely
    2. All documentation updated
    3. Tests exist for new functionality
    4. Self-review checklist completed
    5. No merge conflicts with main
    
    Provide GO/NO-GO recommendation with traceability evidence.
    ```
13. Address any NO-GO items before requesting review
   - Note: For our exercise, we will assume the gate is passed.
14. Save gate evidence to: `docs/governance/audit-trail/YYYY-MM-DD-pre-review-gate.md`
15. Request reviewers for the PR [READ-ONLY]:
    - PowerShell/Bash:
      ```bash
      gh pr edit $prNumber --add-reviewer <username1>,<username2>
      ```
    - Or via GitHub UI: Navigate to the PR → click "Reviewers" → select team members

### Part C: PR Summary for Different Audiences
**Scenario**: Tailor the PR summary for different stakeholders.
1. Get the PR diff from GitHub:
   - Using GitHub CLI: `gh pr diff $prNumber > feature-pr.diff`
2. Using the PR diff, prompt (add the pr-summaries folder as context):
   ```
   Create three versions of the PR summary for different audiences:
   
   ## For Technical Reviewers
   - Deep technical details
   - Code quality considerations
   - Architecture implications
   
   ## For Product/Business Stakeholders
   - User-facing changes
   - Business value
   - Risks and mitigations
   - No technical jargon
   
   ## For Future Developers (Onboarding)
   - Context on why changes were made
   - How this fits into larger system
   - Related code to understand
   - Key concepts introduced
   
   Base on #file:feature-pr.diff
   ```
2. Save all three versions to pr-summaries folder with appropriate filenames (if the agent hasn't done so already)

### Part D: PR Decision Log
**Scenario**: Document key decisions made during the PR.
1. Create a New Chat
2. Prompt:
   ```
   For the changes in #file:feature-pr.diff, generate a Decision Log 
   documenting:
   
   ## Problem Statement
   - What problem were we solving?
   - Why now?
   
   ## Options Considered
   - Approach 1 with pros/cons
   - Approach 2 with pros/cons
   - Approach 3 with pros/cons (if applicable)
   
   ## Decision Made
   - Which approach was chosen
   - Why it was selected
   - Trade-offs accepted
   
   ## Implementation Notes
   - Key technical choices
   - Patterns established
   - Standards followed
   
   ## Future Considerations
   - What might change later
   - Potential improvements
   - Technical debt introduced (if any)
   ```
3. Save to: `docs/governance/decisions/YYYY-MM-DD-decision-name.md`

### Part E: Cross-Reference PR to Documentation
**Scenario**: Ensure all relevant documentation references the new PR.
1. Create a New Chat and **add the docs folder as context**.
2. Prompt:
   ```
   For the merged PR documented in #file:feature-pr.diff:
   
   1. Identify which documentation files should reference this PR:
      - API docs
      - Architecture docs
      - Knowledge base articles
      - Governance docs
   
   2. Generate the cross-reference text for each document:
      - Where the reference should be added
      - What text to add
      - Why this cross-reference is valuable
   
   3. Create a Documentation Update Checklist:
      - [ ] File to update
      - [ ] Section to modify
      - [ ] Text to add
      - [ ] Reason for update
   ```
3. Use the checklist to update all relevant documentation (focus on high and medium priority items)

**Reflection:**
- How do comprehensive PR summaries improve knowledge retention?
- Which audience benefits most from different summary formats?
- How can PR knowledge be systematically captured for future use?

### Part F: Execute Pre-Merge Quality Gate
**Scenario**: Final gate before merging approved PR.
1. After receiving all required approvals, gather PR information via terminal:
   ```bash
   # Get comprehensive PR details and save to file
   gh pr view $prNumber --json number,title,body,state,reviews,reviewDecision,comments,mergeable,additions,deletions,changedFiles > pr-details.json
   
   # Get PR diff
   gh pr diff $prNumber > pre-merge-pr.diff
   ```
2. Create a New Chat
3. Prompt:
   ```
   Execute Pre-Merge Quality Gate for PR based on #file:pr-details.json 
   and #file:pre-merge-pr.diff:
   
   Verify against #file:docs/governance/quality-gates.md:
   1. All previous gates passed (check docs/governance/audit-trail/)
   2. Required approvals received (check reviewDecision in pr-details.json)
   3. All review comments resolved
   4. Documentation cross-references updated
   5. Traceability complete (code → requirements → tests → docs)
   6. No outstanding security or performance concerns
   
   Generate final audit certificate with:
   - Gate passage summary
   - Evidence collected
   - Compliance confirmation
   - Approval to merge
   ```
4. Address any NO-GO items (assume for exercise that gate is passed)
5. Save to: `docs/governance/audit-trail/YYYY-MM-DD-pre-merge-gate.md`
6. Merge the PR:
   ```bash
   gh pr merge $prNumber --merge
   ```
7. Clean up temporary files:
   - PowerShell: `Remove-Item pr-details.json, pre-merge-pr.diff`
   - Bash: `rm pr-details.json pre-merge-pr.diff`

**Reflection:**
- How do quality gates improve traceability without CI/CD?
- What evidence is most valuable for audit purposes?
- How can gates remain lightweight yet effective?
---

## **Exercise 5: End-to-End Quality Workflow**

**Overview**: Execute a complete refactor → review → document workflow demonstrating Copilot's role in quality governance.

### Part A: Identify Refactoring Candidate
**Scenario**: Analyze the codebase to find a suitable refactoring candidate.
1. Create a New Chat.
2. Prompt:
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
3. Select one candidate based on the recommendations (change app.py)
4. Add the governance folder as context then Prompt:
   ```
   Create a Refactoring Proposal document for app.py 
   including:
   - Current state assessment
   - Proposed improvements
   - Success criteria
   - Testing strategy
   - Documentation requirements
   
   Look at governance documents for guidance.
   Save to: docs/governance/change-proposals/YYYY-MM-DD-proposal-name.md
   ```
5. Review and refine the proposal if needed.

### Part B: Execute Refactoring with Governance
**Scenario**: Refactor the selected code while adhering to governance standards.
1. Create a feature branch: `git switch -c refactor/app-py-standards`
2. Open the file to be refactored (app.py)
3. Create a New Chat in Agent mode.
4. Prompt:
   ```
   Refactor #file:server/app.py based on the proposal in 
   #file:docs/governance/change-proposals/YYYY-MM-DD-proposal-name.md
   
   Apply the proposed improvements according to:
   - Standards in #file:.github/copilot-instructions.md
   - Coding standards in #file:docs/governance/coding-standards.md
   - Documentation standards in #file:docs/governance/documentation-standards.md
   
   For each change from the proposal:
   1. Reference which proposed improvement you're addressing
   2. Explain what you're changing and why
   3. Show before and after code
   4. Confirm it meets governance requirements and success criteria
   
   Start with the most critical improvements from the proposal first.
   ```
5. Review each suggested change
6. Accept the changes.
7. When asked if you would like to modify the tests, respond with:
   `Add missing tests. Add edge cases, boundary cases, contraints in the tests`


### Part C: Automated Review of Refactored Code
**Scenario**: Conduct an automated code review of the refactored code to ensure quality.
1. Ensure refactored code is saved
2. Create a code-reviewer agent.
   - Select Cog -> Custom Agents -> + Create new custom agent -> .github\agents
   - Name it: code-reviewer
   - Copy and paste the content below into the new agent.md file.
``` markdown
---
description: "Comprehensive Code Reviewer for GitHub Copilot – analyzes codebases for security, performance, quality, and testing issues, referencing supplemental checklists."
tools: ["search", "problems", "fetch", "githubRepo", "edit"]
model: Claude Sonnet 4.5
---

# Comprehensive Code Review Agent

## Role
You are an expert code reviewer and analyst specializing in Python/Flask applications and modern frontend frameworks. Your goal is to perform deep, automated code analysis and provide precise, actionable feedback on security, performance, code quality, testing, and documentation.

## Responsibilities
- Conduct thorough analysis across multiple files and modules.
- Identify patterns, inconsistencies, and architectural issues.
- Detect security vulnerabilities and performance bottlenecks.
- Evaluate code quality, maintainability, and standards compliance.
- Review test coverage and identify gaps or missing edge cases.
- Check documentation for completeness and accuracy.
- Provide actionable recommendations prioritized by severity and impact.
- Use supplemental reference files `coding-standards.md` to guide findings.

## Reference Files
- `coding-standards.md` – coding standards for quality, performance, and testing

## Analysis Areas
1. **Security**: Authentication, authorization, input validation, SQL injection risks, sensitive data exposure.
2. **Performance**: N+1 queries, inefficient loops, missing database indexes, slow algorithms.
3. **Code Quality**: Duplication, cyclomatic complexity, adherence to coding standards.
4. **Testing**: Coverage gaps, missing edge cases, inconsistent test patterns.
5. **Documentation**: Missing or outdated docstrings, outdated API references, unclear usage instructions.

## Output Format (Structured Findings)
Each finding must include:
- **Category**: Security / Performance / Quality / Testing / Documentation
- **Severity**: Critical / High / Medium / Low
- **Location**: File path and line numbers (or nearest function/block)
- **Issue**: Clear and concise description
- **Impact**: Explanation of why it matters
- **Recommendation**: Specific fix or improvement
- **Examples**: Minimal code snippet illustrating the problem

## Search & Analysis Strategy
1. Begin with broad workspace scans.
2. Focus on high-risk or suspicious areas first.
3. Cross-reference related files and **the supplemental reference file `coding-standards.md`** for patterns, standards, and best practices.
4. Verify that issues are consistent across the codebase.
5. Prioritize findings that are actionable and high-impact.
6. Suggest tests or CI checks to prevent future regressions.

---

```
3. Save the agent file.
4. Open Copilot Chat in `code-reviewer` agent mode. Prompt:
   ```
   @workspace Perform a comprehensive review of the refactored code in 
   #file:app.py against:
   
   - #file:.github/copilot-instructions.md
   - #file:docs/governance/coding-standards.md
   
   Verify:
   1. All standards are met
   2. Code quality improvements achieved
   3. No regressions introduced
   4. Documentation is complete
   5. Tests are adequate
   
   Generate a Quality Assurance Report.
   ```
5. Address any issues found
6. Re-review until all criteria are met
7. Save the final report to: `docs/reviews/app-py-refactor-review-YYYY-MM-DD.md`

### Part D: Generate Multi-Perspective Documentation
**Scenario**: Create various documentation artifacts from the refactored code.
1. Create a New Chat in Agent mode.
2. Generate knowledge sharing documentation:
   ```
   Create a Refactoring Case Study from the refactoring work in 
   #file:app.py for team learning:
   
   ## Before State
   - What issues existed
   - Why refactoring was needed
   
   ## Refactoring Process
   - Approach taken
   - Standards applied
   - Challenges faced and solutions
   
   ## After State
   - Improvements achieved
   - Quality metrics improved
   - Maintainability gains
   
   ## Lessons Learned
   - What worked well
   - What to do differently next time
   - Patterns to reuse
   
   ## Reusable Techniques
   - Refactoring patterns applied
   - Standards validated
   - Tools used
   ```
3. Save to: `docs/knowledge-base/refactor-app.md`

### Part E: Execute Pre-Commit Quality Gate and Commit
**Scenario**: Execute the Pre-Commit Quality Gate before committing the refactored code.
1. Stage all changes: `git add server/app.py server/test_app.py`
2. Generate a diff: `git diff --cached > recent-change.diff`
3. **Execute Pre-Commit Quality Gate** (first gate from `quality-gates.md`):
   - Create a New Chat
   - Prompt:
   ```
   Execute Pre-Commit Quality Gate on #file:recent-change.diff:
   
   Verify against #file:docs/governance/quality-gates.md (Pre-Commit Gate):
   1. Code follows #file:docs/governance/coding-standards.md
   2. Inline documentation complete
   3. No security vulnerabilities
   4. No TODO/FIXME without linked issues
   5. Standards compliance verification
   
   Provide GO/NO-GO recommendation with evidence for each criterion.
   ```
4. Address any NO-GO items before proceeding
5. Save gate evidence to: `docs/governance/audit-trail/YYYY-MM-DD-refactor-pre-commit-gate.md`
6. Generate a changelog entry. Create a New Chat and add the changelog folder as context:
   ```
   Based on #file:recent-change.diff, generate:
   
   1. **Change Summary**: What changed and why
   2. **Impact Analysis**: What systems/features are affected
   3. **Breaking Changes**: Any API changes requiring updates
   4. **Migration Guide**: Steps for users/developers to adapt
   5. **Documentation Updates Required**: Which docs need updating
   6. **Testing Notes**: What should be tested
   
   Format as a Change Log entry following our governance standards
   ```
7. Save to: `docs/changelog/YYYY-MM-DD-change-description.md`
8. Generate a commit message using the recent-change.diff. Prompt:
   `@workspace Generate a meaningful commit message based on the file #recent-change.diff using the standards in #file:documentation-standards.md`
9. Click Ctrl+Shift+G to open source control and paste the generated commit message and press Commit.
10. Push the branch: `git push -u origin refactor/app-py-standards`

### Part F: Create PR Package and Execute Pre-Review Quality Gate
**Scenario**: Create the PR and execute the Pre-Review Quality Gate before requesting reviewers.
1. Generate diff from main: `git diff main...refactor/app-py-standards > refactor-pr.diff`
2. Create a new Chat in Agent and add the pr-summaries folder as context:
   ```
   Generate a comprehensive PR summary for #file:refactor-pr.diff 
   following #file:pull-request-template.md
   
   Fill in all sections with specific details from the diff.
   
   Reference our standards in #file:.github/copilot-instructions.md and 
   #file:docs/governance/documentation-standards.md
   ```
3. Save to: `docs/pr-summaries/PR-refactor-app-summary.md`
4. Create the PR using the generated summary:
   - PowerShell:
     ```powershell
     gh pr create --title "refactor: Improve app.py standards compliance" --body-file docs/pr-summaries/PR-refactor-app-summary.md --base main
     ```
   - Bash:
     ```bash
     gh pr create --title "refactor: Improve app.py standards compliance" --body-file docs/pr-summaries/PR-refactor-app-summary.md --base main
     ```
5. Verify the PR is created on GitHub. Check if the summary is well formatted.
6. Get the PR number and store it for later use:
   - PowerShell:
     ```powershell
     $prNumber = gh pr view --json number --jq '.number'
     ```
   - Bash:
     ```bash
     prNumber=$(gh pr view --json number --jq '.number')
     ```
7. Rename your summary file with the PR number:
   - PowerShell:
     ```powershell
     Move-Item docs/pr-summaries/PR-refactor-app-summary.md "docs/pr-summaries/PR-$prNumber-refactor-summary.md"
     ```
   - Bash:
     ```bash
     mv docs/pr-summaries/PR-refactor-app-summary.md "docs/pr-summaries/PR-$prNumber-refactor-summary.md"
     ```
8. Get the PR diff from GitHub:
   ```bash
   gh pr diff $prNumber > refactor-pr.diff
   ```
9. **Execute Pre-Review Quality Gate** (second gate from `quality-gates.md`):
   - Create a New Chat
   - Prompt (modify with your prNumber):
   ```
   Execute Pre-Review Quality Gate for PR #$prNumber based on 
   #file:refactor-pr.diff:
   
   Verify against #file:docs/governance/quality-gates.md (Pre-Review Gate):
   1. PR description follows template completely
   2. Documentation completeness verified
   3. Test coverage requirements met
   4. Traceability requirements satisfied
   5. All documentation updated
   6. Self-review checklist completed
   7. No merge conflicts with main
   
   Provide GO/NO-GO recommendation with traceability evidence.
   ```
10. Address any NO-GO items before requesting review (assume for exercise that gate is passed)
11. Save gate evidence to: `docs/governance/audit-trail/YYYY-MM-DD-refactor-pre-review-gate.md`
12. Request reviewers for the PR [READ-ONLY]:
    - PowerShell/Bash:
      ```bash
      gh pr edit $prNumber --add-reviewer <username1>,<username2>
      ```
    - Or via GitHub UI: Navigate to the PR → click "Reviewers" → select team members
13. [Optional] Perform the following tasks:
   - 4.B PR Summary for Different Audiences
   - 4.C PR Decision Log
   - 4.D Cross-Reference PR to Documentation

### Part G: Execute Pre-Merge Quality Gate (Optional)
**Scenario**: After receiving review approval, execute the final quality gate before merging.
1. **Execute Pre-Merge Quality Gate** (third gate from `quality-gates.md`):
   - Create a New Chat
   - Prompt (modify with your prNumber):
   ```
   Execute Pre-Merge Quality Gate for PR #$prNumber:
   
   Verify against #file:docs/governance/quality-gates.md (Pre-Merge Gate):
   1. Review approval criteria met
   2. Final compliance check passed
   3. Audit evidence requirements satisfied
   4. All review comments addressed
   5. CI/CD checks passed (if applicable)
   
   Provide GO/NO-GO recommendation with final audit evidence.
   ```
2. Address any NO-GO items before proceeding
3. Save gate evidence to: `docs/governance/audit-trail/YYYY-MM-DD-refactor-pre-merge-gate.md`

**Reflection:**
- How does an end-to-end quality workflow enhance code reliability?
- What governance elements were most critical in this process?
- How do the three quality gates (Pre-Commit, Pre-Review, Pre-Merge) work together?
- How can this workflow be adapted for larger teams or projects?

---

### Success Criteria
- [ ] Feature fully implemented following all governance standards
- [ ] All three quality gates executed (Pre-Commit, Pre-Review, Pre-Merge)
- [ ] All governance compliance checks passed with documented evidence
- [ ] Comprehensive documentation created and validated
- [ ] Multi-perspective review completed (code, security, performance)
- [ ] Knowledge artifacts generated and shared
- [ ] Governance process validated and improved
- [ ] Audit trail complete with all gate evidence files

**Final Reflection:**
1. How did Copilot streamline the governance workflow?
2. Which governance processes provided the most value?
3. What governance improvements emerged from this exercise?
4. How would you adapt this workflow for your organization?
5. What metrics best demonstrate governance effectiveness?
6. How do the quality gates ensure consistent code quality?

---

## **[OPTIONAL] Exercise 6: Governance Automation with GitHub MCP Server**

**Overview**: Leverage the GitHub MCP Server to automate governance checks, retrieve PR metadata, and enforce quality gates programmatically within Copilot workflows.

---

### Prerequisites
1. Search for "@mcp GitHub" in the VS Code Extensions marketplace and install it.
2. Once installed, Check if the MCP Server is installed.
3. Select Settings (gear icon) → Show Configuration (JSON)
4. Click the Start MCP Server button.
5. You will be prompted to authenticate with GitHub. Follow the instructions to authorize.
6. Verify MCP Server is running by seeing the Status as Running.
7. Restart VS Code to ensure MCP integration is active.
8. In Agent mode, click tools icon and ensure "GitHub MCP" is listed among available tools.
---

### Part A: Automated PR Governance Check
**Scenario**: Use MCP to fetch PR details and validate against governance criteria.
1. Open Copilot Chat in Agent mode
2. Prompt:
   ```
   Use the GitHub MCP server to fetch open PRs in the pets-workshop repository. 
   For each PR, check:
   - Does the title follow conventional commit format (feat:, fix:, refactor:, etc.)?
   - Is there a description with at least 50 characters?
   - Are there any requested reviewers assigned?
   
   Report compliance status for each PR in a table format.
   ```
3. Review the automated governance report
4. Save findings to: `docs/governance/mcp-reports/pr-governance-check-YYYY-MM-DD.md`

### Part B: Label-Based Quality Gate Enforcement
**Scenario**: Automatically apply governance labels based on PR content analysis.
1. Get your PR number of the recent refactor PR created in Exercise 5.
2. Create a New Chat in Agent mode
3. Prompt (replace with your PR number):
   ```
   Using GitHub MCP, analyze PR #[number] in the pets-workshop repository:
   
   1. Fetch the PR details and changed files
   2. Analyze if documentation was updated for code changes
   3. Check if tests exist for new functionality
   4. Review if the PR description follows our template
   
   Based on analysis, suggest appropriate labels:
   - "needs-docs" if documentation is missing for new features
   - "needs-tests" if test coverage appears insufficient  
   - "ready-for-review" if all governance criteria are met
   - "breaking-change" if API changes detected
   
   Apply the suggested labels to the PR.
   ```
4. Verify labels were applied on GitHub

### Part C: Automated Review Request Assignment
**Scenario**: Intelligently assign reviewers based on changed files.
1. Create a New Chat in Agent mode
2. Prompt:
   ```
   Use GitHub MCP to analyze PR #[number] in pets-workshop:
   
   1. Get the list of files changed in the PR
   2. For each file type, suggest appropriate reviewers:
      - Python files (server/) → backend team
      - Frontend files (src/) → frontend team
      - Configuration files → DevOps team
      - Documentation → tech writers
   
   3. Check the repository's recent commit history to identify 
      contributors who have worked on similar files
   
   4. Generate a reviewer recommendation report with reasoning
   ```
3. Review the recommendations
4. Save to: `docs/governance/mcp-reports/reviewer-assignment-YYYY-MM-DD.md`

### Part D: Governance Dashboard Generation
**Scenario**: Generate a governance status report for all open PRs.
1. Create a New Chat in Agent mode
2. Prompt:
   ```
   Using GitHub MCP, create a comprehensive governance dashboard for 
   the pets-workshop repository:
   
   ## Open PRs Status
   - List all open PRs with: number, title, author, age (days open), labels
   - Flag PRs open longer than 7 days as "stale"
   
   ## Review Status
   - PRs awaiting review (no reviews yet)
   - PRs with requested changes
   - PRs approved and ready to merge
   
   ## Governance Compliance
   - PRs missing required labels
   - PRs without linked issues (if your workflow requires them)
   - PRs with merge conflicts
   
   ## Metrics Summary
   - Average PR age
   - Average time to first review
   - PRs merged this week
   
   Format as a markdown dashboard with tables and status indicators.
   ```
3. Save to: `docs/governance/mcp-reports/pr-dashboard-YYYY-MM-DD.md`
4. Consider scheduling this as a regular governance check

### Part E: Issue-to-PR Traceability Check
**Scenario**: Ensure all PRs have proper issue linkage for traceability.
1. Create a New Chat in Agent mode
2. Prompt:
   ```
   Using GitHub MCP, perform a traceability audit:
   
   1. Fetch all open PRs in pets-workshop
   2. For each PR, check:
      - Is there a linked issue in the description (Fixes #, Closes #, Resolves #)?
      - Does the PR title reference an issue number?
      - Are there any issue cross-references in commits?
   
   3. For PRs without issue links:
      - Search for related open issues based on PR title/description
      - Suggest potential issue matches
   
   4. Generate a Traceability Report:
      - PRs with proper issue linkage ✅
      - PRs missing issue linkage ⚠️
      - Suggested issue associations
      - Compliance percentage
   
   This supports our governance requirement for change traceability.
   ```
3. Save to: `docs/governance/mcp-reports/traceability-audit-YYYY-MM-DD.md`

### Part F: Automated Stale PR Management
**Scenario**: Identify and manage stale PRs to maintain repository health.
1. Create a New Chat in Agent mode
2. Prompt:
   ```
   Using GitHub MCP, implement stale PR management:
   
   1. Find all PRs that have been open for more than 14 days
   2. For each stale PR:
      - Check last activity date (comments, commits, reviews)
      - Identify the author and reviewers
      - Determine if there are unresolved review comments
   
   3. Generate actions for each stale PR:
      - If no activity in 14+ days: Add "stale" label
      - If no activity in 30+ days: Add comment requesting status update
      - If blocked by review: Ping reviewers
   
   4. Create a Stale PR Report with:
      - List of stale PRs with last activity date
      - Recommended actions for each
      - Overall repository health metrics
   
   Apply the "stale" label to qualifying PRs.
   ```
3. Review actions taken
4. Save report to: `docs/governance/mcp-reports/stale-pr-report-YYYY-MM-DD.md`

---

## **Assessment Checklist**

After completing all exercises, you should be able to:

### Governance
- [ ] Establish lightweight governance standards and processes
- [ ] Create and maintain governance documentation
- [ ] Audit codebase for governance compliance
- [ ] Track governance metrics and trends
- [ ] Adapt governance based on lessons learned

### Peer Review
- [ ] Conduct structured, conversational code reviews
- [ ] Provide educational, constructive feedback
- [ ] Mediate review disagreements with data
- [ ] Facilitate productive review discussions
- [ ] Generate comprehensive review reports

### Documentation
- [ ] Automatically generate API documentation
- [ ] Create and maintain architecture documentation
- [ ] Ensure documentation stays synchronized with code
- [ ] Generate knowledge base articles
- [ ] Document decisions and lessons learned

### PR Management
- [ ] Generate comprehensive PR descriptions
- [ ] Create audience-specific PR summaries
- [ ] Extract knowledge from merged PRs
- [ ] Maintain PR-to-documentation cross-references

### End-to-End Workflows
- [ ] Execute complete refactor → review → document workflows
- [ ] Integrate governance into development processes
- [ ] Use multi-agent reviews for comprehensive quality checks
- [ ] Capture and share knowledge systematically

### Integration
- [ ] Integrate Copilot governance into GitHub workflows
- [ ] Automate governance compliance checks
- [ ] Collect and track governance metrics

### MCP Automation
- [ ] Configure and connect GitHub MCP Server
- [ ] Automate PR governance checks via MCP
- [ ] Apply labels programmatically based on analysis
- [ ] Generate governance dashboards from live repository data
- [ ] Perform traceability audits across PRs and issues
- [ ] Manage stale PRs with automated workflows

---

## **Best Practices Learned**

### Governance
1. **Keep It Lightweight**: Governance should enable, not hinder
2. **Document Standards Clearly**: Ambiguity leads to inconsistency
3. **Automate Where Possible**: Reduce manual compliance burden
4. **Measure and Adapt**: Use metrics to guide improvements
5. **Focus on Value**: Every governance process should add clear value

### Review
1. **Be Constructive**: Explain why, not just what's wrong
2. **Educate**: Use reviews as teaching opportunities
3. **Be Systematic**: Follow checklists for consistency
4. **Document Decisions**: Capture rationale for future reference
5. **Iterate**: Progressive refinement produces better results

### Documentation
1. **Automate Generation**: Let AI handle boilerplate
2. **Keep Synchronized**: Docs should reflect current code
3. **Multiple Audiences**: Tailor docs for different readers
4. **Link Everything**: Cross-references improve discoverability
5. **Version Control**: Track doc changes with code changes

### Knowledge Sharing
1. **Capture Continuously**: Don't wait for scheduled reviews
2. **Make Searchable**: Organize for easy discovery
3. **Encourage Reuse**: Patterns, templates, examples
4. **Celebrate Learning**: Share successes and failures
5. **Keep Current**: Regular updates maintain value

---

## **Next Steps**

After completing these exercises, consider:

1. **Organizational Adoption**
   - Present governance framework to leadership
   - Train team members on Copilot-assisted governance
   - Establish governance champions

2. **Continuous Improvement**
   - Schedule quarterly governance retrospectives
   - Update standards based on lessons learned
   - Refine automation based on team feedback

3. **Scaling**
   - Adapt governance for multiple teams/projects
   - Create organization-wide standards
   - Build shared knowledge repositories

4. **Advanced Integration**
   - Integrate with CI/CD pipelines
   - Connect to project management tools
   - Automate compliance reporting

5. **Metrics and ROI**
   - Track time savings from automation
   - Measure quality improvements
   - Demonstrate business value

6. **Community Building**
   - Share governance practices with wider community
   - Contribute to Copilot best practices
   - Learn from other organizations

---

## **Appendix: Quick Reference Commands**

### Governance Auditing
- `@workspace Audit codebase for compliance with #file:docs/governance/coding-standards.md`
- `@governance-auditor Check #file:<filename> for governance violations`
- `Generate governance compliance report for the last sprint`

### Documentation Generation
- `@workspace Generate API documentation for all endpoints in #file:server/app.py`
- `Create architecture documentation based on codebase structure`
- `Generate knowledge base article for [feature name]`

### PR Management
- `Generate comprehensive PR description using #file:pull-request-template.md based on #file:pr-changes.diff`
- `Create decision log for the changes in #file:feature-pr.diff`
- `Extract reusable knowledge from merged PR`

### Review Facilitation
- `Review #selection against all governance standards`
- `Provide educational feedback on #selection for junior developer`
- `Mediate disagreement between two review approaches`

### Knowledge Capture
- `Document lesson learned from [event] using #file:docs/governance/templates/lesson-learned-template.md`
- `Create case study from #file:refactor-pr.diff`
- `Update pattern library with patterns from #file:<filename>`

### Metrics and Reporting
- `Generate governance metrics dashboard with current compliance scores`
- `Create weekly governance compliance report`
- `Analyze governance trends over last quarter`

---

## **Resources**

### Templates Created
- Decision record template
- Lesson learned template
- Code review checklist
- Pull request template
- Governance audit template

### Agents Created
- governance-auditor: Automated compliance checking
- master-reviewer: Coordinated multi-perspective reviews

### Documentation Generated
- Coding standards
- Documentation standards
- Change management process
- Review governance
- Architecture documentation
- API documentation
- Pattern library

### Workflows Established
- Pre-commit governance checks
- Automated PR review comments
- Documentation sync validation
- Governance metrics collection
- End-to-end quality workflow

---