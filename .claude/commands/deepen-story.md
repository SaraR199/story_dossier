---
description: Phase 2 - Deep development with contradiction checking and plot hole detection
---

You are the Story Dossier Deep Development Orchestrator. Your job is to take the lightweight story concepts and expand them into detailed, internally consistent story dossiers using the author's blueprint structure.

**CRITICAL: You must read MINIMAL context to avoid bloating. Only read:**
1. `story-dossier/input.yaml` - user inputs (including blueprint selection)
2. `story-dossier/workflow-state.json` - workflow tracking

**DO NOT read the story content files or the blueprint. Let subagents handle that.**

---

## Prerequisites

Check workflow-state.json: Phase 1 (lightweight) must be complete before running Phase 2.
If not complete, tell user to run `/generate-story` first.

---

## Phase 2 Workflow: Iterative Deep Development with Blueprint Structure

**Blueprint System:**
The user has selected a blueprint (specified in input.yaml) that defines the structure and fields for the story dossier. Each agent must:
1. Read the blueprint file: `story-dossier/blueprints/{blueprint_name}.md`
2. Intelligently identify which sections of the blueprint relate to their task
3. Use those sections as the structural template for their output
4. Follow the blueprint's formatting, field names, and organization exactly

**Important:** Different blueprints may organize sections differently. Agents must be flexible and use judgment to identify relevant sections.

---

For each section below, spawn THREE agents in sequence:

### Section 1: Characters

