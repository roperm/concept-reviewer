# Build Prompt — Concept Reviewer System (Production v1)

You are to generate the complete production-ready **Concept Reviewer** system from the repository requirement documents.  
Your job is to produce the full implementation, repository structure, prompts, schemas, frontend GPT instructions, backend orchestration, PDF generation, and supporting documentation.

You must treat the repository requirement documents as the authoritative source of truth.

The repository is found here: https://github.com/roperm/concept-reviewer

---

## Source-of-Truth Files

Use these files as authoritative requirements:

1. `README.md`
2. `repo_context.md`
3. `architecture_principles.md`
4. `experts.md`
5. `input.md`
6. `output.md`
7. `review_criteria.md`

If any requirement appears ambiguous, resolve it by following this order of authority:

1. requirement documents
2. schemas
3. prompts
4. orchestration logic
5. rendered output

Do **not** invent a competing specification.

---

## High-Level Objective

Build a full working system called **Concept Reviewer** that reviews manufacturing process concepts using:

- a **thin GPT frontend**
- a **backend API**
- **five independent expert agents**
- a **Panel Chair synthesis agent**
- a **PDF report generation pipeline**
- structured schemas and validation
- deterministic orchestration using the **OpenAI Responses API**

The system must accept structured manufacturing concept input, run independent expert reviews in parallel, synthesize results through a Panel Chair, and generate a formal PDF report as the official deliverable.

---

## Critical Architectural Requirements

You must follow these rules exactly:

### 1. Thin frontend, smart backend
The GPT frontend must only:
- guide the user into the required input format
- identify missing critical information
- package the concept input
- call the backend API
- return the result to the user

The GPT frontend must **not** perform the expert review itself.

### 2. Independent expert evaluation
Each expert must:
- receive the same concept input
- evaluate independently
- not see other experts' outputs before finishing
- remain confined to its own domain

### 3. Strict role boundaries
Experts must stay within their discipline and avoid becoming generalists.

### 4. Structured schemas
All interfaces must use explicit structured schemas:
- concept input payload
- expert outputs
- panel chair output
- backend API request/response
- report data model

### 5. Backend-orchestrated flow
The backend must explicitly orchestrate:
1. receive request
2. validate input
3. run experts in parallel
4. collect expert results
5. run Panel Chair synthesis
6. generate PDF
7. return result

### 6. Preserve original expert opinions
The system must preserve original expert outputs.  
The Panel Chair may synthesize them but must not rewrite or override them.

### 7. Deterministic engineering-style outputs
Favor:
- concise technical language
- stable section ordering
- explicit assumptions
- clear validation
- structured output

Avoid:
- filler
- vague praise
- creative writing style
- hidden assumptions

### 8. PDF is the official output
The formal review output must be a PDF.

### 9. Responses API for v1
Use the **OpenAI Responses API directly** for expert calls and Panel Chair calls.  
Do **not** use the Agents SDK for v1.

### 10. Keep v1 narrow
Do **not** add:
- CAD parsing
- simulation
- cost estimation
- vendor comparison
- dynamic tool use
- autonomous planning features

---

## Expert Roles to Implement

Implement these expert agents exactly as independent reviewers:

1. **Process Engineer**
2. **Mechanical Engineer**
3. **Quality Engineer**
4. **Safety Expert**
5. **Technician**

Also implement:

6. **Panel Chair** — synthesis only, not an additional technical reviewer

Use `experts.md` and `review_criteria.md` to derive the exact role instructions and focus areas.

---

## Required User Input Model

Build the system around the input structure defined in `input.md`.

The concept input must support at least these fields:

- concept_title
- process_step_or_workstation_purpose
- product_description
- concept_description
- production_requirements
- quality_requirements
- safety_considerations
- equipment_automation_assumptions
- facility_constraints
- known_risks_or_concerns
- current_process
- additional_notes
- attachments

Rules:
- unknown information should be allowed explicitly as `"Unknown"`
- missing required fields should be validated clearly
- attachments are optional in v1 and may be accepted structurally even if not deeply processed

