# ADR-002: Infrastructure as Code approach

**Status:** Superseded for V2; Bicep remains a possible future milestone  
**Date:** 2026-11-08  
**Updated:** 2026-11-10

## Context

V1 was created manually through the Azure Portal so the resource configuration and dependencies could be understood first. The original V2 proposal was to introduce Azure Bicep for infrastructure deployment.

## Decision and actual outcome

V2 identity, Key Vault, network and governance controls were configured manually through the Azure Portal. **Bicep templates and automated Bicep deployment were not implemented in V2.**

The original proposal is therefore not a description of the delivered V2 implementation. Bicep may be introduced in a later milestone after resource ownership, shared dependencies, parameters, deployment scope and cost have been reviewed.

## Consequences

- Do not claim that V2 infrastructure is deployed through Bicep.
- Do not run a template deployment from the reserved Bicep folder as part of V2.
- Any future templates must be validated and tested before deployment.
- Parameters must not contain secrets.
- Shared resources and version-specific resources must be clearly identified.
- Future V3–V5 infrastructure methods remain undecided until their scope is designed.