1. **character_expansion_agent**: Expand character concepts into full dossiers
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/blueprints/{blueprint_name}.md`
     - ALL files in `story-dossier/lightweight/`
   - Writes: `story-dossier/deep/characters/main-characters.md`
   - Task:
     - Read the blueprint and identify ALL sections related to character development (e.g., character_roster, main_character profiles, voice, motivation, etc.)
     - Use those sections as your structural template
     - Create detailed character profiles following the blueprint's exact format
     - Fill out all fields with specific, story-relevant details based on the lightweight concepts
     - Ensure characters enable the tropes and plot from input.yaml

2. **character_consistency_agent**: Check for contradictions and logic issues
   - Reads:
     - `story-dossier/input.yaml`
     - ALL `story-dossier/lightweight/*.md`
     - `story-dossier/deep/characters/main-characters.md`
   - Writes: `story-dossier/deep/characters/consistency-check.md`
   - Reviews:
     - Do character traits support their planned actions?
     - Do backgrounds logically lead to their current state?
     - Are motivations clear and consistent?
     - Do character arcs align with the story's themes?
     - Any contradictions between characters?
   - Output: List specific issues found with line references

3. **character_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/deep/characters/main-characters.md`
     - `story-dossier/deep/characters/consistency-check.md`
   - Edits: `story-dossier/deep/characters/main-characters.md` (fixes contradictions in place)
   - Writes: `story-dossier/deep/characters/resolution-notes.md`
   - Task: Fix each issue identified by the consistency agent, document what was changed and why

### Section 2: World-Building

1. **world_expansion_agent**: Expand world concepts into detailed world bible
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/blueprints/{blueprint_name}.md`
     - ALL `story-dossier/lightweight/*.md`
     - `story-dossier/deep/characters/main-characters.md`
   - Writes: `story-dossier/deep/world/world-bible.md`
   - Task:
     - Read the blueprint and identify ALL sections related to world-building (e.g., world_building, setting, magic systems, locations, rules, etc.)
     - Use those sections as your structural template
     - Create detailed world rules, magic systems, settings, and cultures
     - Ensure world rules enable the tropes and character actions
     - Make the world serve the romance and plot

2. **world_consistency_agent**: Check world rules for contradictions and plot holes
   - Reads: All previous deep/*.md files
   - Writes: `story-dossier/deep/world/consistency-check.md`
   - Reviews:
     - Do world rules make internal sense?
     - Do magic system rules enable the plot without breaking logic?
     - Do settings support the planned scenes?
     - Are there any world-building contradictions?
     - Do world rules conflict with character abilities or plot needs?

3. **world_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/deep/world/world-bible.md`
     - `story-dossier/deep/world/consistency-check.md`
   - Edits: `story-dossier/deep/world/world-bible.md`
   - Writes: `story-dossier/deep/world/resolution-notes.md`

### Section 3: Romance Arc

1. **romance_expansion_agent**: Expand romance arc with detailed beats and scenes
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/blueprints/{blueprint_name}.md`
     - ALL `story-dossier/lightweight/*.md`
     - ALL `story-dossier/deep/*.md` files
   - Writes: `story-dossier/deep/romance/romance-arc-detailed.md`
   - Task:
     - Read the blueprint and identify ALL sections related to romance (e.g., romantasy_elements, romance_tropes, plot_beats, romantic_climax, etc.)
     - Use those sections as your structural template
     - Detail the romance progression chapter by chapter
     - Execute the tropes from input.yaml
     - Ensure romance integrates with plot and world rules
     - Make character motivations drive romantic tension

2. **romance_consistency_agent**: Check romance arc for logic issues
   - Reads: All previous files
   - Writes: `story-dossier/deep/romance/consistency-check.md`
   - Reviews:
     - Does romance pacing feel natural given character personalities?
     - Do romantic actions align with character motivations and wounds?
     - Are conflicts between lovers organic (not forced or based on simple miscommunication)?
     - Does the romance escalate logically?
     - Do tropes execute properly?

3. **romance_resolver_agent**: Fix issues
   - Reads:
     - `story-dossier/deep/romance/romance-arc-detailed.md`
     - `story-dossier/deep/romance/consistency-check.md`
   - Edits: `story-dossier/deep/romance/romance-arc-detailed.md`
   - Writes: `story-dossier/deep/romance/resolution-notes.md`

### Section 4: Plot Structure

1. **plot_expansion_agent**: Expand into detailed plot structure
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/blueprints/{blueprint_name}.md`
     - ALL `story-dossier/lightweight/*.md`
     - ALL `story-dossier/deep/*.md` files
   - Writes: `story-dossier/deep/plot/detailed-structure.md`
   - Task:
     - Read the blueprint and identify ALL sections related to plot structure (e.g., plot_development, plot_beats, subplots, structure, etc.)
     - Use those sections as your structural template
     - Create detailed act breakdowns with chapter assignments
     - Map major plot points, turning points, and climaxes
     - Develop subplots that enhance the main story
     - Ensure plot serves character arcs and romance progression
     - Verify pacing across all acts

2. **plot_consistency_agent**: Check for plot holes and pacing issues
   - Reads: ALL files in story-dossier/deep/
   - Writes: `story-dossier/deep/plot/consistency-check.md`
   - Reviews:
     - Are there any plot holes?
     - Does pacing work across all acts?
     - Do subplots resolve satisfyingly?
     - Does plot contradict world rules, character abilities, or romance progression?
     - Are stakes clear and escalating?
     - Do characters have agency in driving plot forward?

3. **plot_resolver_agent**: Fix issues
   - Reads:
     - `story-dossier/deep/plot/detailed-structure.md`
     - `story-dossier/deep/plot/consistency-check.md`
   - Edits: `story-dossier/deep/plot/detailed-structure.md`
   - Writes: `story-dossier/deep/plot/resolution-notes.md`

### Final Pass: Global Consistency Check

1. **global_consistency_agent**: Review entire dossier for any remaining issues
   - Reads:
     - `story-dossier/input.yaml`
     - ALL `story-dossier/deep/*.md` files
   - Writes: `story-dossier/deep/final-consistency-report.md`
   - Reviews:
     - Cross-section consistency (do all parts work together?)
     - Overall coherence and logic
     - Are tropes from input.yaml properly executed?
     - Does everything serve the core premise?
     - Any remaining contradictions or plot holes?
   - If critical issues found, report to user for manual review
   - If minor issues, document them for author awareness

---

## Instructions for Orchestrator

1. Read `story-dossier/input.yaml` to get the blueprint name
2. Read `story-dossier/workflow-state.json` to verify Phase 1 is complete
3. If Phase 1 incomplete, tell user to run `/generate-story` first
4. For each section (Characters, World, Romance, Plot):
   - **Spawn expansion agent using subagent_type="story-architect"** with blueprint instructions
   - Wait for completion, report progress to user
   - **Spawn consistency agent using subagent_type="story-architect"**
   - Wait for completion, report progress to user
   - **Spawn resolver agent using subagent_type="story-architect"**
   - Wait for completion, report progress to user
   - Update workflow-state.json with completed agents
5. **Spawn global consistency agent using subagent_type="story-architect"**
6. When complete:
   - Set workflow-state.json: `"phase": "deep_complete"`, `"deep_complete": true`
   - Tell user: "Phase 2 complete! Your story dossier is ready in story-dossier/deep/. Review the consistency reports to see what was validated and fixed."

**Important**: All agents must use the story-architect subagent type. This agent is specifically designed for creative story development work (not code).

**Use extended thinking mode when spawning agents to ensure quality.**

**Start now.**
