Document Type: Canonical Interface Contract  
Layer: Generation ↔ Execution Adapter  
Status: Canonical (Active)  
Version: v0.1.0  
Parent Constraints:  
- Prospera Execution Layer v1.0.0  
- Prospera Generation Layer v0.1.0  
Owner: Prospera Architecture Group  
Last Updated: 2026-01-07  

---

## 1. Purpose

This document defines the canonical adapter contract between the Prospera
Generation Layer and the Prospera Execution Layer.

The adapter is a strict boundary component responsible for validating,
normalizing, and enforcing execution readiness of generation outputs.

The adapter SHALL NOT perform generation, decision-making, optimization,
or execution.

---

## 2. Input Contract

### 2.1 Accepted Input

The adapter accepts exactly one input type:

- generation_result.schema.json (canonical)

Inputs not conforming to the canonical generation result schema MUST be
rejected without modification.

### 2.2 Status Handling

- status = SUCCESS  
  Indicates a candidate payload intended for execution.

- status = FAILURE  
  Indicates a terminal generation failure and MUST NOT be forwarded to
  execution.

---

## 3. Output Contract

The adapter produces exactly one of the following outcomes:

### 3.1 EXECUTION_READY

Emitted only when all validation and constraint checks pass.

Required fields:
- execution_status = "EXECUTION_READY"
- execution_payload (normalized)
- metadata (propagated and extended)

### 3.2 EXECUTION_REJECTED

Emitted when validation, schema compliance, or constraint checks fail.

Required fields:
- execution_status = "EXECUTION_REJECTED"
- rejection_reason (canonical)
- metadata

The adapter MUST NOT emit partial or ambiguous states.

---

## 4. Validation Rules

The adapter MUST enforce the following rules:

1. Schema compliance is mandatory.
2. Required fields MUST be present and non-null.
3. Payload MUST be deterministic and self-contained.
4. No implicit instructions or inferred intent are allowed.
5. No autonomous execution triggers are permitted.

Any violation MUST result in EXECUTION_REJECTED.

---

## 5. Failure Mapping

| Generation Failure Code | Adapter Decision        | Execution Outcome      |
|-------------------------|-------------------------|------------------------|
| UNSCHEMA_COMPLIANT      | Reject                  | EXECUTION_REJECTED     |
| INCOMPLETE_PAYLOAD      | Reject                  | EXECUTION_REJECTED     |
| NON_DETERMINISTIC       | Reject                  | EXECUTION_REJECTED     |
| POLICY_VIOLATION        | Reject                  | EXECUTION_REJECTED     |

No failure code may be remapped or ignored.

---

## 6. Non-Responsibilities

The adapter explicitly SHALL NOT:

- Regenerate content
- Modify semantic meaning
- Inject missing data
- Perform execution
- Perform authorization or policy decisions

---

## 7. Versioning and Change Control

- Adapter contract versions are strictly bound to Generation Layer versions.
- Breaking changes require a new minor or major version.
- Execution Layer implementations MUST declare supported adapter versions.

---

— End of Canonical Interface Contract —
