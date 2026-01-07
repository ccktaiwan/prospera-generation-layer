Prospera Generation Layer — Examples & Validation Matrix
Document Type: Canonical Validation Examples Index
Layer: Prospera Generation Layer
Status: Canonical (Active)
Version: v0.1.0
Parent Constraint: Prospera Execution Layer v1.0.0
Owner: Prospera Architecture Group
Last Updated: 2026-01-07
Scope: Schema-compliant reference artifacts

Purpose

This directory contains canonical, schema-compliant example artifacts for the Prospera Generation Layer.
These examples are not illustrative drafts — they are executable validation references intended for
CI pipelines, local validators, and downstream integration testing.

All files in this directory MUST validate successfully against the corresponding schemas
defined in /schemas.

Example Set Overview

1) generation_task.valid.json
Role:
Canonical valid input example for the Generation Layer.

Validated Against:
- schemas/generation_task.schema.json

Usage:
- Reference for request construction
- CI validation baseline for task ingestion
- Developer contract reference when integrating generators

Contract Guarantees:
- Fully schema-compliant
- Deterministic, non-autonomous intent
- Execution-ready constraint alignment

2) generation_result.success.json
Role:
Canonical SUCCESS result example for the Generation Layer.

Validated Against:
- schemas/generation_result.schema.json

Usage:
- Reference for successful generation output formatting
- Validation target for generators producing execution-ready payloads
- CI enforcement of success-path correctness

Contract Guarantees:
- Includes payload and metadata
- No failure_code present
- Timestamp and version traceability preserved

3) generation_result.failure.json
Role:
Canonical FAILURE result example for the Generation Layer.

Validated Against:
- schemas/generation_result.schema.json
- schemas/generation_failure.enum.json

Usage:
- Reference for standardized failure signaling
- Validation target for generator error handling
- CI enforcement of failure-path determinism

Contract Guarantees:
- Includes failure_code and metadata
- No payload present
- Failure classification strictly controlled by enum

Validation Rules

1. All example files MUST validate against their referenced schemas.
2. Examples MUST NOT include additionalProperties outside schema allowances.
3. Examples MUST reflect canonical semantics — not edge experimentation.
4. CI pipelines SHOULD reject any change that breaks example validation.

Operational Recommendation

Typical validation workflow:
1. Validate generation_task.valid.json against generation_task.schema.json
2. Validate generation_result.success.json against generation_result.schema.json
3. Validate generation_result.failure.json against generation_result.schema.json

These examples collectively define the minimum viable correctness envelope
for any Generation Layer implementation.

Change Control

- Modifying example files without schema changes is allowed only to clarify structure,
  not to change semantic meaning.
- Any example change that requires schema modification MUST follow schema versioning rules.
- Examples serve the schema — schemas do not adapt to examples.

End of Document
