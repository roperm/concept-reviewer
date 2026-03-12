# Architecture Principles — Concept Reviewer

This document defines the architectural principles for the **Concept Reviewer** system. These principles are intended to guide system design, implementation, testing, and future expansion.

The purpose of these principles is to keep the system:

- deterministic
- maintainable
- testable
- auditable
- extensible

The system reviews manufacturing process concepts using a GPT frontend, a backend panel of expert agents, a Panel Chair synthesis agent, and a PDF reporting pipeline.

---

# 1. Thin Frontend, Smart Backend

The GPT frontend must remain a **thin interaction layer**.

Its responsibilities are limited to:

- guiding the user to provide input in the required format
- clarifying missing critical information when needed
- packaging the input for the backend
- calling the backend API
- presenting the returned results

The GPT frontend must **not perform the actual expert review**.

All engineering reasoning, expert analysis, synthesis, and report generation must be handled in the backend.

## Rationale

This keeps the core logic:

- version-controlled
- testable
- reproducible
- independent of GPT Builder configuration drift

---

# 2. Independent Expert Evaluation

Each expert agent must evaluate the concept **independently**.

Each expert must:

- receive the same concept input
- use only its own role instructions
- avoid seeing the outputs of the other experts before completing its own review
- remain focused on its own discipline

## Rationale

This reduces:

- cross-contamination of opinions
- premature consensus
- redundant outputs
- panel groupthink

The system is intended to simulate a structured multidisciplinary design review. Independence is essential to that objective.

---

# 3. Strict Role Boundaries

Each expert must remain within its domain.

Experts may mention cross-functional issues only when they directly affect their area of responsibility.

Examples:

- Safety may discuss motion hazards, ergonomics, energy exposure, and safeguarding.
- Quality may discuss detectability, inspection strategy, defect prevention, and traceability.
- Mechanical may discuss fixture robustness, tolerance sensitivity, and reliability.

Experts must not act as generalists.

## Rationale

Without role boundaries, all agents tend to produce similar output. That weakens the value of the panel.

---

# 4. Structured Inputs and Outputs

All system interfaces must use structured schemas.

This applies to:

- concept input payloads
- expert review outputs
- panel chair outputs
- backend API responses

The system should avoid relying on freeform text parsing wherever possible.

## Rationale

Structured schemas improve:

- validation
- consistency
- error handling
- PDF generation
- maintainability
- future integration with other systems

---

# 5. Source-of-Truth Documentation

The following repository files are authoritative requirements:

- `experts.md`
- `input.md`
- `output.md`

Implementation decisions must align with these files unless there is a clear and documented reason to extend them.

## Rationale

This prevents drift between:

- design intent
- prompts
- schemas
- implementation

The codebase should reflect the documented requirements rather than creating a second, conflicting specification.

---

# 6. Backend-Orchestrated Agent Flow

The backend must explicitly orchestrate the review process.

Required sequence:

1. receive concept input
2. validate input
3. run expert agents
4. collect outputs
5. run Panel Chair synthesis
6. generate PDF
7. return results

The orchestration logic must live in backend code, not in the GPT frontend.

## Rationale

Explicit orchestration improves:

- observability
- error handling
- reproducibility
- debugging
- test coverage

---

# 7. Parallel Expert Execution

Expert agents should run in parallel unless there is a strong operational reason not to.

## Rationale

Parallel execution:

- reduces latency
- preserves independence
- simplifies orchestration logic
- keeps expert evaluation symmetric

The system should not serialize experts unless required by future constraints.

---

# 8. Panel Chair as Synthesizer, Not Reviewer

The Panel Chair must not behave like another expert reviewer.

The Panel Chair must:

- organize the expert results
- summarize points of agreement
- summarize points of disagreement
- identify major issues
- provide a final panel-level recommendation

The Panel Chair must **not**:

- rewrite expert opinions
- override expert statements
- inject new expert analysis that was not supported by the panel inputs

## Rationale

The value of the chair role is clarity and synthesis, not additional domain judgment.

---

# 9. Preserve Original Expert Opinions

The original expert outputs must remain intact throughout the system.

The PDF report and any structured output should preserve the expert assessments as originally returned, except for formatting normalization needed for display.

## Rationale

This improves:

- auditability
- traceability
- trustworthiness
- review fidelity

Users should be able to distinguish between:

- what an expert said
- what the Panel Chair summarized

---

# 10. Deterministic Structure Over Creative Flourish

The system should favor:

- disciplined formatting
- explicit assumptions
- stable section ordering
- structured recommendations
- concise technical language

The system should avoid:

- unnecessary stylistic variation
- excessive narrative expansion
- overly creative phrasing
- vague praise or filler

## Rationale

This is an engineering review system, not a general brainstorming tool.

Consistency matters more than stylistic variety.

---

# 11. Validate Early, Fail Clearly

The backend must validate input before expert execution begins.

