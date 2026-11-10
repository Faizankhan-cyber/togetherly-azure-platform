# ADR-002: Infrastructure as Code approach

**Status:** Superseded for V2; Bicep remains a possible future milestone  
**Date:** 2026-11-08  
**Updated:** 2026-11-10

## Context

V1 was created manually through the Azure Portal so the resource configuration and dependencies could be understood first. The original V2 proposal was to introduce Azure Bicep for infrastructure deployment.

## Decision and actual outcome

V2 identity, Key Vault, network and governance controls were configured manually through the Azure Portal. **Bicep templates and automated Bicep deployment were not implemented in V2.**

The original proposal is therefore not a description of the delivered V2 implementation. The owner has selected Azure Bicep as the rebuild/deployment method beginning with V3 after AZ-104. The current V1/V2 environment will be deleted after its configuration is preserved, then V1 and V2 will be recreated from Bicep before V3 is added. No deployable Bicep templates exist yet; the current folder is reserved. See the [V1/V2 Infrastructure Preservation Pack](../preservation/v2-infrastructure-preservation-pack.md).

## Consequences

- Do not claim that V2 infrastructure is deployed through Bicep.
- Do not run a template deployment from the reserved Bicep folder as part of V2.
- Any future templates must be validated and tested before deployment.
- Parameters must not contain secrets.
- The temporary V1/V2 resource group is a teardown boundary, not a permanent shared resource group.
- Future V3–V5 infrastructure methods remain undecided until their scope is designed.
