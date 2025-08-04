---
name: pr
description: Create or update PR with JIRA tags from commits
---

# Create or Update Pull Request

This command creates a new PR or updates an existing one for the current branch.

## Steps:

1. **Get current branch and check if it's main/master**
   - Exit if on main/master branch

2. **Find the base branch** (where current branch started from)
   - Use `git merge-base` to find the common ancestor
   - Determine the parent branch (usually main or develop)

3. **Check if PR already exists**
   - Use `gh pr list` to check for existing PR

4. **Collect all JIRA tags from commits**
   - Get all commits between base branch and HEAD
   - Extract unique JIRA tags (format: XXX-####) from commit messages
   - Sort tags for consistent ordering
   - Use commits ONLY for JIRA tag extraction, not for understanding changes

5. **Analyze code changes**
   - Run `git diff {base_branch}...HEAD` to see all code changes
   - Examine modified files by type (source code, tests, configs, docs)
   - Understand the actual implementation changes, not just commit descriptions
   - Group changes by component/module/feature area

6. **Generate PR title**
   - Format: "[TAG-1] [TAG-2] Description"
   - Create description based on actual code changes analyzed
   - If no JIRA tags found, use branch name

7. **Generate PR description**
   - Structure the PR description with these sections:
     - **Summary** (Korean): Brief overview based on code analysis of what this PR accomplishes
     - **Key Changes** (Korean): Bullet points of key features from code diff (exclude logging improvements, reformatting, etc.)
     - **Changes by Component**: Detailed technical changes grouped by component/area from actual diff
     - **Test Plan**: Checklist of testing steps based on what was changed
     - **Breaking Changes**: Identify from code diff any breaking changes or "None"
   - Include JIRA links for each tag: `https://deepingsource.atlassian.net/browse/{JIRA_TAG}`
   - Base all content on actual code changes, not commit messages
   - **DO NOT include "🤖 Generated with Claude Code" or any Claude attribution**

8. **Create or update PR**
   - If PR doesn't exist: `gh pr create`
   - If PR exists: `gh pr edit`
   - Set base branch, title, and body

## PR Description Format:

```markdown
## Summary
[Korean text describing what this PR accomplishes]

## Key Changes
- [Korean bullet point for key feature 1]
- [Korean bullet point for key feature 2]

Related JIRA: [PII-1234](https://deepingsource.atlassian.net/browse/PII-1234)

## Changes by Component

### Component/Area Name
- Technical change 1
- Technical change 2

## Test Plan
- [ ] Test step 1
- [ ] Test step 2
- [ ] Verify no regression in existing functionality

## Breaking Changes
None
```

## Important Notes:
- The base branch is automatically detected (where the branch diverged from)
- **PR content is based on actual code diff analysis, NOT commit messages**
- Commits are used ONLY for extracting JIRA tags
- All JIRA tags from commits are included in the title
- Summary and Key Changes sections use Korean language
- All section titles remain in English
- Key Changes should only include significant features (not logging, formatting, refactoring)
- Each JIRA tag gets a clickable link to the issue
- The PR description reflects what the code actually does, not what commits say
- No Claude attribution is added to the PR
