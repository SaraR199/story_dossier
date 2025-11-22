# Story Dossier Generator

An automated AI-assisted workflow for fantasy romance authors to generate comprehensive story dossiers using Claude Code's agent orchestration.

## What This Does

This system helps you go from **tropes + genre + research** → **complete story dossier** in three phases:

1. **Phase 1 (Generation)**: Fast generation of core concepts - premise, characters, world, romance arc, plot structure
2. **Phase 2 (Validation)**: Consistency checking and resolution to fix contradictions and plot holes
3. **Phase 3 (Integration)**: Sharpen each section using specifics from all others for maximum cohesion

The result: A concise, internally consistent, deeply integrated story dossier ready for outline generation.

## Directory Structure

```
story-dossier/
├── input.yaml                          # YOU FILL THIS OUT
├── workflow-state.json                 # Tracks progress (auto-managed)
├── blueprints/                         # Blueprint templates (optional)
│   └── epic-fantasy-romance.md
└── lightweight/                        # All phases work here
    ├── 01-story-concept.md             # Phase 1: Core story premise
    ├── 02-character-concept.md         # Phase 1: Character basics
    ├── 03-world-concept.md             # Phase 1: World rules
    ├── 04-romance-arc.md               # Phase 1: Romance progression
    ├── 05-plot-structure.md            # Phase 1: Plot structure
    ├── character-consistency-check.md  # Phase 2: Validation
    ├── character-resolution-notes.md   # Phase 2: Fixes
    ├── world-consistency-check.md      # Phase 2: Validation
    ├── world-resolution-notes.md       # Phase 2: Fixes
    ├── romance-consistency-check.md    # Phase 2: Validation
    ├── romance-resolution-notes.md     # Phase 2: Fixes
    ├── plot-consistency-check.md       # Phase 2: Validation
    ├── plot-resolution-notes.md        # Phase 2: Fixes
    ├── final-consistency-report.md     # Phase 2: Overall validation
    ├── character-integration-notes.md  # Phase 3: Enhancement
    ├── world-integration-notes.md      # Phase 3: Enhancement
    ├── romance-integration-notes.md    # Phase 3: Enhancement
    ├── plot-integration-notes.md       # Phase 3: Enhancement
    └── cohesion-report.md              # Phase 3: Final assessment
```

## Workflow Philosophy

**Lightweight & Focused:**
The workflow keeps everything in the `lightweight/` directory, avoiding bloat. Phase 1 generates core concepts, Phase 2 validates and fixes issues, and Phase 3 integrates sections for maximum cohesion.

**No Over-Expansion:**
Unlike traditional story development tools that create massive detailed documents, this workflow maintains concise concepts while ensuring they're logically consistent and well-integrated.

## How to Use

### Step 1: Fill Out Your Inputs

Edit `story-dossier/input.yaml` with:
- Romance tropes you want to explore
- Genre/subgenre
- Chapter count
- Your genre research, comp titles, notes
- Any must-include or avoid elements

### Step 2: Run Phase 1 (Lightweight Generation)

In Claude Code, run:
```
/generate-story
```

This spawns 5 specialized agents that will:
1. Generate your story premise and themes
2. Create basic character concepts
3. Design world rules that enable your tropes
4. Map out the romance arc
5. Structure the plot into 4 acts

**Output**: Files in `lightweight/` directory

**Time**: ~5-10 minutes (agents run sequentially)

### Step 3: Review and Validate

Open the files in `lightweight/` and review:
- Does the premise excite you?
- Do the characters feel right?
- Does the world make sense?
- Does the romance arc follow your tropes?

**Make any edits you want** directly in these files. They become the foundation for Phase 2.

### Step 4: Run Phase 2 (Validation)

When you're happy with the lightweight concepts, run:
```
/deepen-story
```

This spawns 9 specialized agents (2 per section + 1 final check) that will:

**For each section (Characters, World, Romance, Plot):**
1. **Consistency Agent**: Checks for contradictions and logic issues
2. **Resolver Agent**: Fixes identified problems in place

**Plus a final global consistency check across all sections**

**Output**: Validation reports and resolution notes in `lightweight/` directory. The core concept files are edited to fix any issues.

**Time**: ~10-15 minutes (agents run sequentially)

### Step 5: Review Phase 2 Outputs

Review the validation files in `lightweight/`:
- `*-consistency-check.md` files show what issues were found
- `*-resolution-notes.md` files show how they were fixed
- `final-consistency-report.md` shows overall analysis

**Make any edits you want** directly in the core concept files before moving to Phase 3.

### Step 6: Run Phase 3 (Integration Enhancement)

When you're satisfied with the validated sections, run:
```
/integrate-story
```

This spawns 5 specialized agents that will:

