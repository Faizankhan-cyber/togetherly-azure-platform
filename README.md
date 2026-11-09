# Togetherly Azure Platform

A cumulative Azure cloud engineering project built around the Togetherly Workforce application. The platform is developed in versions, with each version adding a new layer of cloud engineering skills.

[![Microsoft Azure](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1](https://img.shields.io/badge/V1-Deployed%20%7C%20Deallocated-blue)](docs/versions/v1.md)
[![Roadmap](https://img.shields.io/badge/Roadmap-V1%E2%80%93V5-informational)](docs/versions/)

## Current status

**V1 deployment and application testing are complete.** The VM was deallocated on 9 November 2026. The public application is not expected to be reachable while the VM remains deallocated. Final resource review and cleanup are still pending.

## What V1 includes

- Azure Virtual Network, subnet and Network Security Group.
- Ubuntu 24.04 Linux virtual machine in Central India.
- Nginx serving the React/Vite frontend over HTTPS.
- Node.js Express TinyFish API running as a systemd service on `127.0.0.1:3001`.
- Nginx reverse proxy for `/api/tinyfish/*`.
- Integration with external Supabase and TinyFish services.
- Twelve V1 implementation screenshots. Sanitised copies have been uploaded and replace the originals in the public evidence folder.

**Architecture note:** Azure Functions was considered during planning but was not deployed. The V1 API runs on the VM as a Node.js Express service. See [V1 architecture](docs/architecture/v1-architecture.md).

## Roadmap

| Version | Focus | Status |
|---|---|---|
| V1 | Azure hosting, networking, HTTPS and application integration | Deployed; cleanup pending |
| V2 | Identity, secrets, RBAC, policy and data governance | Planned |
| V3 | Network hardening, monitoring, backup and operations | Planned |
| V4 | Microsoft Sentinel, KQL and security investigation | Planned |
| V5 | Availability, disaster recovery and resilience | Planned |

The versions build on one shared platform. The resource group `rg-togetherly-platform` must be preserved during version-specific cleanup. V1 was provisioned manually through the Azure Portal; Azure Bicep is planned for V2. Terraform is not part of the current roadmap.

## Repository guide

- [Project overview](PROJECT_OVERVIEW.md): purpose, architecture and roadmap.
- [Architecture](docs/architecture/): deployed V1 design and diagrams.
- [Version records](docs/versions/): implementation status and plans.
- [Infrastructure](infra/): V1 provisioning records and shared resource inventory.
- [Evidence](evidence/v1/): twelve V1 implementation screenshots with sanitised public copies.
- [Deployment runbook](docs/runbooks/deployment.md): how V1 was deployed and verified.

The Togetherly Workforce application source is maintained separately: [togetherly-workforce](https://github.com/Faizankhan-cyber/togetherly-workforce).

## Safety

- Never commit credentials, API keys, tokens, private keys, `.env` files or personal identifiers.
- Keep TinyFish credentials on the server side.
- Review screenshots and logs before publishing.
- Verify resource dependencies before deleting anything.
- Do not delete the shared `rg-togetherly-platform` resource group during V1 cleanup.

## License

No open-source license has been selected. All rights reserved unless a license is added.
