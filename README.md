# Togetherly Azure Platform

> A five-version cloud engineering project that evolves the Togetherly Workforce application into a secure, observable, threat-aware, and resilient Azure environment.

[![Cloud](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1 provisioning](https://img.shields.io/badge/V1-Azure%20Portal%20Manual-yellow)](docs/versions/v1.md)
[![V2 IaC](https://img.shields.io/badge/V2-Azure%20Bicep-0078D4)](https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview)

## Overview

Togetherly Azure Platform is one cumulative cloud engineering project built around the existing **Togetherly Workforce** application. Each version builds on the previous one:

1. **V1 — Initial Azure deployment:** deploy the frontend to an Azure Linux VM with Nginx and host the TinyFish API routes on Azure Functions.
2. **V2 — Identity, secrets, and data governance:** introduce stronger workload identity, Key Vault, RBAC, policy, and data-handling controls using Azure Bicep.
3. **V3 — Hardening and operations:** add network controls, monitoring, alerts, backup, restore, and patching.
4. **V4 — Detection and investigation:** add Microsoft Sentinel, KQL detections, Python posture checks, and authorised security testing.
5. **V5 — Enterprise architecture and resilience:** evaluate availability, recovery, disaster recovery, and measured RTO/RPO.

This is one repository and one evolving platform roadmap, not five disconnected labs.

**Current status:** Repository foundation and V1 planning. No Azure resources have been provisioned for this project.

## Version roadmap

| Version | Focus | Infrastructure / learning approach | Status |
|---|---|---|---|
| V1 | Initial Azure deployment | Manual Azure Portal provisioning; VM, VNet, NSG, Nginx, Azure Functions | Planning |
| V2 | Identity, secrets, governance | Azure Bicep; Managed Identity, Key Vault, RBAC, Policy | Planned |
| V3 | Hardening and operations | Build on the existing platform | Planned |
| V4 | Detection and investigation | Sentinel, KQL, Python security checks | Planned |
| V5 | Architecture and resilience | Availability and disaster recovery design | Planned |

## V1 architecture

The proposed V1 topology is:

- Azure Linux VM + Nginx serves the React/Vite frontend.
- Nginx reverse-proxies `/api/tinyfish/*` requests to an Azure Function App over HTTPS.
- Azure Functions hosts the adapted TinyFish API endpoints.
- Azure Functions calls Supabase and TinyFish over HTTPS.
- Supabase remains the external managed authentication and PostgreSQL service.

The existing Togetherly Workforce API routes are Vercel-style serverless handlers, not an Express server. They must be adapted and tested for the Azure Functions Node.js v4 programming model.

See [V1 Architecture Diagram](docs/architecture/v1-architecture.md).

## Shared resource group

All versions use one shared Azure Resource Group:

`rg-togetherly-platform`

Resources are added and retired individually as the platform evolves. Do not delete the shared resource group during a version-specific cleanup. Only remove explicitly identified resources after checking that they are not used by another version.

## Repository structure

The complete, maintained folder blueprint is documented in [Repository Structure](docs/repository-structure.md). The main responsibilities are:

- `docs/`: architecture, decisions, security, runbooks, cost guidance, and version plans.
- `infra/`: Azure Portal provisioning notes, Bicep templates, and shared resource records.
- `application/`: Azure-specific Functions and Nginx components only; the main Togetherly Workforce app stays in its own repository.
- `scripts/` and `tests/`: reusable automation and validation, added when implemented.
- `evidence/`: sanitised proof organised by version.

Future folders are planned placeholders, not completed code or deployed services. V1 uses the Azure Portal; Bicep is reserved for V2. Terraform is not part of this project roadmap.

## Engineering principles

- **One cumulative project:** all versions build on the same platform and repository.
- **Manual V1, Bicep V2:** V1 resources are created through the Azure Portal; V2 introduces Azure Bicep. Terraform is not part of the current roadmap.
- **Evidence before cleanup:** capture and sanitise evidence before removing disposable resources.
- **Least privilege:** restrict network access and protect all credentials.
- **Cost discipline:** validate prices before provisioning and review charges regularly. Budget alerts are not spending caps.
- **Honest status:** distinguish planned, deployed, tested, and verified capabilities.
- **Authorised testing:** security testing is limited to project-owned or explicitly authorised resources.

## V1 acceptance summary

- [ ] Frontend deployed to the Azure VM and served over HTTPS.
- [ ] Nginx proxies TinyFish API requests to Azure Functions.
- [ ] Azure Functions preserve authentication and authorization checks.
- [ ] Supabase login and required application flows work.
- [ ] TinyFish routes are tested and secrets remain server-side.
- [ ] Network access is restricted and resources are tagged.
- [ ] Cost, evidence, known gaps, and cleanup are documented.

See [V1 implementation checklist](docs/versions/v1.md) for detailed acceptance and evidence requirements.

## Cost planning

The initial planning envelope recorded for the project is US$100, subject to the Azure for Students offer and current regional pricing.

| Stage | Planning target |
|---|---:|
| V1 | US$15 |
| V2 | US$10 |
| V3 | US$35 |
| V4 | US$30 |
| Reserve | US$10 |
| **Total** | **US$100** |

These are planning estimates, not quotes or guaranteed limits. Check the current Azure Pricing Calculator and subscription offer before creating resources.

## Security and disclosure

Never commit credentials, API keys, tokens, private keys, `.env` files, subscription or tenant identifiers, personal IP addresses, or unredacted account details. Sanitise screenshots and logs before committing them. Do not modify production Togetherly data or authentication settings as part of this infrastructure project without a separately approved change.

## Getting started

1. Read [Project Overview](PROJECT_OVERVIEW.md).
2. Review the [V1 architecture](docs/architecture/v1-architecture.md).
3. Follow the [V1 plan](docs/versions/v1.md).
4. Do not provision resources until region, availability, network exposure, runtime compatibility, and cost checks are complete.

## License

No open-source license has been selected. Unless and until a license is added, all rights are reserved by the repository owner.
