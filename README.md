# Story Dossier Generator

An automated AI-assisted workflow for authors to generate comprehensive story foundations using Claude Code's agent orchestration.

## What This Does

This system helps you go from **genre + themes + research** → **complete story dossier** in three phases:

1. **Phase 1 (Generation)**: Fast generation of core concepts - premise, characters, and world
2. **Phase 2 (Validation)**: Consistency checking and resolution to fix contradictions and logic issues
3. **Phase 3 (Integration)**: Sharpen each section using specifics from all others for maximum cohesion

The result: A concise, internally consistent, deeply integrated story foundation ready for development.

## Directory Structure

```
story-dossier/
├── input.yaml                          # YOU FILL THIS OUT
├── workflow-state.json                 # Tracks progress (auto-managed)
├── blueprints/                         # Blueprint templates (optional)
│   └── epic-fantasy-romance.md
└── lightweight/                        # All phases work here
    ├── 01-story-concept.md             # Phase 1: Core story premise
    ├── 02-character-concept.md         # Phase 1: Character concepts
    ├── 03-world-concept.md             # Phase 1: World rules
    ├── character-consistency-check.md  # Phase 2: Validation
    ├── character-resolution-notes.md   # Phase 2: Fixes
    ├── world-consistency-check.md      # Phase 2: Validation
    ├── world-resolution-notes.md       # Phase 2: Fixes
    ├── final-consistency-report.md     # Phase 2: Overall validation
    ├── character-integration-notes.md  # Phase 3: Enhancement
    ├── world-integration-notes.md      # Phase 3: Enhancement
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
- Genre/subgenre
- Core themes
- Your genre research, comp titles, notes
- Any must-include or avoid elements
- (Optional) Specify a blueprint file for structural guidance

### Step 2: Run Phase 1 (Lightweight Generation)

In Claude Code, run:
```
/generate-story
```

This spawns 3 specialized agents that will:
1. Generate your story premise and themes (using blueprint's project_essentials template if provided)
2. Create character concepts with arcs and motivations (using blueprint's character_roster template, runs in parallel with world)
3. Design world rules that enable your story (using blueprint's world_building template, runs in parallel with character)

If you specified a blueprint in input.yaml, agents will use it as a structural template to organize their outputs.

**Output**: 3 core files in `lightweight/` directory

**Time**: ~2-3 minutes (story sequential, then character + world in parallel)

### Step 3: Review and Validate

Open the files in `lightweight/` and review:
- Does the premise excite you?
- Do the characters feel right?
- Does the world make sense?

**Make any edits you want** directly in these files. They become the foundation for Phase 2.

### Step 4: Run Phase 2 (Validation)

When you're happy with the lightweight concepts, run:
```
/deepen-story
```

This spawns 5 specialized agents (2 per section + 1 final check) that will:

**Step 1:** Both consistency agents run in parallel
**Step 2:** Both resolver agents run in parallel
**Step 3:** Global consistency check

**Output**: Validation reports and resolution notes in `lightweight/` directory. The core concept files are edited to fix any issues.

**Time**: ~3-4 minutes (consistency checks in parallel, resolvers in parallel, then global check)

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

This spawns 3 specialized agents that will:

**Step 1:** Both integration agents run in parallel
- **Character Integration**: Sharpen characters using specifics from story and world
- **World Integration**: Adjust world to amplify character conflicts and story themes

**Step 2:** Cohesion check verifies integration

**The Goal:** Make sections **amplify** each other, not just coexist.

**Examples of integration:**
- Character traits create friction with specific world rules
- World magic system creates natural obstacles for these specific characters
- Story themes are reflected in both character arcs and world details
- World culture shapes character voice and motivations

**Output**: Core concept files edited in place with integration notes showing enhancements

**Time**: ~2-3 minutes (both integrations in parallel, then cohesion check)

### Step 7: Final Review

Review the integration results in `lightweight/`:
- `*-integration-notes.md` files show what was enhanced and why
- `cohesion-report.md` shows overall assessment with cohesion score
- The complete dossier should feel like an integrated whole, not separate pieces

The story foundation is now ready for further development!

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
- Understands genre conventions and expectations
- Actively avoids clichés and overused tropes
- Thinks in terms of character motivation, world logic, and thematic resonance

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

### Parallel & Sequential Processing
The workflow balances parallel and sequential execution:
- **Parallel:** Independent agents run simultaneously (e.g., character + world generation)
- **Sequential:** Dependent steps run in order (e.g., story concept before character/world)
- Maximizes speed while maintaining logical dependencies
- Allows for human validation between phases

## Customization

### Default Settings
Configured for:
- Story concept, character, and world development
- Genre-agnostic foundation building
- Flexible structure to support any genre
- Optional blueprint templates for consistent output structure

### Using Blueprints
Blueprints provide structural templates for organizing story outputs:
- Located in `story-dossier/blueprints/`
- Specify blueprint in `input.yaml` (optional)
- Agents use blueprint sections as organizational templates
- Default blueprint: `epic-fantasy-romance.md` (customizable for any genre)
- Create custom blueprints by copying and modifying existing ones

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
