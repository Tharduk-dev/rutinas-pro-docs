------------------------------------------------------------

# Documentation Governance Model

## Overview

This document defines the official governance model for the Extended Technical Documentation (DTE) of the project.

The DTE is a structural component of the system and is governed under the same constitutional principles as the codebase.

---

## Primary and Secondary Source of Truth Model

### Primary Source of Truth

The authoritative and editable version of the DTE resides inside the private repository:

rutinas-pro

Directory:

/docs

All documentation updates must originate from this location.

No documentation changes are allowed directly in the public repository.

---

### Secondary Source of Truth (Public Mirror)

A public mirror of the DTE exists for transparency and public access.

Public repository:

rutinas-pro-docs

Public URL:

https://tharduk-dev.github.io/rutinas-pro-docs/

The public repository is strictly read-only.

It must never receive direct commits.

It must never be modified manually.

It must only be updated through the official subtree publication mechanism.

---

## Official Publication Mechanism

The only allowed mechanism for publishing the DTE is:

git subtree push --prefix=docs docs-public main

Rules:

- The prefix must always be exactly: docs
- No other directory may be published.
- The prefix must not be modified.
- The command must not be altered.
- No alternative publication mechanism is allowed.
- Submodules are prohibited.
- Manual copy operations are prohibited.
- Direct commits to the public repository are prohibited.

Any variation from this command constitutes a governance violation.

---

## Git Governance Integration

The DTE publication model is constitutionally integrated into the Git governance system.

Publication principles:

1. All documentation changes must occur in the private repository.
2. The private repository remains the primary constitutional source.
3. The public repository acts exclusively as a mirror.
4. The subtree publication must preserve structural isolation.
5. The publication process must not expose non-documentation directories.

---

## Automation Readiness

Future automation may trigger documentation publication through the instruction:

"actualiza documentación"

However, even under automation:

- The prefix must remain docs.
- The command must remain unchanged.
- The model must remain primary/secondary.
- No structural deviation is allowed.

---

## Change Registration

Change ID: DOC-INFRA-001  
Category: Structural Governance Milestone  
EPIC: Documentation Infrastructure Formalization  
Impact Level: Governance-Level Structural Reinforcement  

This change formalizes:

- The DTE primary/secondary model.
- Mandatory subtree publication.
- Mandatory --prefix=docs enforcement.
- GitHub Pages activation.
- Public mirror read-only status.

No product architecture was modified.
No stack changes were introduced.
No backend or frontend components were affected.

---

## Constitutional Alignment

This documentation update reflects an already implemented governance decision and ensures alignment between:

- Constitutional Level 1 governance
- Git execution protocol
- Documentation structural model

The DTE now formally mirrors the constitutional publication mechanism.

------------------------------------------------------------
