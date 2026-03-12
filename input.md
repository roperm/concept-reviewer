# Manufacturing Process Concept Review Panel — Input Instructions

This document defines how users must provide input to the Manufacturing Process Concept Review Panel GPT. The goal of the input structure is to ensure the expert panel receives enough information to perform a meaningful and disciplined review while minimizing assumptions and ambiguity.

The GPT will transform the provided information into a structured concept package and send it to the backend review panel.

Users should provide information using the sections below. If certain information is unavailable, the user should state **"Unknown"** rather than omitting the field.

---

# Input Format

Users should provide input using the following structure.

Each section is described in detail below.

---

# Section Descriptions

## Concept Title

A short name used to identify the concept being reviewed.

Examples:

- Catheter Tip Bonding Station
- Semi-Automated Adhesive Dispense Workstation
- Vision-Guided Assembly Fixture

This title will appear in the final review report.

---

## Process Step or Workstation Purpose

Describe the specific manufacturing step that this concept performs.

Examples:

- Dispensing UV adhesive and bonding catheter components
- Aligning and pressing two plastic housings together
- Inspecting molded parts for cosmetic defects

The description should explain **what the station is intended to accomplish**, not how it works.

---

## Product Description

Provide a brief description of the product being manufactured.

Relevant details may include:

- product type
- size or scale of components
- materials involved
- critical functional features

Examples:

- Disposable polymer catheter assembly
- Small plastic valve component
- Stainless steel surgical tool

This information helps experts evaluate manufacturability and inspection requirements.

---

## Concept Description

Describe the proposed manufacturing concept in detail.

Include information such as:

- how parts enter the station
- how parts are positioned or fixtured
- what operations are performed
- how the operator interacts with the process
- how parts exit the station

The concept description should explain **the intended workflow and mechanism of the station**.

If available, include:

- step-by-step operation sequence
- automation elements
- sensors or verification methods
- operator responsibilities

---

## Production Requirements

Provide the production expectations for the process.

Include:

- target production volume
- expected cycle time or takt time
- number of operators
- level of automation (manual, semi-automated, automated)

Examples:

- 120 units per hour
- Single operator station
- Semi-automated workstation with pneumatic actuation

This information helps the panel evaluate throughput feasibility and process design.

---

## Quality Requirements

Describe the product quality requirements relevant to this process step.

Examples:

- dimensional tolerances
- cosmetic inspection criteria
- bond strength requirements
- leak testing requirements
- presence verification

Examples:

- Bond alignment tolerance ±0.25 mm
- Adhesive must not enter catheter lumen
- Cosmetic surfaces must be free of scratches

Quality information allows the Quality Engineer and Process Engineer to evaluate inspection and control strategies.

---

## Safety Considerations

Identify known safety factors associated with the process.

Examples:

- UV light exposure
- heated components
- sharp tools
- moving mechanisms
- chemical exposure

If hazards are unknown, indicate that safety considerations have **not yet been evaluated**.

This section helps the Safety Expert identify potential hazards.

---

## Equipment / Automation Assumptions

Describe assumptions about equipment used in the concept.

Examples:

- pneumatic press
- robotic arm
- vision inspection system
- custom mechanical fixture
- manual operator placement

Include any assumptions about sensors, controls, or automation systems.

---

## Facility Constraints

Provide any known physical or environmental limitations.

Examples:

- available floor space
- available utilities (air, power, vacuum)
- cleanroom requirements
- ESD requirements
- lighting constraints

Examples:

- 6 ft × 8 ft available footprint
- 120V electrical service
- compressed air available
- ISO Class 8 cleanroom

These constraints help the panel assess feasibility.

---

## Known Risks or Concerns

List any concerns already identified by the concept designer.

Examples:

- adhesive placement accuracy may be difficult
- operator loading may cause alignment variability
- fixture complexity may increase maintenance

Providing known risks helps the panel focus on areas that may need deeper evaluation.

---

## Current Process (if applicable)

If the concept is intended to replace or improve an existing process, describe the current method.

Include:

- how the process currently operates
- known problems with the current process
- limitations that the new concept is intended to address

Examples:

- manual adhesive dispensing currently causes variability
- operator alignment step frequently causes defects
- current inspection step is slow

---

## Additional Notes

Provide any other information that may help the panel evaluate the concept.

Examples:

- development timeline
- cost constraints
- validation requirements
- compatibility with existing equipment

---

## Attachments (optional)

Users may attach supporting materials such as:

- sketches
- CAD images
- diagrams
- photos of prototypes
- process flow diagrams

Attachments can significantly improve the panel’s ability to evaluate the concept.

---

---

# Important Guidelines

To ensure accurate evaluation:

- Provide **clear and detailed descriptions**
- Avoid vague statements such as "standard process"
- Specify **quantitative values when possible**
- Clearly identify **unknowns or assumptions**

The quality of the panel’s review will depend heavily on the quality of the information provided.

Incomplete information may reduce expert confidence in their assessments.
