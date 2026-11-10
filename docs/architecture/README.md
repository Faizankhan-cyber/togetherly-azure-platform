# Architecture

This directory contains the platform architecture, network topology, trust boundaries and version-by-version design decisions.

## Current architecture

- [V1 integrated architecture](v1-architecture.md) describes the deployed application request flow and Azure infrastructure.
- [V2 architecture](v2-architecture.md) documents the implemented Managed Identity, Key Vault, RBAC, network restrictions, secret-retrieval flow and Azure Policy controls.
- [Editable V1 Mermaid source](diagrams/v1-architecture.mmd) remains the source for the V1 diagram.
- [Architecture overview](architecture-overview.md) records the cumulative V1–V5 direction.
- [Network topology](network-topology.md) records the shared VNet and subnet design.

## Architecture decisions and boundaries

- Distinguish deployed and verified components from planned future capabilities.
- The resource group `rg-togetherly-platform` is shared across the cumulative V1–V5 platform.
- V1 and V2 were provisioned manually through the Azure Portal.
- Azure Bicep was considered for V2 but was **not implemented**. Bicep is the planned IaC method beginning with V3; Terraform is not part of the current project.
- The deployed TinyFish API is a Node.js Express service on the VM, not Azure Functions.
- V2 Key Vault uses a public endpoint with network ACL restrictions and a VNet service endpoint; it does not use a Private Endpoint.
- V2 Azure Policy assignment uses DoNotEnforce and evaluates compliance without blocking resource operations.
- Record material architecture decisions in `docs/decisions/`.
