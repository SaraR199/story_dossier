# Branch-Based Project Workflow

This repository uses a **branch-per-book** workflow to manage multiple story projects while sharing the same workflow system and blueprints.

## Repository Structure

### Main Branch
- Contains the workflow system (`.claude/commands/`)
- Contains shared blueprints (`story-dossier/blueprints/`)
- Contains documentation (`README.md`)
- Contains **template/empty** `story-dossier/` directory
- **Does NOT contain** any book-specific story content

### Book Branches
- Each book gets its own branch (e.g., `book/shadow-king`, `book/blood-bonds`)
- Contains filled-out `story-dossier/input.yaml`
- Contains generated story content (`lightweight/`, `deep/`)
- Branched from main, so includes all workflow updates

## Starting a New Book Project

### Option 1: From Main Branch (Recommended)

```bash
# Make sure you're on main and up to date
git checkout main
git pull

# Create a new branch for your book
git checkout -b book/your-book-name

# Edit input.yaml with your story details
# Edit story-dossier/input.yaml

# Run the workflow
# /generate-story
# /deepen-story
# /integrate-story

# Commit your story content
git add story-dossier/
git commit -m "Complete story dossier for [Your Book Name]"
git push -u origin book/your-book-name
```

### Option 2: From Existing Book (for Series)

If you're writing a sequel and want to reference the previous book:

```bash
# Start from your previous book's branch
git checkout book/book1-shadow-king

# Create new branch for book 2
git checkout -b book/book2-blood-bonds

# Clear the old story content
rm -rf story-dossier/lightweight/* story-dossier/deep/*

# Reset workflow state
# Edit story-dossier/workflow-state.json to reset it

# Edit input.yaml for new book
# Run workflow for new book
```

## Working on Multiple Books

### Switch Between Books

```bash
# Switch to a different book
git checkout book/shadow-king

# Your story-dossier/ now contains that book's content
# Run commands, make edits, commit changes

# Switch to another book
git checkout book/blood-bonds

# story-dossier/ now contains this book's content
```

### Compare Across Books

```bash
# View character differences between books
git diff book/shadow-king:story-dossier/deep/characters/main-characters.md book/blood-bonds:story-dossier/deep/characters/main-characters.md

# Check out a file from another branch for reference
git show book/shadow-king:story-dossier/deep/world/world-bible.md
```

## Updating Workflow System Across All Books

When you improve the workflow commands or add new blueprints:

### Update Main Branch

```bash
# Make changes on main
git checkout main

# Edit .claude/commands/ or add new blueprints
# Commit changes
git add .claude/ story-dossier/blueprints/
git commit -m "Add new blueprint for cozy fantasy romance"
git push
```

### Merge Updates to Book Branches

```bash
# Switch to your book branch
git checkout book/shadow-king

# Merge main to get workflow updates
git merge main

# Resolve any conflicts (usually none for workflow files)
# Your book content is preserved, workflow is updated

# Push updated branch
git push
```

## Branch Naming Convention

Recommended naming:
- `main` - workflow system only
- `book/[book-title]` - individual book projects
- `series/[series-name]/book[N]` - for series with shared elements

Examples:
- `book/shadow-king`
- `book/blood-bonds`
- `series/realms-of-chaos/book1`
- `series/realms-of-chaos/book2`

## What Gets Committed Where

### Main Branch
✅ `.claude/commands/` - workflow orchestrators
✅ `story-dossier/blueprints/` - templates
✅ `README.md`, `WORKFLOW.md` - documentation
✅ `story-dossier/input.yaml` - template only
✅ `story-dossier/workflow-state.json` - template only
❌ `story-dossier/lightweight/` - kept empty
❌ `story-dossier/deep/` - kept empty

### Book Branches
✅ Everything from main (via merge)
✅ `story-dossier/input.yaml` - filled out for this book
✅ `story-dossier/workflow-state.json` - tracks this book's progress
✅ `story-dossier/lightweight/` - generated concepts
✅ `story-dossier/deep/` - generated dossier

## Best Practices

1. **Always start book branches from main** - ensures you have latest workflow
2. **Commit story content regularly** - don't lose generated work
3. **Merge main periodically** - keep workflow system up to date
4. **Use descriptive branch names** - helps identify books quickly
5. **Don't work on multiple books simultaneously** - switch branches instead

## Troubleshooting

**Q: I accidentally committed story content to main. How do I fix it?**
```bash
# Remove the files
git rm -r story-dossier/lightweight/* story-dossier/deep/*
git commit -m "Remove story content from main"

# The content still exists in your book branches
```

**Q: I want to create a new book but keep reference to another book's world**
```bash
# Start from the other book's branch
git checkout book/shadow-king

# Create new branch
git checkout -b book/blood-bonds

# Keep the world files but regenerate characters/romance/plot
# Manually edit as needed
```

**Q: How do I share a blueprint across all my books?**
```bash
# Add/update blueprint on main
git checkout main
# Add to story-dossier/blueprints/
git add story-dossier/blueprints/my-new-blueprint.md
git commit -m "Add new blueprint"
git push

# Merge to each book branch
git checkout book/shadow-king
git merge main
git push
```

---

## Quick Reference

```bash
# Start new book
git checkout main && git pull
git checkout -b book/[name]
# Fill input.yaml, run workflow, commit

# Switch books
git checkout book/[name]

# Update workflow in book
git merge main

# Update blueprint for all books
git checkout main
# Edit blueprints, commit
git checkout book/[name] && git merge main
```
