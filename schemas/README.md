Prospera Generation Layer — Canonical Schemas Index
Document Type: Canonical Schema Index
Layer: Prospera Generation Layer
Status: Canonical (Active)
Version: v0.1.0
Parent Constraint: Prospera Execution Layer v1.0.0
Owner: Prospera Architecture Group
Last Updated: 2026-01-07
Scope: Generation task and result contracts (schema-level)

Purpose

This document is the canonical index for all JSON schemas shipped in the Prospera Generation Layer.
It defines the stable contract surface that downstream layers (validators, CI, tooling, Execution Layer adapters)
MUST consume without reinterpretation.

Contract Rules

1. Schemas in this directory are canonical.
2. Downstream implementations MUST validate against these schemas.
3. Do not add non-canonical schema copies elsewhere in the repository.
4. Schema changes are versioned and MUST be release-tracked.
5. Examples in /examples MUST remain schema-compliant.

Schema Set

1) Generation Task Schema
File: generation_task.schema.json
Role: Canonical input contract for generation requests.
Primary Use:
- Validates generation request payloads before any model/tool invocation
- Enforces required fields and prohibited ambiguity for deterministic execution readiness

2) Generation Result Schema
File: generation_result.schema.json
Role: Canonical output contract for generation outcomes (SUCCESS / FAILURE).
Primary Use:
- Normalizes generation outputs for downstream execution readiness
- Enforces mutually exclusive success payload vs failure_code signaling
- Provides metadata for auditability and traceability

3) Generation Failure Enum
File: generation_failure.enum.json
Role: Canonical list of failure codes for Generation Layer hard/soft failures.
Primary Use:
- Guarantees consistent failure classification across tools, CI, and runtime
- Enables deterministic recovery routing and analytics

Compatibility Notes

- All schemas target JSON Schema draft 2020-12.
- generation_result.schema.json references generation_failure.enum.json for failure_code.
- SUCCESS results MUST include payload and metadata.
- FAILURE results MUST include failure_code and metadata.

Operational Use (CI / Tooling)

Recommended validation flow:
1. Validate incoming generation tasks with generation_task.schema.json
2. Validate produced outcomes with generation_result.schema.json
3. Reject any artifact that violates additionalProperties=false constraints

Change Control

- Any schema modification MUST increment schema versions and be recorded in the release notes.
- Breaking changes require a minor or major version bump (SemVer discipline at the layer level).
- Do not modify canonical meaning via examples; examples must follow schemas, not redefine them.

End of Document
