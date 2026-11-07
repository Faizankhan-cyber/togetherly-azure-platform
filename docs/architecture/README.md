# Architecture

This directory contains the platform architecture diagrams, deployment topology, trust boundaries, and version-by-version architecture changes.

## Current diagram

See [V1 Azure Architecture](v1-architecture.md) for the proposed V1 topology.

## Architecture rules

- Diagrams must distinguish proposed, deployed, and verified components.
- The V1 resource group is shared across the cumulative V1–V5 platform.
- V1 infrastructure is created manually through the Azure Portal.
- V2 is planned to introduce Azure Bicep. Terraform is not currently part of the project plan.
- The existing Togetherly TinyFish API routes are Vercel-style serverless handlers. V1 proposes adapting them to Azure Functions; they are not a conventional Node.js server.
- Record material architecture changes in `docs/decisions/`.
