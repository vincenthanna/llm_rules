---
name: lint
description: Run linting and auto-fix all issues
---

Run all linting and code quality checks:

!uv run pre-commit run --all-files

If there are any failures:
1. Analyze each failure type
2. For auto-fixable issues, apply the fixes
3. For manual fixes needed (refurb, pyupgrade, etc.):
   - Read the specific files mentioned
   - Apply the suggested fixes
   - Explain what was changed
4. Re-run the command to verify all issues are resolved
5. Provide a summary of what was fixed

NOTE: The command from the Makefile is: `uv run pre-commit run --all-files`
