---
name: docstring
description: Add docstrings, type hints, and update project documentation
---

# Add Docstrings, Type Hints, and Update Documentation

Please perform the following tasks:

## 1. Python Code Documentation

First, check the Python version for this project:
- Look for `.python-version` file in the project root
- If empty or not found, check `requires-python` in `pyproject.toml`
- Use the discovered Python version to guide syntax recommendations

Scan ONLY modified Python files in the project:

**IMPORTANT**: Process only Python files that have been modified according to git status:
- Use `git diff --name-only HEAD` to get files modified since last commit
- Use `git status --porcelain` to get files with unstaged changes
- Filter results to only include `.py` files
- Exclude `__pycache__` directories and `.pyc` files
- Focus only on files that actually need documentation updates

For each Python file:

### Add Google-style docstrings to all:
- Modules (at the top of files)
- Classes
- Functions and methods

### Add type hints to all:
- Function parameters
- Function return types
- Class attributes
- Variable declarations where beneficial

### Add inline comments for:
- Complex logic
- Non-obvious code sections
- Important business logic

### Use Python version-appropriate syntax features:

Based on the project's Python version, use these features:

**For Python 3.12+:**
- Use `type` statement for type aliases
- Use `|` for union types instead of `Union`
- Use built-in generic types (list, dict) instead of typing module equivalents
- Use `override` decorator for overridden methods
- Use improved f-string syntax with nested expressions

**For Python 3.11:**
- Use `|` for union types instead of `Union`
- Use built-in generic types (list, dict) instead of typing module equivalents
- Use `Self` type for methods that return their own class
- Use exception groups and `except*` where appropriate

**For Python 3.10:**
- Use `|` for union types instead of `Union`
- Use built-in generic types (list, dict) instead of typing module equivalents
- Use structural pattern matching where it improves readability
- Use parenthesized context managers

**For Python 3.9:**
- Use built-in generic types (list, dict) instead of typing module equivalents
- Use `Annotated` for additional type metadata where helpful

**For earlier versions:**
- Use appropriate typing imports (List, Dict, Union, etc.)
- Follow the syntax constraints of the specific version

### Follow these guidelines:
- Use Google-style docstrings format
- Include Args, Returns, Raises sections where applicable
- Add Examples section for complex functions
- Keep docstrings concise but informative
- Ensure all public APIs are well documented
- Use syntax features that match the project's Python version
- **IMPORTANT: All comments and docstrings must be in English**
- If you find comments in other languages, either delete them if unnecessary or translate and improve them to English
- Ensure all documentation is clear and follows proper English grammar

## 2. Update Makefile

Analyze the current project structure and update the Makefile:

### Review and update targets:
- Ensure all Python scripts have corresponding make targets if needed
- Update file paths in existing targets to match current structure
- Add new targets for any new functionality
- Remove targets for deleted functionality
- Ensure virtual environment commands use the correct Python version

### Common updates to check:
- `make install`: Verify it uses current dependency files
- `make test`: Ensure test paths are correct
- `make lint`: Verify linting commands match current setup
- `make format`: Check formatting tools configuration
- `make samples` or similar: Update paths for data processing scripts
- `make clean`: Ensure it cleans all generated files
- Add Docker-related targets if Docker is used
- Add any new utility targets based on current scripts

### Makefile best practices:
- Use .PHONY for non-file targets
- Include help target with descriptions
- Use variables for repeated paths
- Ensure targets have proper dependencies
- Add comments explaining complex targets

## 3. Update Project Documentation

After updating the Python code documentation and Makefile:

### Update README.md:
- Ensure the feature list accurately reflects current functionality
- Update installation and setup instructions if needed
- Verify all command examples work with the current implementation
- Update the architecture section to match the current codebase
- Add or update any new features or capabilities
- Ensure troubleshooting section covers common issues
- Verify Python version requirements are correctly stated

### Update CLAUDE.md:
- Update the project structure section to reflect current file organization
- Ensure all commands in the Commands section are current and working
- Update the Architecture section with any new components or changes
- Add any new conventions or patterns being used in the codebase
- Update technology stack if new dependencies were added
- Document any new development workflows or procedures
- Note the Python version and its implications for code style

### Documentation Guidelines:
- Keep descriptions clear and concise
- Use consistent formatting throughout
- Include practical examples where helpful
- Ensure all file paths and commands are accurate
- Remove any outdated or deprecated information
- Add any new configuration options or environment variables
- Ensure Python version requirements are consistent across all documentation

## Processing Order

1. **First, get modified file list**:
   - Run `git diff --name-only HEAD` to get files modified since last commit
   - Run `git status --porcelain | cut -c4-` to get files with unstaged changes
   - Combine and filter results to only include `.py` files
   - Exclude `__pycache__` directories and `.pyc` files
   - Create a checklist of all modified Python files to process

2. **Process Python files systematically**:
   - Start with root directory files (main.py, config.py, etc.)
   - Then process each subdirectory completely:
     - db/
     - models/ (ALL model files)
     - processors/ (ALL processor files)
     - utils/
     - realtime/
     - today/
     - daily/
     - analysis/
     - tests/ (if needed)
   - For each file, add module docstring, class docstrings, function docstrings, and type hints
   - Track progress and ensure no file is skipped

3. **Then update documentation**:
   - Update/Create Makefile
   - Update README.md
   - Update CLAUDE.md

Show a summary of files processed at the end to confirm complete coverage.