---

## Required Expert Output Model

Each expert must return a structured object corresponding to the output table in `output.md`.

Each expert row must contain:

- `expert`
- `overall_assessment`
- `key_strengths`
- `key_concerns`
- `major_risks`
- `recommended_changes`
- `key_questions`
- `confidence`

Each field should be consistently typed and suitable for rendering into the final report table.

Use arrays for multi-item fields where practical.

---

## Required Panel Chair Output Model

The Panel Chair must take:
- the normalized concept input
- all completed expert outputs

The Panel Chair must return structured synthesis containing at minimum:

- `concept_overview`
- `panel_consensus_observations`
- `areas_of_disagreement`
- `most_critical_issues`
- `overall_panel_recommendation`
- `supporting_notes` (optional)

The Panel Chair must **not**:
- invent new expert opinions
- overwrite expert statements
- behave as a sixth technical evaluator

Allowed recommendation values:

- `Proceed`
- `Proceed with revisions`
- `Rework concept before further development`
- `Reject concept`

---

## PDF Report Requirements

Generate a formal PDF as the final artifact.

The PDF must include these sections in order:

1. **Concept Overview**
2. **Expert Review Table**
3. **Panel Consensus Observations**
4. **Areas of Disagreement**
5. **Most Critical Issues Requiring Attention**
6. **Overall Panel Recommendation**
7. **Supporting Notes** (only if needed)

Rules:
- preserve original expert wording in the expert table
- use clear professional formatting suitable for engineering design review documentation
- ensure every displayed field maps directly to backend data structures
- keep the layout readable and consistent

---

## Technology and Implementation Expectations

Unless there is a strong reason otherwise, use this default stack:

### Backend
- **Python**
- **FastAPI**
- **Pydantic**
- **OpenAI Responses API**
- PDF generation using a stable Python solution such as:
  - ReportLab, or
  - HTML-to-PDF pipeline if implemented cleanly and deterministically

### Frontend GPT integration
Provide:
- GPT system instructions for the frontend
- backend API contract the GPT will call
- example action schema or equivalent integration spec

### Project quality
The repo must be:
- modular
- testable
- readable
- maintainable
- documented

---

## Required Repository Structure

Generate a clean repository structure like this or better:

```text
concept-reviewer/
├─ README.md
├─ .env.example
├─ requirements.txt
├─ pyproject.toml
├─ app/
│  ├─ main.py
│  ├─ config.py
│  ├─ api/
│  │  ├─ routes.py
│  │  └─ models.py
│  ├─ schemas/
│  │  ├─ concept_input.py
│  │  ├─ expert_output.py
│  │  ├─ panel_output.py
│  │  └─ review_bundle.py
│  ├─ agents/
│  │  ├─ base.py
│  │  ├─ process_engineer.py
│  │  ├─ mechanical_engineer.py
│  │  ├─ quality_engineer.py
│  │  ├─ safety_expert.py
│  │  ├─ technician.py
│  │  ├─ panel_chair.py
│  │  └─ prompts/
│  │     ├─ process_engineer.md
│  │     ├─ mechanical_engineer.md
│  │     ├─ quality_engineer.md
│  │     ├─ safety_expert.md
│  │     ├─ technician.md
│  │     └─ panel_chair.md
│  ├─ services/
│  │  ├─ validation.py
│  │  ├─ orchestrator.py
│  │  ├─ openai_client.py
│  │  └─ pdf_generator.py
│  ├─ utils/
│  │  ├─ logging.py
│  │  └─ formatting.py
│  └─ tests/
│     ├─ test_validation.py
│     ├─ test_orchestrator.py
│     ├─ test_pdf_generator.py
│     └─ test_schema_roundtrip.py
├─ frontend_gpt/
│  ├─ gpt_system_instructions.md
│  ├─ action_schema.json
│  └─ usage_notes.md
└─ docs/
   ├─ api_contract.md
   ├─ architecture.md
   ├─ prompt_design.md
   └─ deployment.md
