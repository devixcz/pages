# Contribution Guidelines

## Introduction
Thank you for contributing to this project! To ensure a clean and maintainable codebase, we follow a structured contribution process. This document outlines the best practices for writing commit messages, naming merge requests, and linking work to JIRA tickets.

---

## 1. Git Branching Model
Our branching model follows [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/), with the following primary shared branches:

- **main**: Corresponds to the production environment and should always be in a deployable state (often integrated with CI/CD tools).
- **staging**: Branched from `main`, this is a stable environment for User Acceptance Testing (UAT) before deploying to production.
- **develop**: The main development branch from which feature branches, bug fixes, and other updates are created. While not considered stable, only reviewed and tested changes should be merged here.
- **Feature branches**: Created from `develop` and used for individual features or bug fixes. These follow the naming convention `feature/<JIRA-TICKET-ID>-short-description` or `bug/<JIRA-TICKET-ID>-short-description`.

---

## 2. Branching Strategy
- Use feature branches for all development (`feature/<JIRA-TICKET-ID>-short-description`).
- For bug fixes: `fix/<JIRA-TICKET-ID>-short-description`.
- For hotfixes (urgent production issues): `hotfix/<JIRA-TICKET-ID>-short-description`.

**Examples:**
```
feature/JIRA-123-oauth2-login
fix/JIRA-456-pagination-bug
hotfix/JIRA-789-critical-db-error
```

---

