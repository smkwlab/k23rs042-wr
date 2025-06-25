# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a LaTeX template for weekly reports (週間報告) used by students at Kyushu Sangyo University. The template includes automated PDF building via GitHub Actions when TeX files are modified.

## Key Commands

### LaTeX Compilation
```bash
# Compile the weekly report
latexmk -pv 20yy-mm-dd.tex

# Clean auxiliary files
latexmk -c
```

### Text Linting
```bash
# Check Japanese text quality (requires textlint)
textlint 20yy-mm-dd.tex
```

## Architecture

### LaTeX Configuration
- **Engine**: uplatex with dvipdfmx for Japanese text processing
- **Main Document**: `20yy-mm-dd.tex` (template filename, should be renamed with actual date)
- **Configuration**: `.latexmkrc` defines compilation settings for uplatex workflow
- **Text Quality**: `.textlintrc` enforces Japanese academic writing standards

### Template Structure
The weekly report template includes:
- **今週の報告**: Current week's activities (research, job hunting, other)
- **来週の予定**: Next week's plans
- **スケジュール**: Semester schedule table
- **Image Support**: `img/` directory for figures (includes sample haro-mini.jpg)

### Automated Workflow
- **Trigger**: GitHub Actions runs on TeX file changes
- **Container**: Uses `ghcr.io/smkwlab/texlive-ja-textlint:2023c-alpine` 
- **Process**: Detects modified TeX files and automatically builds PDFs
- **Output**: Generated PDFs are released as artifacts

## File Naming Convention
- Main TeX file should follow `20yy-mm-dd.tex` pattern (year-month-day)
- Generated PDFs are ignored by git (pattern `20??-??-??.pdf` in .gitignore)
- Students rename the template file to match their report date

## Japanese Text Standards
The textlint configuration enforces:
- Maximum 8 consecutive kanji characters (with university name exceptions)
- Proper punctuation and spacing between half/full-width characters
- Consistent writing style (である調)
- Sentence length limits (max 100 characters)
- University-specific terminology allowlist

## Security and Permission Guidelines

### 🚨 CRITICAL: GitHub Administration Rules

#### Git and GitHub Operations
- **NEVER use `--admin` flag** with `gh pr merge` or similar commands
- **NEVER bypass Branch Protection Rules** without explicit user permission
- **ALWAYS respect the configured workflow**: approval process, status checks, etc.

#### When Branch Protection Blocks Operations
1. **Report the situation** to user with specific error message
2. **Explain available options**:
   - Wait for required approvals
   - Wait for status checks to pass
   - Use `--auto` flag for automatic merge after requirements met
   - Request explicit permission for admin override (emergency only)
3. **Wait for user instruction** - never assume intent

#### Proper Error Handling Example
```bash
# When this fails:
gh pr merge 90 --squash --delete-branch
# Error: Pull request is not mergeable: the base branch policy prohibits the merge

# CORRECT response:
echo "Branch Protection Rules prevent merge. Options:"
echo "1. Wait for required approvals (currently need: 1)"
echo "2. Wait for status checks (currently pending: build-and-release-pdf)"
echo "3. Use --auto to merge automatically when requirements met"
echo "4. Request admin override (emergency only)"
echo "Please specify how to proceed."

# WRONG response:
gh pr merge 90 --squash --delete-branch --admin  # NEVER DO THIS
```

#### Emergency Admin Override
- Only use `--admin` flag when explicitly requested by user
- Document the reason for override in commit/PR description
- Report the action taken and why it was necessary

### Rationale
Branch Protection Rules exist to:
- Ensure code quality through required reviews
- Prevent accidental breaking changes
- Maintain audit trail of changes
- Enforce consistent development workflow

Bypassing these rules undermines repository security and development process integrity.