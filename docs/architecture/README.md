# Architecture

This directory contains platform architecture diagrams, deployment topology, trust boundaries, and version-by-version architecture changes.

## Current V1 design

- [V1 integrated architecture](v1-architecture.md) combines the application request flow with the Azure infrastructure and network layout.
- [Editable V1 Mermaid source](diagrams/v1-architecture.mmd) is the single source of truth for the integrated diagram.
- [Architecture overview](architecture-overview.md) records the cumulative V1–V5 direction.
- [Network topology](network-topology.md) is the home for approved network details.

## Architecture rules

- Diagrams must distinguish proposed, deployed, and verified components.
- The V1 resource group is shared across the cumulative V1–V5 platform.
- V1 infrastructure is created manually through the Azure Portal.
- V2 is planned to introduce Azure Bicep. Terraform is not part of the project plan.
- Existing Togetherly TinyFish API routes are Vercel-style serverless handlers. V1 proposes adapting them to Azure Functions; they are not a conventional Node.js server.
- Keep the integrated V1 diagram as the single source of truth; do not create separate duplicate application and infrastructure diagrams unless the architecture becomes too complex to read.
- Record material architecture decisions in `docs/decisions/`.
