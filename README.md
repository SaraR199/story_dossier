# Story Dossier Generator

An automated AI-assisted workflow for fantasy romance authors to generate comprehensive story dossiers using Claude Code's agent orchestration.

## What This Does

This system helps you go from **tropes + genre + research** → **complete story dossier** in three phases:

1. **Phase 1 (Lightweight)**: Fast generation of core concepts - premise, characters, world, romance arc, plot structure
2. **Phase 2 (Deep Development)**: Iterative expansion with contradiction checking and plot hole detection
3. **Phase 3 (Integration)**: Sharpen each section using specifics from all others for maximum cohesion

The result: A detailed, internally consistent, deeply integrated story dossier ready for outline generation.

## Directory Structure

```
story-dossier/
├── input.yaml                  # YOU FILL THIS OUT
├── workflow-state.json         # Tracks progress (auto-managed)
├── blueprints/                 # Blueprint templates for different genres
│   ├── epic-fantasy-romance.md
│   ├── cozy-fantasy-romance.md (add your own)
│   └── [your-custom-blueprint].md
├── lightweight/                # Phase 1 outputs
│   ├── 01-story-concept.md
│   ├── 02-character-concept.md
│   ├── 03-world-concept.md
│   ├── 04-romance-arc.md
│   └── 05-plot-structure.md
└── deep/                       # Phase 2 & 3 outputs
    ├── characters/
    │   ├── main-characters.md
    │   ├── consistency-check.md
    │   ├── resolution-notes.md
    │   └── integration-notes.md
    ├── world/
    │   ├── world-bible.md
    │   ├── consistency-check.md
    │   ├── resolution-notes.md
    │   └── integration-notes.md
    ├── romance/
    │   ├── romance-arc-detailed.md
    │   ├── consistency-check.md
    │   ├── resolution-notes.md
    │   └── integration-notes.md
    ├── plot/
    │   ├── detailed-structure.md
    │   ├── consistency-check.md
    │   ├── resolution-notes.md
    │   └── integration-notes.md
    ├── final-consistency-report.md
    └── cohesion-report.md
```

## Blueprint System

**What are blueprints?**
Blueprints are structured templates that define exactly what fields and sections your story dossier should contain. Each blueprint is tailored to a specific genre or story type.

**Why use blueprints?**
- Ensures Phase 2 outputs have YOUR preferred structure
- Different genres need different information (epic fantasy vs contemporary romance)
- Agents intelligently fill out YOUR template, not a generic one
- Consistent format across all your projects

**How it works:**
1. You select a blueprint in `input.yaml` (e.g., `blueprint: "epic-fantasy-romance"`)
2. Phase 2 agents read that blueprint file from `story-dossier/blueprints/`
3. Agents intelligently identify which sections relate to their task (characters, world, romance, plot)
4. Agents fill out those sections using the lightweight concepts from Phase 1
5. Output follows your blueprint's exact structure and field names

**Creating custom blueprints:**
1. Copy an existing blueprint from `story-dossier/blueprints/`
2. Modify sections, add/remove fields, reorganize as needed
3. Save with a descriptive name (e.g., `urban-fantasy-romance.md`)
4. Update `input.yaml` to use your new blueprint
5. Agents will automatically adapt to your custom structure

**Blueprint flexibility:**
- Each blueprint can have completely different sections
- Agents use AI to identify relevant sections (no hardcoded mapping)
- You can reorganize, rename, or restructure however you want
- Perfect for authors with specific workflows or genre requirements

## How to Use

### Step 1: Fill Out Your Inputs

Edit `story-dossier/input.yaml` with:
- **Blueprint selection** - choose which blueprint structure to use
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

### Step 4: Run Phase 2 (Deep Development)

When you're happy with the lightweight concepts, run:
```
/deepen-story
```

This spawns 12 specialized agents (3 per section) that will:

**For each section (Characters, World, Romance, Plot):**
1. **Expansion Agent**: Adds depth and detail
2. **Consistency Agent**: Checks for contradictions and logic issues
3. **Resolver Agent**: Fixes identified problems

**Plus a final global consistency check across all sections**

**Output**: Detailed files in `deep/` directory with consistency reports

**Time**: ~20-30 minutes (agents run sequentially)

### Step 5: Review Phase 2 Outputs

Review the files in `deep/` especially:
- `consistency-check.md` files show what issues were found
- `resolution-notes.md` files show how they were fixed
- `final-consistency-report.md` shows overall analysis

**Make any edits you want** directly in these files before moving to Phase 3.

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

**Output**: Each section edited in place with `integration-notes.md` files showing enhancements

**Time**: ~15-20 minutes (agents run sequentially)

### Step 7: Final Review

Review the integration results:
- `integration-notes.md` files in each section show what was enhanced and why
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
- `generate-story.md` - Phase 1 logic
- `deepen-story.md` - Phase 2 logic
- `integrate-story.md` - Phase 3 logic

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
