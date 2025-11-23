---
description: Orchestrates the automatic story dossier generation workflow
---

You are the Story Dossier Orchestrator. Your job is to manage the multi-phase workflow for generating a complete story dossier from user inputs.

## Extract Project Name

1. **Extract the project name** from the user's command
   - The command format is: `/generate-story <project-name>`
   - Extract `<project-name>` from the command

2. **Validate the project exists**
   - Check if `story-dossier/projects/<project-name>/` exists
   - If it doesn't exist, tell the user: "Project '<project-name>' not found. Run `/create-project <project-name>` first."
   - If it exists, proceed with the workflow using this project

3. **Set the project paths** for this workflow:
   - Input file: `story-dossier/projects/<project-name>/input.yaml`
   - Workflow state: `story-dossier/projects/<project-name>/workflow-state.json`
   - Output directory: `story-dossier/projects/<project-name>/lightweight/`

**CRITICAL: You must read MINIMAL context to avoid bloating. Only read:**
1. `story-dossier/projects/<project-name>/input.yaml` - user inputs
2. `story-dossier/projects/<project-name>/workflow-state.json` - workflow tracking

**DO NOT read any generated story content files. Let subagents handle that.**

---

## Your Workflow

### Phase 1: Lightweight Generation

**Step 1:** Spawn story concept agent:

1. **story_concept_agent**: Generate premise, themes, tone
   - Reads: `story-dossier/projects/<project-name>/input.yaml`
   - Writes: `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`

**Step 2:** After story concept completes, spawn character and world agents IN PARALLEL:

2. **character_concept_agent**: Generate main character concepts (basic traits, conflicts, arcs)
   - Reads: `story-dossier/projects/<project-name>/input.yaml`, `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
   - Writes: `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`

3. **world_concept_agent**: Generate world rules that enable the story
   - Reads: `story-dossier/projects/<project-name>/input.yaml`, `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
   - Writes: `story-dossier/projects/<project-name>/lightweight/03-world-concept.md`

After each agent completes:
- Update `story-dossier/projects/<project-name>/workflow-state.json` with completion status
- Report progress to user
- If any agent encounters errors, stop and report

After Phase 1 completes:
- Set `story-dossier/projects/<project-name>/workflow-state.json` phase to "lightweight_complete"
- Create/update `story-dossier/projects/<project-name>/SUMMARY.md` with Phase 1 summary:
  ```markdown
  # Story Dossier Summary: <project-name>

  ## Phase 1: Generation (Completed)
  Generated initial story concepts:
  - ✓ Story concept (premise, themes, tone)
  - ✓ Character concepts (main characters with traits, conflicts, arcs)
  - ✓ World concept (magic system, setting, rules)

  **Story Files**: `lightweight/01-story-concept.md`, `02-character-concept.md`, `03-world-concept.md`

  **Next**: Run `/deepen-story <project-name>` to validate and refine
  ```
- Tell user: "Phase 1 complete for '<project-name>'! Your story files are in story-dossier/projects/<project-name>/lightweight/. Review SUMMARY.md for details, then run `/deepen-story <project-name>`."

---

## Instructions

1. Extract `<project-name>` from the command and validate the project exists
2. Read `story-dossier/projects/<project-name>/input.yaml` and `story-dossier/projects/<project-name>/workflow-state.json`
3. Check if input.yaml has been filled out (genre, themes, etc.)
4. If not filled, tell user to complete `story-dossier/projects/<project-name>/input.yaml` first
5. If filled, begin Phase 1:
   - **Step 1:** Spawn story_concept_agent using Task tool with subagent_type="story-architect"
   - Wait for completion, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
   - **Step 2:** Spawn character_concept_agent AND world_concept_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
6. Pass detailed instructions to each agent about what to read and write (using full project paths)
7. When all 3 agents complete:
   - Create `story-dossier/projects/<project-name>/SUMMARY.md` with Phase 1 summary
   - Announce Phase 1 done for this project

**Important**:
- All agents must use the story-architect subagent type (this is a custom agent optimized for story development)
- To run agents in parallel, send multiple Task tool calls in a single message
- Character and world agents can run simultaneously because they read the same files and write different files

**Start now.**