**Integration Refinement (one agent per section):**
1. **Character Integration**: Sharpen characters using specifics from world/romance/plot
2. **World Integration**: Adjust world to amplify character conflicts and romance situations
3. **Romance Integration**: Heighten chemistry using character wounds and world constraints
4. **Plot Integration**: Make plot showcase character growth and leverage world mechanics
5. **Cohesion Check**: Verify integration enhanced cohesion without breaking consistency

**The Goal:** Make sections **amplify** each other, not just coexist.

**Examples of integration:**
- Character trait creates friction with specific world rule
- World magic system forces exact romance trope situations
- Romance beats align perfectly with plot turning points
- Plot obstacles arise from character flaws + world constraints

**Output**: Core concept files edited in place with `*-integration-notes.md` files showing enhancements

**Time**: ~10-15 minutes (agents run sequentially)

### Step 7: Final Review

Review the integration results in `lightweight/`:
- `*-integration-notes.md` files show what was enhanced and why
- `cohesion-report.md` shows overall assessment with cohesion score
- The complete dossier should feel like an integrated whole, not separate pieces

The story dossier is now ready for outline generation!

## Architecture: Why It's Built This Way

### Lightweight Orchestrator
The orchestrator commands (`/generate-story`, `/deepen-story`, `/integrate-story`) read ONLY:
- `input.yaml` (your inputs)
- `workflow-state.json` (progress tracking)

They never read the story content, keeping context minimal.

### Custom Story Development Agent
All subagents use the **story-architect** agent (`.claude/agents/story-architect.md`), a custom agent optimized for creative story development:

**Why a custom agent?**
- Focused on story planning, not code
- Trained to avoid generic "LLM soup" outputs
- Emphasizes logical consistency and unique details
- Understands fantasy romance genre conventions
- Actively avoids clichés and overused tropes
- Thinks in terms of character motivation, world logic, and emotional resonance

**What makes it different from code agents?**
- No code-focused thinking or terminology
- Prioritizes creative problem-solving over technical solutions
- Validates story logic, not code logic
- Creates cohesive narratives, not modular functions
- Focuses on emotional truth and character authenticity

All workflow phases use this same agent persona for consistency.

### Heavy-Context Subagents
Each specialized agent:
- Uses the story-architect agent type
- Reads only what it needs from previous outputs
- Uses extended thinking mode for quality
- Writes focused outputs
- Updates workflow state

### Sequential Processing
Agents run one at a time to:
- Ensure consistency (each agent builds on previous work)
- Avoid context bloat from parallel processing
- Allow for human validation between phases

## Customization

### Default Settings
Configured for:
- Fantasy romance genre
- 70k-90k words
- 24-28 chapters
- 4-act structure (Act 1, 2A, 2B, 3)

### To Modify
Edit the orchestrator commands in `.claude/commands/`:
- `generate-story.md` - Phase 1 (generation) logic
- `deepen-story.md` - Phase 2 (validation) logic
- `integrate-story.md` - Phase 3 (integration) logic

## Managing Multiple Book Projects

This workflow is designed for **branch-per-book** project management. Each book gets its own branch while sharing the workflow system and blueprints.

### Quick Start: New Book Project

```bash
# Start from main branch
git checkout main
git pull

# Create branch for your new book
git checkout -b book/your-book-name

# Fill out story-dossier/input.yaml with your story details
# Run the workflow: /generate-story → /deepen-story → /integrate-story
# Commit your completed story dossier

git add story-dossier/
git commit -m "Complete story dossier for [Book Name]"
git push -u origin book/your-book-name
```

### Switch Between Book Projects

```bash
# Work on a different book
git checkout book/shadow-king

# Your story-dossier/ automatically switches to that book's content
# Make edits, run commands, commit changes

# Switch to another book
git checkout book/blood-bonds
```

### Update Workflow Across All Books

When you improve the workflow or add new blueprints:

```bash
# Update on main
git checkout main
# Make your workflow improvements
git commit and push

# Merge to your book branches
git checkout book/your-book-name
git merge main
git push
```

### Branch Structure

- **main**: Workflow system, blueprints, documentation (no book content)
- **book/[name]**: Individual book branches (contain story content)
- Each book branch can be worked on independently
- Workflow updates on main can be merged to all book branches

**See WORKFLOW.md for detailed multi-project workflow documentation.**

## Troubleshooting

**"Phase 1 not complete" error when running Phase 2:**
- Run `/generate-story` first
- Check `workflow-state.json` shows `"lightweight_complete": true`

**Agent gets stuck or errors:**
- Check `workflow-state.json` for error messages
- Review the last file the agent tried to write
- You may need to manually fix and re-run

**Want to regenerate a specific section:**
- Delete the section's output files
- Manually update `workflow-state.json` to remove that agent from `completed_agents`
- Re-run the phase command

## Next Steps

After your story dossier is complete, you can:
1. Use it as context for Claude to generate a detailed chapter-by-chapter outline
2. Export it to your writing software
3. Iterate on any section by asking Claude to revise specific files

---

**Questions or issues?** Check the orchestrator command files in `.claude/commands/` for detailed logic.
