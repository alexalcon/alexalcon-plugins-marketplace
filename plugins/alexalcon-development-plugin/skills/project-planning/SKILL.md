---
name: project-planning
description: Generate a simple and well-structured project plan for any software development project, including Notion database templates to track and document the full development workflow from planning through requirements, task breakdown, implementation, testing, and delivery. Use this skill when starting, organizing, or managing any software development project. 
---

# Software Project Planning

This skill helps generate a comprehensive project plan for software development projects, ensuring all stages from planning to delivery are well-documented and tracked using Notion database templates.

## When to Use This Skill

Use this skill when you need to create a structured project plan for a software development project, including tracking and documenting all stages from planning to delivery using Notion database templates. This ensures that all tasks, requirements, and progress are clearly defined and easily accessible, monitored, managed and well documented throughout the project lifecycle.

## Inputs (Parameters)

### Entry input

The entry input for this skill is the information about the software development (app) project, which will be gathered from the user entry prompt.

### Detailed inputs

Ask all the questions you need to know about the software development (app) project to provide the best possible project plan. The answers of this questioning process will be the inputs used to create a detailed and accurate project plan. Thus, follow a thorough and systematic questioning approach to gather all necessary information before generating the project plan, based on the following guidelines.


- Ask one question at a time. 
- Wait for the answer to each question before asking the next one. 
- Do not limit the number of questions.
- Ensure that all gathered information is complete and accurate before proceeding to generate the project plan.
- The following questions must be asked to gather all necessary information about the project:
  - Ask the current knowledge and experience level of the developer.
  - Ask about the project goals and objectives.
  - Ask about the target audience or users.
  - Ask about the project scope and deliverables.
  - Ask about the timeline and deadlines.
  - Ask about the available resources and budget.
  - Ask about potential risks and challenges.
  - Ask about any specific technologies or tools to be used.

Only provide the project plan once you have asked all the questions you consider necessary.

## Instructions (Steps by Step Instructions)

Once gathered all the necessary information about the project, proceed to generate a comprehensive project plan file that includes all stages from planning to delivery, ensuring that each aspect is well-documented and tracked using Notion database templates. 

### Step 1: Generate Project Plan

- The project plan should include sections for project goals, scope, deliverables, timeline, resources, budget, risks, and any specific technologies or tools to be used. 
- Organize the project plan in structured phases to be easily followed and tracked throughout the project lifecycle, e.g., Phase0, Phase1, Phase2, etc, and then add it to the project plan file.
- Ensure that the project plan is clear, concise, and easy to follow, providing a structured roadmap for the successful execution and completion of the project.
- The format of the project plan file is up to you (make it professional, like a real world project plan).
- Create the project plan file according to the rules and constraints specified above.

### Step 2: Notion Database Generation and Integration

- Once the project plan is generated and saved, proceed to create and integrate the corresponding Notion databases. 
- Build the Notion databases using this agent skill from '../notion-database/SKILL.md' according to the project name and the phase list in the project plan.

### Step 3: Project Phase Build Checklist 

- Create a checklist for each project phase based on the project plan.
- Ensure that each checklist item is actionable and clearly defines the tasks to be completed in that phase.
- For the project phase checklist use the example link in `./references/notion_project_phase_checklists_example.txt` and create a Notion page item (inside the view Docs) in the `Development Notes` DB from the generated Notion databases. I want the same format as the example link.

### Step 4: Project Plan Summary Report in Notion Page Item

- Create a summary report of the project plan as a Notion page item (inside the view Docs) in the `Development Notes` DB from the generated Notion databases.

## Rules and Constraints

- The project plan must be created as a LaTeX file and include both the code and the compiled PDF.
- The project plan file must be located in the `assets/project` directory in the project's root, creating the directory if it does not exist.
- For the project phases generation look at the example link in `./references/notion_project_phases_example.txt`. I want the same format, organization, structure and workflow from this example, always sarting with a "Phase 0 — Foundations".


## Outputs Format (Result(s))

- The project plan file should include all sections and phases as specified in the instructions and follow the structure outlined in the example links.
- The project plan file format it is up to you, but it should be professional and suitable for real-world project planning.

## Examples (Sample Inputs and Outputs)

- See `./references/notion_project_dbs_example.txt` to grab a reference idea of a project managemet structure.
- See `./references/notion_project_phase_checklists_example.txt` to grab a reference idea of a project phase checklist structure.
