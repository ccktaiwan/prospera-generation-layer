# Generation Validator Contract

Document Type: Canonical Engineering Contract
Layer: Prospera Generation Layer
Artifact ID: GEN-VAL-01
Status: Canonical (Active)
Version: v0.1.0
Parent Constraint: Prospera Execution Layer v1.0.0
Dependent Artifacts:
- generation_task.schema.json
- generation_failure.enum.json
- generation_result.schema.json
- generator.interface.md
Last Updated: 2026-01-07

---

## 1. Purpose

This document defines the mandatory validation contract for all
generation outputs produced within the Prospera Generation Layer.

The validator exists to enforce correctness, safety, and execution
readiness prior to any handoff to the Execution Layer.

Validation is a hard gate, not a best-effort check.

---

## 2. Validation Responsibility

Validation MUST occur:

- After generation completes
- Before execution is attempted
- Before any output is persisted or transmitted

No execution workflow may bypass validation.

---

## 3. Validation Scope

The validator MUST enforce the following checks, in order:

1. Input task schema validation
2. Generator interface compliance
3. Result schema validation
4. Failure enum enforcement
5. Execution safety eligibility

Failure at any step results in immediate rejection.

---

## 4. Mandatory Validation Rules

### 4.1 Generation Task Validation

- Input MUST conform to generation_task.schema.json
- Invalid tasks MUST be rejected
- Rejection reason MUST be traceable

---

### 4.2 Interface Compliance

- Generator MUST expose the canonical interface
- Output MUST match GenerationResult shape
- Silent success is forbidden

---

### 4.3 Result Schema Validation

- Output MUST validate against generation_result.schema.json
- SUCCESS and FAILURE states MUST be mutually exclusive
- Payload MUST be present only on SUCCESS

---

### 4.4 Failure Signaling Enforcement

When generation fails:

- failure_code MUST be present
- failure_code MUST be one of generation_failure.enum.json
- Free-form error strings MUST NOT replace failure_code

---

### 4.5 Execution Eligibility Check

A SUCCESS result MUST be considered executable only if:

- Schema validation passes
- No non-autonomous behavior is detected
- No deterministic violation is flagged
- Payload is structurally complete

If any condition is unmet, validation MUST fail.

---

## 5. Failure Handling Semantics

On validation failure:

- Execution MUST NOT proceed
- Failure MUST be explicitly logged
- Failure MUST be attributable to a specific rule

Automatic correction or retry is forbidden.

---

## 6. CI Enforcement Requirements

CI pipelines MUST include validation steps that:

- Validate all schemas on change
- Reject non-conforming generator outputs
- Prevent merge of artifacts that weaken validation guarantees

Any CI bypass constitutes a system violation.

---

## 7. Non-Autonomous Enforcement

Validators MUST actively detect and reject:

- Implicit intent inference
- Scope expansion
- Output mutation attempts
- Policy assumption

Such cases MUST emit NON_AUTONOMOUS_BREACH.

---

## 8. Compatibility and Versioning

Validator behavior is version-bound.

Changes to validation rules require:

- Explicit version increment
- Migration notes
- Downstream impact review

---

## 9. Summary

The Generation Validator is the final gate protecting the Execution Layer
from unsafe, ambiguous, or invalid generation outputs.

It is mandatory, deterministic, and non-negotiable.

---

End of Document
