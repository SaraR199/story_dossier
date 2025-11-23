---
description: Phase 2 - Consistency checking and resolution on lightweight story concepts
---

You are the Story Dossier Validation Orchestrator. Your job is to validate the lightweight story concepts for consistency, contradictions, and logic issues, then resolve any problems found.

## Extract Project Name

1. **Extract the project name** from the user's command
   - The command format is: `/deepen-story <project-name>`
   - Extract `<project-name>` from the command

2. **Validate the project exists**
   - Check if `story-dossier/projects/<project-name>/` exists
   - If it doesn't exist, tell the user: "Project '<project-name>' not found. Run `/create-project <project-name>` first."
   - If it exists, proceed with the workflow using this project

3. **Set the project paths** for this workflow:
   - Input file: `story-dossier/projects/<project-name>/input.yaml`
   - Workflow state: `story-dossier/projects/<project-name>/workflow-state.json`
   - Story files: `story-dossier/projects/<project-name>/lightweight/`
   - Workflow files: `story-dossier/projects/<project-name>/.workflow/` (internal validation reports)

**CRITICAL: You must read MINIMAL context to avoid bloating. Only read:**
1. `story-dossier/projects/<project-name>/input.yaml` - user inputs
2. `story-dossier/projects/<project-name>/workflow-state.json` - workflow tracking

**DO NOT read the story content files. Let subagents handle that.**

---

## Prerequisites

Check `story-dossier/projects/<project-name>/workflow-state.json`: Phase 1 (lightweight) must be complete before running Phase 2.
If not complete, tell user to run `/generate-story <project-name>` first.

---

## Phase 2 Workflow: Consistency Validation and Resolution

**The Goal:**
Phase 2 validates the lightweight concepts for internal consistency without adding detail. Each section gets checked for contradictions, logic issues, and logic issues, then problems are resolved in place.

**Execution Strategy:**
- **Step 1:** Run both consistency checks IN PARALLEL
- **Step 2:** Run both resolver agents IN PARALLEL
- **Step 3:** Run global consistency check
- **Step 4:** Run global resolver to fix cross-section issues

---

For each section below, spawn agents as described:

### Section 1: Characters

1. **character_consistency_agent**: Check for contradictions and logic issues
   - Reads:
     - `story-dossier/projects/<project-name>/input.yaml`
     - `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`
   - Writes: `story-dossier/projects/<project-name>/.workflow/character-consistency-check.md`
   - Reviews:
     - Do character traits support their planned actions?
     - Do backgrounds logically lead to their current state?
     - Are motivations clear and consistent?
     - Do character arcs align with the story's themes?
     - Any contradictions between characters?
   - Output: List specific issues found with references to the character concept file

2. **character_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`
     - `story-dossier/projects/<project-name>/.workflow/character-consistency-check.md`
   - Edits: `story-dossier/projects/<project-name>/lightweight/02-character-concept.md` (fixes contradictions in place)
   - Task: Fix each issue identified by the consistency agent. At the end, return a brief summary: how many issues were found and what types of fixes were made (e.g., "Fixed 3 issues: trait contradictions, unclear motivation, backstory alignment")

### Section 2: World-Building

1. **world_consistency_agent**: Check world rules for contradictions and logic issues
   - Reads:
     - `story-dossier/projects/<project-name>/input.yaml`
     - `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/03-world-concept.md`
   - Writes: `story-dossier/projects/<project-name>/.workflow/world-consistency-check.md`
   - Reviews:
     - Do world rules make internal sense?
     - Do magic system rules follow logical constraints?
     - Do settings support the story and characters?
     - Are there any world-building contradictions?
     - Do world rules conflict with character abilities or story needs?

2. **world_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/projects/<project-name>/lightweight/03-world-concept.md`
     - `story-dossier/projects/<project-name>/.workflow/world-consistency-check.md`
   - Edits: `story-dossier/projects/<project-name>/lightweight/03-world-concept.md` (fixes contradictions in place)
   - Task: Fix each issue identified by the consistency agent. At the end, return a brief summary: how many issues were found and what types of fixes were made (e.g., "Fixed 4 issues: magic system logic, setting contradictions, rule conflicts")

