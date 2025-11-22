---
description: Create a new story project with initial directory structure
---

You are creating a new story project. Follow these steps:

## Instructions

1. **Extract the project name** from the user's command
   - The command format is: `/create-project <project-name>`
   - Extract `<project-name>` from the command

2. **Validate the project name**
   - Check if `story-dossier/projects/<project-name>/` already exists
   - If it exists, tell the user: "Project '<project-name>' already exists. Choose a different name or use the existing project."
   - If it doesn't exist, proceed

3. **Create the project structure**:
   ```
   story-dossier/projects/<project-name>/
   ├── input.yaml (copied from templates)
   ├── workflow-state.json (initialized)
   └── lightweight/ (empty directory for generated content)
   ```

4. **Execute the following using Bash tool**:
   - Create project directory: `mkdir -p story-dossier/projects/<project-name>/lightweight`
   - Copy template: `cp story-dossier/templates/input.yaml story-dossier/projects/<project-name>/`
   - Initialize workflow state: Create `story-dossier/projects/<project-name>/workflow-state.json` with:
     ```json
     {
       "phase": "not_started",
       "lightweight_complete": false,
       "deep_complete": false,
       "integration_complete": false,
       "completed_agents": [],
       "current_agent": null,
       "validation_status": "pending",
       "errors": [],
       "timestamp": null
     }
     ```

5. **Confirm to user**:
   - Tell them: "✓ Created project '<project-name>'"
   - Tell them: "Next steps:"
   - Tell them: "  1. Fill out story-dossier/projects/<project-name>/input.yaml with your story details"
   - Tell them: "  2. Run `/generate-story <project-name>` to start Phase 1"

**Start now.**
