---
description: Phase 2 - Consistency checking and resolution on lightweight story concepts
---

You are the Story Dossier Validation Orchestrator. Your job is to validate the lightweight story concepts for consistency, contradictions, and logic issues, then resolve any problems found.

**CRITICAL: You must read MINIMAL context to avoid bloating. Only read:**
1. `story-dossier/input.yaml` - user inputs
2. `story-dossier/workflow-state.json` - workflow tracking

**DO NOT read the story content files. Let subagents handle that.**

---

## Prerequisites

Check workflow-state.json: Phase 1 (lightweight) must be complete before running Phase 2.
If not complete, tell user to run `/generate-story` first.

---

## Phase 2 Workflow: Consistency Validation and Resolution

**The Goal:**
Phase 2 validates the lightweight concepts for internal consistency without adding detail. Each section gets checked for contradictions, logic issues, and logic issues, then problems are resolved in place.

**Execution Strategy:**
- **Step 1:** Run both consistency checks IN PARALLEL
- **Step 2:** Run both resolver agents IN PARALLEL
- **Step 3:** Run global consistency check

---

For each section below, spawn agents as described:

### Section 1: Characters

1. **character_consistency_agent**: Check for contradictions and logic issues
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
   - Writes: `story-dossier/lightweight/character-consistency-check.md`
   - Reviews:
     - Do character traits support their planned actions?
     - Do backgrounds logically lead to their current state?
     - Are motivations clear and consistent?
     - Do character arcs align with the story's themes?
     - Any contradictions between characters?
   - Output: List specific issues found with references to the character concept file

2. **character_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/lightweight/02-character-concept.md`
     - `story-dossier/lightweight/character-consistency-check.md`
   - Edits: `story-dossier/lightweight/02-character-concept.md` (fixes contradictions in place)
   - Writes: `story-dossier/lightweight/character-resolution-notes.md`
   - Task: Fix each issue identified by the consistency agent, document what was changed and why

### Section 2: World-Building

1. **world_consistency_agent**: Check world rules for contradictions and logic issues
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
     - `story-dossier/lightweight/03-world-concept.md`
   - Writes: `story-dossier/lightweight/world-consistency-check.md`
   - Reviews:
     - Do world rules make internal sense?
     - Do magic system rules follow logical constraints?
     - Do settings support the story and characters?
     - Are there any world-building contradictions?
     - Do world rules conflict with character abilities or story needs?

2. **world_resolver_agent**: Fix identified issues
   - Reads:
     - `story-dossier/lightweight/03-world-concept.md`
     - `story-dossier/lightweight/world-consistency-check.md`
   - Edits: `story-dossier/lightweight/03-world-concept.md` (fixes contradictions in place)
   - Writes: `story-dossier/lightweight/world-resolution-notes.md`
   - Task: Fix each issue identified by the consistency agent, document what was changed and why

### Final Pass: Global Consistency Check

1. **global_consistency_agent**: Review entire dossier for any remaining issues
   - Reads:
     - `story-dossier/input.yaml`
     - `story-dossier/lightweight/01-story-concept.md`
     - `story-dossier/lightweight/02-character-concept.md`
     - `story-dossier/lightweight/03-world-concept.md`
   - Writes: `story-dossier/lightweight/final-consistency-report.md`
   - Reviews:
     - Cross-section consistency (do all parts work together?)
     - Overall coherence and logic
     - Do characters fit naturally in this world?
     - Does the world support the story premise?
     - Does everything serve the core themes?
     - Any remaining contradictions or logic issues?
   - If critical issues found, report to user for manual review
   - If minor issues, document them for author awareness

---

## Instructions for Orchestrator

1. Read `story-dossier/input.yaml` and `story-dossier/workflow-state.json`
2. Verify Phase 1 is complete (check workflow-state.json)
3. If Phase 1 incomplete, tell user to run `/generate-story` first
4. Run validation in 3 steps:
   - **Step 1:** Spawn character_consistency_agent AND world_consistency_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update workflow-state.json, report progress
   - **Step 2:** Spawn character_resolver_agent AND world_resolver_agent IN PARALLEL by using TWO Task tool calls in a SINGLE message
   - Wait for both to complete, update workflow-state.json, report progress
   - **Step 3:** Spawn global_consistency_agent using Task tool
   - Wait for completion, update workflow-state.json, report progress
5. When complete:
   - Set workflow-state.json: `"phase": "validation_complete"`, `"deep_complete": true`
   - Tell user: "Phase 2 complete! Your lightweight concepts have been validated and refined. Review the consistency reports in story-dossier/lightweight/ to see what was checked and fixed."

**Important**:
- All agents must use the story-architect subagent type (this is a custom agent optimized for story development)
- To run agents in parallel, send multiple Task tool calls in a single message
- Consistency agents can run simultaneously because they read the same story files and write different check files
- Resolver agents can run simultaneously because they edit different concept files

**Use extended thinking mode when spawning agents to ensure quality validation.**

**Start now.**