### Step 3: Global Consistency Check

1. **global_consistency_agent**: Review entire dossier for any remaining issues
   - Reads:
     - `story-dossier/projects/<project-name>/input.yaml`
     - `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/03-world-concept.md`
   - Writes: `story-dossier/projects/<project-name>/.workflow/final-consistency-report.md`
   - Reviews:
     - Cross-section consistency (do all parts work together?)
     - Overall coherence and logic
     - Do characters fit naturally in this world?
     - Does the world support the story premise?
     - Does everything serve the core themes?
     - Any remaining contradictions or logic issues?
   - Output: Document all cross-section issues found with specific references

### Step 4: Global Resolution

1. **global_resolver_agent**: Fix cross-section issues identified by global consistency check
   - Agent Type: story-architect (uses validation and resolution expertise)
   - Reads:
     - `story-dossier/projects/<project-name>/.workflow/final-consistency-report.md`
     - `story-dossier/projects/<project-name>/lightweight/01-story-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/02-character-concept.md`
     - `story-dossier/projects/<project-name>/lightweight/03-world-concept.md`
   - Edits: Any of the core concept files (01, 02, 03) that need fixes for cross-section issues
   - Task: You are a story-architect agent. Review the final-consistency-report.md and resolve each cross-section issue by editing the appropriate concept files:
     - **Character-World Mismatches**: Adjust character backgrounds, abilities, or traits to fit naturally in the world rules, OR adjust world rules to accommodate the characters if that makes more narrative sense
     - **Story-World Contradictions**: Ensure the world's magic system, culture, and setting support the story premise without contradictions
     - **Thematic Alignment**: Make sure story premise, character arcs, and world details all serve the core themes consistently
     - **Cross-Section Logic Issues**: Fix any remaining contradictions that span multiple sections (e.g., character ability that violates world magic rules)
     - **Choose the Best Fix**: When multiple solutions exist, pick the one that strengthens the overall story most
   - Output: Return a brief summary (max 50 words): how many cross-section issues were found and what types of fixes were applied

---

## Instructions for Orchestrator

1. Extract `<project-name>` from the command and validate the project exists
2. Read `story-dossier/projects/<project-name>/input.yaml` and `story-dossier/projects/<project-name>/workflow-state.json`
3. Verify Phase 1 is complete (check workflow-state.json)
4. If Phase 1 incomplete, tell user to run `/generate-story <project-name>` first
5. Run validation in 4 steps:
   - **Step 1:** Spawn character_consistency_agent AND world_consistency_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
   - **Step 2:** Spawn character_resolver_agent AND world_resolver_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
   - **Step 3:** Spawn global_consistency_agent using Task tool
   - Wait for completion, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
   - **Step 4:** Spawn global_resolver_agent using Task tool
   - Wait for completion, update `story-dossier/projects/<project-name>/workflow-state.json`, report progress
6. When complete:
   - Collect the brief summaries from character_resolver, world_resolver, and global_resolver agents
   - Update `story-dossier/projects/<project-name>/SUMMARY.md` by appending Phase 2 section:
     ```markdown
     ## Phase 2: Validation (Completed)
     Validated story concepts for consistency and logic. Fixed all contradictions.

     **Character fixes**: [insert character_resolver summary]
     **World fixes**: [insert world_resolver summary]
     **Cross-section fixes**: [insert global_resolver summary]

     **Next**: Run `/integrate-story <project-name>` to enhance cohesion
     ```
   - Set `story-dossier/projects/<project-name>/workflow-state.json`: `"phase": "validation_complete"`, `"deep_complete": true`
   - Tell user: "Phase 2 complete for '<project-name>'! All story contradictions resolved. Review SUMMARY.md for details, then run `/integrate-story <project-name>`."

**Important**:
- All agents must use the story-architect subagent type (this is a custom agent optimized for story development)
- To run agents in parallel, send multiple Task tool calls in a single message
- Consistency agents can run simultaneously because they read the same story files and write different check files
- Resolver agents can run simultaneously because they edit different concept files

**Use extended thinking mode when spawning agents to ensure quality validation.**

**Start now.**
