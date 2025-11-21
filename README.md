# Story Dossier Generator

An automated AI-assisted workflow for fantasy romance authors to generate comprehensive story dossiers using Claude Code's agent orchestration.

## What This Does

This system helps you go from **tropes + genre + research** → **complete story dossier** in two phases:

1. **Phase 1 (Lightweight)**: Fast generation of core concepts - premise, characters, world, romance arc, plot structure
2. **Phase 2 (Deep Development)**: Iterative expansion with contradiction checking and plot hole detection

The result: A detailed, internally consistent story dossier ready for outline generation.

## Directory Structure

```
story-dossier/
├── input.yaml                  # YOU FILL THIS OUT
├── workflow-state.json         # Tracks progress (auto-managed)
├── lightweight/                # Phase 1 outputs
│   ├── 01-story-concept.md
│   ├── 02-character-concept.md
│   ├── 03-world-concept.md
│   ├── 04-romance-arc.md
│   └── 05-plot-structure.md
└── deep/                       # Phase 2 outputs
    ├── characters/
    │   ├── main-characters.md
    │   ├── consistency-check.md
    │   └── resolution-notes.md
    ├── world/
    ├── romance/
    └── plot/
```

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

### Step 5: Final Review

Review the files in `deep/` especially:
- `consistency-check.md` files show what issues were found
- `resolution-notes.md` files show how they were fixed
- `final-consistency-report.md` shows overall analysis

The story dossier is now ready for outline generation!

## Architecture: Why It's Built This Way

### Lightweight Orchestrator
The orchestrator commands (`/generate-story`, `/deepen-story`) read ONLY:
- `input.yaml` (your inputs)
- `workflow-state.json` (progress tracking)

They never read the story content, keeping context minimal.

### Heavy-Context Subagents
Each specialized agent:
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
