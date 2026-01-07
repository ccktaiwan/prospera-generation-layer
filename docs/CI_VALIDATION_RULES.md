Prospera Generation Layer — Machine Validation Rules

Document Type: Canonical CI Validation Rules
Layer: Prospera Generation Layer
Status: Canonical (Active)
Version: v0.1.0
Parent Constraints:
- Prospera Generation Layer Schemas v0.1.0
- Prospera Execution Layer v1.0.0
Owner: Prospera Architecture Group
Last Updated: 2026-01-07
Scope: Machine-enforceable validation rules for CI pipelines


Purpose

This document defines mandatory, machine-enforceable validation rules
for Continuous Integration (CI) pipelines consuming the Prospera
Generation Layer.

The rules in this document are normative and binding.
Any deviation MUST be treated as a contract violation.


Validation Philosophy

CI validation is responsible for enforcing structural correctness,
contract stability, and responsibility boundaries.

CI validation MUST NOT:
- Infer intent
- Repair malformed artifacts
- Apply heuristic judgment
- Bypass declared failure semantics

CI validation MUST operate strictly on declared schemas, examples,
and documented boundaries.


Validation Targets

CI pipelines MUST validate the following artifact classes:

1. Schema Artifacts
2. Example Artifacts
3. Adapter Boundary Artifacts
4. Cross-artifact Consistency


Rule Set A — Schema Validation Rules

A1. All JSON schema files under /schemas MUST validate against
    JSON Schema Draft 2020-12.

A2. Schemas MUST NOT contain:
    - Undefined $ref targets
    - Circular $ref dependencies
    - additionalProperties set to true unless explicitly required

A3. Schema filenames and $id values MUST be stable and version-agnostic.


Rule Set B — Example Validation Rules

B1. Every example file under /examples MUST validate against at least
    one declared canonical schema.

B2. Examples declared as PASS MUST validate successfully.

B3. Examples declared as FAILURE MUST fail at the documented boundary,
    not earlier or later.

B4. Examples MUST NOT include undocumented fields or implicit semantics.

B5. Example metadata fields (timestamps, versions) MUST conform to
    declared formats and constraints.


Rule Set C — Adapter Boundary Validation Rules

C1. Adapter example artifacts under /examples/adapter MUST validate
    against execution_adapter_output.schema.json.

C2. Rejection examples MUST include a rejection_reason that matches
    execution_adapter_rejection.enum.json exactly.

C3. Rejection artifacts MUST NOT include execution_payload.

C4. EXECUTION_READY artifacts MUST include execution_payload and MUST
    NOT include rejection_reason.


Rule Set D — Cross-Artifact Consistency Rules

D1. All example artifacts MUST reference existing schemas.

D2. No example may reference a schema outside the declared repository scope.

D3. Responsibility boundaries declared in documentation MUST align with
    actual validation outcomes.

D4. Breaking changes across schemas, examples, or adapter artifacts
    MUST be accompanied by a version increment.


Failure Handling

Any CI rule violation MUST:

- Fail the CI job immediately
- Emit a machine-readable error message
- Prevent merge or release

Partial success states are prohibited.


Implementation Notes (Non-Normative)

CI implementations MAY use tools such as:
- ajv
- jsonschema
- spectral
- custom validators

Tool choice is non-binding, but rule compliance is mandatory.


Change Control

This document is a canonical enforcement artifact.

Any modification requires:
- Version increment
- Review approval
- Alignment with schema and example versions

Silent changes are prohibited.


Compliance Statement

Any repository, pipeline, or system claiming compliance with the
Prospera Generation Layer MUST implement CI validation consistent
with the rules defined in this document.


End of Document
