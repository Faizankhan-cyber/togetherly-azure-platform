# ADR-002: Introduce Azure Bicep in V2

**Status:** Planned  
**Date:** 2026-11-08

## Context

V1 is intended to be created manually through the Azure Portal so the resource configuration and dependencies are understood first.

## Decision

Introduce Azure Bicep as the Infrastructure as Code learning and deployment method for V2. Keep V1 manual; do not retroactively require Bicep for V1.

## Consequences

V2 templates must be reviewed, validated, and tested before deployment. Parameters must not contain secrets. State which resources are shared and which are version-specific. Future V3–V5 infrastructure methods remain undecided until their scope is designed.