## 3. Commit Messages (Conventional Commits)
All commit messages **must** follow the [Conventional Commits](https://www.conventionalcommits.org) format:

```
<type>(optional scope): <message>

[Optional body: more details about the commit]

[Optional references: Fixes JIRA-123, Refs JIRA-456]
```

### Allowed Commit Types
| Type       | Description |
|------------|-------------|
| `feat`     | Adds a new feature |
| `fix`      | Fixes a bug |
| `chore`    | Updates dependencies, build scripts, or CI/CD |
| `docs`     | Documentation updates only |
| `refactor` | Code changes that neither fix a bug nor add a feature |
| `perf`     | Performance improvements |
| `test`     | Adds or updates tests |
| `style`    | Formatting, missing semi-colons, etc. (no logic changes) |
| `ci`       | CI/CD configuration changes |

### Example Commit Messages
```
feat(auth): add OAuth2 login support

Implements OAuth2 login using Google and Facebook authentication.

Fixes JIRA-123
```
```
fix(api): resolve pagination issue

Pagination was returning duplicate results due to incorrect SQL offset.
Adjusted query logic to fix the issue.

Refs: JIRA-456
```

### Tools for Conventional Commits
To help developers follow the **Conventional Commits** format, various tools and plugins are available:

- **Commitlint**: Lints commit messages to enforce the standard ([GitHub](https://github.com/conventional-changelog/commitlint)).
- **VSCode Plugins**:
  - [Conventional Commits](https://marketplace.visualstudio.com/items?itemName=vivaxy.vscode-conventional-commits)
  - [Git Commit Message Editor](https://marketplace.visualstudio.com/items?itemName=KnisterPeter.vscode-commitizen)
- **Commitizen**: A CLI tool that guides developers to write proper commit messages ([GitHub](https://github.com/commitizen/cz-cli)).

---

## 4. Naming Merge Requests (MRs)
Merge request titles should follow the **Conventional Commits** format:

```
<type>(optional scope): <short description> [JIRA-XXX]
```

### Examples
```
feat(auth): add OAuth2 login flow [JIRA-123]
fix(api): resolve pagination issue [JIRA-456]
chore(deps): upgrade Node.js to 18.x [JIRA-789]
```

### Merge Request Description Template
When opening a merge request, follow this structure:
```md
## Summary
Shortly describe the purpose of this MR.

## Changes
- List significant changes made in this MR.

## How to Test
Steps to verify the changes.

## Related Issues
Closes JIRA-123

## Breaking Changes
Describe if any breaking changes were introduced.
```

---

## 5. Enforcing These Rules
To ensure consistency, the following measures should be taken:

- Enable **Git hooks** for commit message linting (`commitlint`).
- Use **GitLab CI/CD** to validate commit messages.
- Require all MRs to be reviewed before merging.
- Enable **JIRA integration** in GitLab for automatic issue tracking.

Example `.gitlab-ci.yml` configuration for commit linting:
```yaml
commitlint:
  image: node:18
  script:
    - npx commitlint --from=HEAD~10 --to=HEAD
  only:
    - merge_requests
```

# Development Process

## 1. Starting Work on a Feature or Bug
- The developer moves the ticket from **To-Do** to **In Progress** and assigns themselves to it.
- A new feature branch is created from the `develop` branch, following the naming conventions outlined in the Contribution Guidelines.

## 2. Development Phase
- The developer makes necessary code changes.
- Commits are made following **Conventional Commits** guidelines.
- The branch can have multiple commits as needed.

## 3. Preparing for Code Review
Before submitting a merge request, the developer must:
1. **Rebase** the branch onto the latest `develop` branch.
2. Run all available local tests (e.g., **ESLint, Unit Tests, Playwright tests** if applicable).
3. Manually test common feature scenarios (if extensive testing is required, this step can be left to testers, but a comment should be added to the ticket).
4. Create a **Merge Request (MR)** that adheres to the naming and description standards from the Contribution Guidelines.
5. **Squash commits** into a single commit before merging to `develop`.

## 4. Code Review Process
- The developer moves the ticket to **Code Review** and adds a comment linking the MR.
- Peer review is conducted by another developer.
- If the MR is approved, the author merges it into `develop` and ensures that CI/CD pipelines run successfully.
- **Developers should merge their own MRs**—no one else should merge on their behalf.

## 5. Testing on Development Environment
- After merging, the ticket moves to **Test on Dev**.
- The QA team or other team members perform basic functionality tests.
- If issues are found, the ticket is moved back to **To-Do**, assigned to the responsible developer, and they must handle the fix themselves.

## 6. Handling Returned Tickets
- If a ticket is sent back from **Code Review** or any **Testing** phase, it returns to **To-Do** with the assigned developer responsible for corrections.
- The developer applies fixes, repeats the testing process, and re-submits for review.

# Code Review Guidelines

## 1. Purpose of Code Review
Code reviews are a crucial part of maintaining high-quality code, ensuring maintainability, and sharing knowledge within the team. Every merge request (MR) must go through a review process before being merged.

---

## 2. Responsibilities of the Author
- Ensure the code follows project standards and is properly formatted.
- Run all tests locally before submitting the MR.
- Provide a **clear MR description** including:
  - Summary of changes
  - Steps to test
  - Related issues (e.g., `Closes JIRA-123`)
- Request review from at least one other developer.
- Address feedback promptly and keep discussions constructive.
- Squash commits if necessary before merging to maintain a clean history.

---

## 3. Responsibilities of the Reviewer
- Review the MR **thoroughly but efficiently**.
- Check if the code adheres to **Conventional Commits** and **coding standards**.
- Ensure **logic correctness, performance, and security best practices**.
- Verify that all **tests pass** and request additional tests if needed.
- Look for **duplicate or unnecessary code** that could be optimized.
- Provide clear and **constructive feedback**, including suggestions for improvement.
- Approve the MR only when it meets all quality standards.

---

## 4. Reviewing Checklist
### Code Quality
✅ Is the code **readable** and well-structured?
✅ Are function and variable names **meaningful**?
✅ Is the **code modular** and follows the **single responsibility principle**?

### Standards & Best Practices
✅ Does the code comply with our **coding conventions**?
✅ Are **security concerns** (e.g., authentication, validation, SQL injection) addressed?
✅ Does the code handle **edge cases and error conditions**?

### Testing & Performance
✅ Have unit tests been updated or added?
✅ Does the MR include **integration tests** where necessary?
✅ Are any **performance bottlenecks** introduced?
✅ Does the code pass **CI/CD pipelines** without failures?

---

## 5. Handling Review Feedback
- If changes are requested, the **author updates the code** and reassigns the MR to the reviewer.
- Keep discussions professional and focus on **code improvements**.
- If disagreements arise, discuss them with the team or a lead developer.
- Once approved, the **author merges the MR** (no one should merge someone else's MR).

---

## 6. Review Turnaround Time
- Reviewers should aim to provide feedback **within 24 hours**.
- If urgent, use team communication channels to request a faster review.
- Avoid **blocking the review process**—if unavailable, assign another reviewer.

# Environment Description

Our application operates in three primary environments, each corresponding to a shared Git branch. Every environment has its own URL, typically starting with a subdomain that matches the corresponding Git branch.

## 1. **Develop Environment**
- Corresponds to the `develop` branch.
- A shared working environment for developers.
- Anything deployed here is available for testing but is not considered complete and may be unstable.
- Uses its own database, which can be reset at any time.
  - The easiest way to fix database issues is often a complete reset.
- Should not use or share real resources (e.g., third-party APIs, mailing services), only development variants.
- Should not contain real user data.
- Should not be publicly accessible.
- Not meant for presentations to stakeholders (may experience outages).
- May use debugging tools.
- Used for **exploratory testing, smoke testing, and ad hoc testing**.

## 2. **Staging Environment**
- Corresponds to the `staging` branch.
- Should always be stable and fully functional.
- Used for presenting new features to stakeholders.
- Used for **UAT testing, regression testing, and release candidate testing**.
- Debugging tools are not allowed.
- May contain a snapshot of real data, but it should not be linked to production.
- The staging database is only reset in exceptional cases.

## 3. **Production Environment**
- Corresponds to the `main` branch, and nothing should be on `main` that is not deployed.
  - Theoretically, the `main` branch should always be deployable.
- The **production environment must be stable and thoroughly tested**.
- Contains **real user data, API credentials, etc.**, so appropriate security measures must be taken:
  - Do not allow unauthorized access to the database.
  - Do not create dumps of the production database.

## 4. **Optional: Feature Branch Environments**
- In some cases, it may be beneficial to have an environment for a specific feature, corresponding to a particular feature branch.
- Typically runs on a subdomain matching the Git branch name.
- If possible, it shares resources with the develop environment.
- In exceptional cases (e.g., incompatible database schema), it may have its own resources.
- Mainly used for iterative development of specific features.

# Deployment Process Across Environments

## 1. Deployment Sequence
- Every change, except for **hotfixes**, must be deployed in the following order:
  1. **Develop**
  2. **Staging**
  3. **Production**

## 2. Deployment Between Shared Branches
- **Pull requests (PRs) may be used** when deploying changes between shared branches, but they are **not required**.
- If PRs are used, **commits should not be squashed** (to ensure proper release notes generation).

## 3. Deployment from Develop to Staging
- Tested changes from `develop` can be deployed to `staging` **at the discretion of the developer**.
- It is recommended to **inform the team** when deploying to `staging`.

## 4. Deployment from Staging to Production
1. **Tag the current staging version** according to [Semantic Versioning](https://semver.org/).
2. If using a pull request for merging, the **auto-generated commit message** can be used as release notes.
3. **All changes in `main` must be deployed**. There should never be changes in `main` that are not deployed.