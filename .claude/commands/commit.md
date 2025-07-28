---
name: commit
description: Create atomic git commits with Conventional Commits format
---

Create atomic git commits following Conventional Commits specification:

1. Check current branch and warn if on main
2. Show git status and diff to understand all changes
3. **Analyze changes for atomic commits by type**:
   - **CRITICAL**: Split different commit types into separate atomic commits even within the same file
   - Group changes by logical purpose and commit type (each commit should be the smallest meaningful change)
   - Separate unrelated changes into different commits
   - **Never mix commit types** (e.g., feat + docs in same commit)
   - Common atomic groupings:
     - Feature implementation separate from tests
     - Bug fixes separate from refactoring
     - Documentation updates separate from code changes
     - Dependency updates separate from implementation
     - **Same file, different types**: Split feat/fix/docs changes in the same file into separate commits
4. **Create atomic commits** (repeat for each logical group):
   a. Use `git add -p` or selective staging to include only related changes
   b. Determine commit type:
      - `feat`: New feature
      - `fix`: Bug fix
      - `docs`: Documentation only changes
      - `style`: Changes that don't affect code meaning (white-space, formatting)
      - `refactor`: Code change that neither fixes a bug nor adds a feature
      - `perf`: Performance improvement
      - `test`: Adding missing tests or correcting existing tests
      - `build`: Changes to build system or dependencies
      - `ci`: Changes to CI configuration files and scripts
      - `chore`: Other changes that don't modify src or test files
   c. Draft commit message: `[<jira-tag>] <type>[optional scope]: <description>`
      - **JIRA TAG**: If user mentions "Use jira tag XXX-####", use that tag (e.g., [PII-1141])
      - Otherwise, extract from current git branch name (e.g., branch PII-1141 → [PII-1141])
      - One-liner description in imperative mood (e.g., "add" not "adds")
      - Keep under 120 characters (including jira tag prefix)
      - Focus on single purpose
      - Example: `[PII-1141] fix: resolve memory leak in data processor`
   d. Create the commit
5. Show final git log to confirm atomic commits
6. Push all commits to remote (do not push if it is the main branch)

ATOMIC COMMIT PRINCIPLES:
- Each commit serves a single, discrete purpose
- Commits should be self-contained and meaningful
- **NEVER mix different commit types** (e.g., feat + docs, fix + refactor)
- **Split changes by type even within the same file** (e.g., feature code vs documentation in same file)
- If you can't describe the commit in one line, it's probably not atomic
- Commit early and often, but with purpose
- **Use selective staging** (`git add -p`) to separate changes by type within files

IMPORTANT:
- Break down large changes into multiple atomic commits
- Use selective staging (`git add -p`) to group related changes
- Each commit should pass tests independently when possible
- Review each commit's diff before proceeding to the next