Validation should catch:

- missing required fields
- malformed payloads
- unsupported values
- structurally incomplete requests

Validation errors should be explicit and actionable.

## Rationale

Clear validation prevents wasted LLM calls and reduces ambiguous failures later in the workflow.

---

# 12. PDF Is the Official Deliverable

The final concept review must be generated as a **PDF**.

The PDF is the official output artifact for:

- review sharing
- design documentation
- project records
- stakeholder communication

The structured JSON outputs are internal system artifacts. The PDF is the formal review deliverable.

## Rationale

This aligns the system with real engineering workflows, where review outputs are often archived and distributed as documents.

---

# 13. Separate Prompt Content from Execution Logic

Prompt text should be stored separately from orchestration code whenever practical.

Examples:

- expert prompt files in an `agents/` directory
- GPT instructions in `frontend_gpt/`
- backend execution logic in `backend/`

## Rationale

This improves:

- maintainability
- prompt iteration
- readability
- testing
- version control clarity

Prompt updates should not require large code rewrites.

---

# 14. Prefer Explicit Assumptions Over Silent Guessing

When information is missing, the system should either:

- ask for critical missing information, or
- proceed using clearly labeled assumptions

The system should not silently invent key manufacturing details.

## Rationale

The review must remain credible. Hidden assumptions reduce trust.

---

# 15. No Hidden Synthesis in Expert Prompts

Expert prompts must not instruct experts to produce panel-level conclusions.

Experts must not:

- summarize likely consensus
- predict what other experts would think
- provide final go/no-go decisions for the panel as a whole

## Rationale

That would blur the distinction between expert review and synthesis.

---

# 16. No Additional Aggregation Layer Unless Required Later

The system should not introduce extra coordination layers beyond:

- expert agents
- Panel Chair
- PDF generation

In particular, do **not** add a separate risk aggregator component unless future requirements explicitly justify it.

## Rationale

The current review flow is simpler, easier to test, and aligned with the intended design.

---

# 17. Clear Mapping from Data to Report

Every field in the final PDF should map clearly back to a backend data structure.

Examples:

- concept title → input schema
- expert row values → expert output schema
- chair recommendation → panel summary schema

## Rationale

This simplifies report generation and reduces ambiguity during implementation.

---

# 18. Favor Testability in All Components

Each major component should be testable in isolation.

Components should include:

- input validation
- expert execution wrapper
- chair execution wrapper
- orchestration logic
- PDF generation

## Rationale

Testable components make the system easier to debug and safer to extend.

---

# 19. Logging Must Support Review Traceability

The backend should log enough information to support debugging and auditability.

Recommended logging points:

- request received
- validation result
- expert execution started/completed
- panel chair execution started/completed
- PDF generation started/completed
- final response returned

Sensitive information should be handled appropriately.

## Rationale

This is especially important for a system intended to support engineering decisions.

---

# 20. Version 1 Should Stay Narrow

Version 1 should remain focused on:

- text-based concept review
- independent expert assessment
- panel synthesis
- PDF report generation

Version 1 should avoid adding:

- cost estimation
- simulation
- CAD parsing
- vendor comparison
- dynamic tool use
- autonomous planning features

## Rationale

A narrower version 1 is more likely to work well and reveal the right next improvements.

---

# 21. Extensibility Without Premature Complexity

The architecture should support future additions such as:

- more expert types
- different review modes
- image attachments
- richer UI
- additional report templates

However, these future possibilities should not complicate version 1 unnecessarily.

## Rationale

The design should be extensible, but current implementation should stay controlled and practical.

---

# 22. Responses API as the Core LLM Interface

The system should use the **OpenAI Responses API directly** for version 1.

Each expert agent and the Panel Chair should be implemented as a separate Responses API call with its own system prompt and structured output handling.

## Rationale

This provides:

- simpler implementation
- explicit orchestration control
- easier debugging
- clean separation of roles
- better fit for a fixed review pipeline

The Agents SDK can be reconsidered later if the system becomes substantially more dynamic.

---

# 23. One Direction of Authority

Authority should flow in this order:

1. requirement documents
2. schemas
3. prompts
4. orchestration logic
5. rendered output

Rendered output must never become the de facto source of truth.

## Rationale

This preserves consistency and prevents implementation details from reshaping requirements accidentally.

---

# 24. Design for Maintainers, Not Just for Models

The repository should be understandable to a developer who did not write the original system.

Use:

- clear file names
- explicit schemas
- modular code
- readable prompts
- documented interfaces

## Rationale

This system should be maintainable by humans over time, not only generatable by models.

---

# Final Principle

The Concept Reviewer system should behave like a **disciplined engineering review workflow**, not like a freeform chat experience.

Every architectural decision should support:

- structured expert independence
- trustworthy synthesis
- consistent outputs
- implementation clarity
- practical use in engineering review contexts
