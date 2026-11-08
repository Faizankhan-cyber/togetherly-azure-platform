# Architecture

This directory contains the platform architecture, network topology, trust boundaries and version-by-version design decisions.

## Current V1 design

- [V1 integrated architecture](v1-architecture.md) describes the deployed application request flow and Azure infrastructure.
- [Editable V1 Mermaid source](diagrams/v1-architecture.mmd) is the single source of truth for the integrated diagram.
- [Architecture overview](architecture-overview.md) records the cumulative V1–V5 direction.
- [Network topology](network-topology.md) records approved network details.

## Architecture rules

- Distinguish proposed, deployed and verified components.
- The resource group is shared across the cumulative V1–V5 platform.
- V1 was provisioned manually through the Azure Portal.
- V2 is planned to introduce Azure Bicep. Terraform is not part of the project roadmap.
- The deployed V1 TinyFish API is a Node.js Express service on the VM, not Azure Functions.
- Keep the integrated V1 diagram as the single source of truth.
- Record material architecture decisions in `docs/decisions/`.
