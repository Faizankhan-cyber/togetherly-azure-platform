# Togetherly Azure Platform

> A five-version cloud engineering project that evolves the Togetherly Workforce application into a secure, observable, threat-aware, and resilient Azure environment.

[![Cloud](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1](https://img.shields.io/badge/V1-Deployed%20Core%20Workload-success)](docs/versions/v1.md)
[![V2 IaC](https://img.shields.io/badge/V2-Azure%20Bicep-0078D4)](https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview)

## Overview

Togetherly Azure Platform is one cumulative cloud engineering project built around the existing Togetherly Workforce application. Each version builds on the previous one:

1. **V1 — Initial Azure deployment:** deploy the frontend and TinyFish API to an Azure Linux VM using Nginx and a local Node.js service.
2. **V2 — Identity, secrets, and data governance:** introduce workload identity, Key Vault, RBAC, policy, and data-handling controls using Azure Bicep.
3. **V3 — Hardening and operations:** add network controls, monitoring, alerts, backup, restore, and patching.
4. **V4 — Detection and investigation:** add Microsoft Sentinel, KQL detections, Python posture checks, and authorised security testing.
5. **V5 — Enterprise architecture and resilience:** evaluate availability, recovery, disaster recovery, and measured RTO/RPO.

This is one repository and one evolving platform roadmap, not five disconnected labs.

**Current status:** V1 core application deployment is running and smoke-tested. Evidence capture, actual cost review, and cleanup review remain before V1 is fully closed.

## Version roadmap

| Version | Focus | Infrastructure / learning approach | Status |
|---|---|---|---|
| V1 | Initial Azure deployment | Manual Azure Portal provisioning; VM, VNet, NSG, Nginx, Node.js API | Core deployment complete; closure tasks remain |
| V2 | Identity, secrets, governance | Azure Bicep; Managed Identity, Key Vault, RBAC, Policy | Planned |
| V3 | Hardening and operations | Build on the existing platform | Planned |
| V4 | Detection and investigation | Sentinel, KQL, Python security checks | Planned |
| V5 | Architecture and resilience | Availability and disaster recovery design | Planned |

## V1 deployed architecture

- Azure Linux VM in Central India.
- Nginx serves the React/Vite frontend over HTTPS.
- Nginx reverse-proxies `/api/tinyfish/*` to a Node.js Express API bound to `127.0.0.1:3001`.
- The API runs as `tinyfish-api.service` under systemd.
- The API connects to external Supabase and TinyFish services using server-side environment configuration.
- Public application: https://togetherly-v1-faizan.centralindia.cloudapp.azure.com

**Architecture correction:** Azure Functions was part of an earlier proposal but was not deployed. V1 uses a local Node.js API service on the VM. The repository documentation records this implementation honestly.

See [V1 Architecture](docs/architecture/v1-architecture.md) and [V1 implementation record](docs/versions/v1.md).

## Shared resource group

All versions use one shared Azure Resource Group: `rg-togetherly-platform`.

Resources are added and retired individually as the platform evolves. Do not delete the shared resource group during a version-specific cleanup. Only remove explicitly identified resources after checking that they are not used by another version.

## Repository structure

The maintained folder blueprint is documented in [Repository Structure](docs/repository-structure.md). The main responsibilities are:

- `docs/`: architecture, decisions, security, runbooks, cost guidance, and version plans.
- `infra/`: Azure Portal provisioning notes, Bicep templates, and shared resource records.
- `application/`: Azure-specific deployment notes; the main Togetherly Workforce app stays in its own repository.
- `scripts/` and `tests/`: reusable automation and validation, added when implemented.
- `evidence/`: sanitised proof organised by version.

Future folders are planned placeholders, not completed code or deployed services. V1 uses the Azure Portal; Bicep is reserved for V2. Terraform is not part of this project roadmap.

## Engineering principles

- **One cumulative project:** all versions build on the same platform and repository.
- **Manual V1, Bicep V2:** V1 resources are created through the Azure Portal; V2 introduces Azure Bicep.
- **Evidence before cleanup:** capture and sanitise evidence before removing disposable resources.
- **Least privilege:** restrict network access and protect all credentials.
- **Cost discipline:** validate prices before provisioning and review charges regularly. Budget alerts are not spending caps.
- **Honest status:** distinguish planned, deployed, tested, and verified capabilities.
- **Authorised testing:** security testing is limited to project-owned or explicitly authorised resources.

## V1 acceptance summary

- [x] Frontend deployed to the Azure VM and served over HTTPS.
- [x] Nginx proxies TinyFish API requests to the local Node.js service.
- [x] API authentication rejects unauthenticated requests.
- [x] Supabase login and authenticated application use verified in the live app.
- [x] TinyFish search, fetch, and agent workflows confirmed by the user.
- [x] Network access configured and resources tagged.
- [ ] Capture and commit sanitised evidence.
- [ ] Review actual Azure cost and remaining credit.
- [ ] Complete cleanup review and record final resource state.

See [V1 implementation checklist](docs/versions/v1.md) for detailed acceptance and evidence requirements.

## Cost planning

The initial planning envelope recorded for the project is US$100, subject to the Azure for Students offer and current regional pricing. This is a planning estimate, not a quote, hard limit, or spending cap. Review actual billing before cleanup or additional provisioning.

## Security and disclosure

Never commit credentials, API keys, tokens, private keys, `.env` files, subscription or tenant identifiers, personal IP addresses, or unredacted account details. Sanitise screenshots and logs before committing them. Do not modify production Togetherly data or authentication settings as part of this infrastructure project without a separately approved change.

## Getting started

1. Read [Project Overview](PROJECT_OVERVIEW.md).
2. Review the [V1 architecture](docs/architecture/v1-architecture.md).
3. Follow the [V1 implementation record](docs/versions/v1.md).
4. Review the V2–V5 roadmap before extending the platform.

## License

No open-source license has been selected. Unless and until a license is added, all rights are reserved by the repository owner.
