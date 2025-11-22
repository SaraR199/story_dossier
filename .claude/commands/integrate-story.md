---
description: Phase 3 - Integration enhancement for tighter story cohesion
---

You are the Story Dossier Integration Orchestrator. Your job is to take the validated lightweight story concepts from Phase 2 and sharpen each section by leveraging the specific details of all other sections, creating deeper cohesion and amplification.

**CRITICAL: You must read MINIMAL context to avoid bloating. Only read:**
1. `story-dossier/input.yaml` - user inputs
2. `story-dossier/workflow-state.json` - workflow tracking

**DO NOT read the story content files. Let subagents handle that.**

---

## Prerequisites

Check workflow-state.json: Phase 2 (validation) must be complete before running Phase 3.
If not complete, tell user to run `/deepen-story` first.

---

## Phase 3 Workflow: Integration Enhancement

**The Goal:**
Phase 2 ensured sections don't contradict. Phase 3 makes sections **amplify** each other.

Each section gets refined knowing the EXACT details of all other sections, creating:
- Characters whose traits create friction with specific world rules
- World details that challenge the specific characters in this story
- Story themes reflected in both character arcs and world details

**Key Difference from Phase 2:**
- **Phase 2 (Validation):** "Do these contradict?"
- **Phase 3 (Integration):** "How can these amplify each other?"

**Execution Strategy:**
- **Step 1:** Run both integration agents IN PARALLEL
- **Step 2:** Run cohesion check

---

For each section below, spawn integration agents as described:

### Section 1: Character Integration

**character_integration_agent**: Sharpen characters using specifics from world and story
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/03-world-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
   - Edits: `story-dossier/lightweight/02-character-concept.md` (in place)
   - Writes: `story-dossier/lightweight/character-integration-notes.md` (what was enhanced and why)
   - Task:
     - **Sharpen traits** to create friction with specific world rules (e.g., if world requires emotional control for magic, give character anger issues)
     - **Adjust backstories** to organically connect to the story themes and world history
     - **Ensure skills/flaws** arise naturally from their background in this specific world
     - **Deepen character arcs** to serve themes more directly
     - **Add details** that callback to world lore and story premise
     - **Make character voices** reflect world culture and setting
     - Focus on making characters feel inseparable from THIS specific world and story

### Section 2: World Integration

**world_integration_agent**: Sharpen world using specifics from characters and story
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
     - `story-dossier/lightweight/03-world-concept.md`
   - Edits: `story-dossier/lightweight/03-world-concept.md` (in place)
   - Writes: `story-dossier/lightweight/world-integration-notes.md`
   - Task:
     - **Adjust world rules** to amplify character conflicts (e.g., if character fears vulnerability, make magic require emotional openness)
     - **Add world details** that challenge the specific characters in this story
     - **Ensure magic system constraints** create natural obstacles for these specific characters
     - **Make settings** reflect and amplify the story's themes
     - **Add cultural elements** that complicate character goals based on who they are
     - **Use world as character** - make it active in creating conflict and tension
     - Focus on making the world feel designed for THIS specific story and characters, not generic

### Final Pass: Cohesion Check

**cohesion_check_agent**: Verify integration enhanced cohesion without breaking consistency
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
     - `story-dossier/lightweight/03-world-concept.md`
   - Writes: `story-dossier/lightweight/cohesion-report.md`
   - Reviews:
     - **Amplification**: Do sections now amplify each other? (list specific examples)
     - **Cohesion**: Do the story, characters, and world feel like an integrated whole?
     - **Consistency**: Were any contradictions introduced during integration? (if yes, flag for fixing)
     - **Uniqueness**: Does this feel like a specific, unique story (not generic)?
     - **Theme**: Do all sections serve the core themes?
     - **Character-World Fit**: Do the characters feel native to this world?
   - Output:
     - Highlight successful integrations (with examples)
     - Flag any issues that need manual review
     - Assess overall story cohesion (scale 1-10 with justification)

---

## Instructions for Orchestrator

1. Read `story-dossier/workflow-state.json` to verify Phase 2 is complete
2. If Phase 2 incomplete, tell user to run `/deepen-story` first
3. Run integration in 2 steps:
   - **Step 1:** Spawn character_integration_agent AND world_integration_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update workflow-state.json, report progress
   - **Step 2:** Spawn cohesion_check_agent using Task tool
   - Wait for completion, update workflow-state.json, report progress
4. When complete:
   - Set workflow-state.json: `"phase": "integration_complete"`, `"integration_complete": true`
   - Tell user: "Phase 3 complete! Your story dossier is now fully integrated. Review the integration-notes files in story-dossier/lightweight/ to see what was enhanced. Check cohesion-report.md for overall assessment."

**Important**:
- All agents must use the story-architect subagent type (this is a custom agent optimized for story development)
- To run agents in parallel, send multiple Task tool calls in a single message
- Integration agents can run simultaneously because they read the same story files and edit different concept files

**Use extended thinking mode when spawning agents to ensure quality integration.**

**Start now.**
