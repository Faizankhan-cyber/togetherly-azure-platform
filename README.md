# Togetherly Azure Platform

A cumulative Azure cloud engineering project built around the Togetherly Workforce application. Each version adds a practical layer of cloud engineering, security, operations and resilience.

[![Microsoft Azure](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1](https://img.shields.io/badge/V1-Deployed-blue)](docs/versions/v1.md)
[![V2](https://img.shields.io/badge/V2-Implemented%20%7C%20Evidence%20Uploaded-blue)](docs/versions/v2.md) [![Release](https://img.shields.io/badge/Release-v2.0.0-blue)](https://github.com/Faizankhan-cyber/togetherly-azure-platform/releases/tag/v2.0.0)
[![Roadmap](https://img.shields.io/badge/Roadmap-V1%E2%80%93V5-informational)](docs/versions/)

## Current status

**V1 is deployed and its application workflows were verified. V2 identity, secret-management, network and governance controls have been implemented and functionally tested.** The `v2.0.0` GitHub release is published. All 14 V2 screenshot files are now uploaded under `evidence/v2/`; a full-size visual privacy review remains outstanding.

The VM was running during V2 service verification; its current live state has not been independently rechecked for this documentation update. Confirm that it is **Stopped (deallocated)** when not testing. Managed disks, a retained public IP and Key Vault operations may still incur charges while the VM is deallocated.

## V1 — Initial Azure deployment

- Azure Virtual Network, application subnet and Network Security Group.
- Ubuntu 24.04 Linux VM in Central India.
- Nginx serving the React/Vite frontend over HTTPS.
- Node.js Express TinyFish API running as a systemd service on `127.0.0.1:3001`.
- Nginx reverse proxy for `/api/tinyfish/*`.
- Integration with external Supabase and TinyFish services.
- Twelve sanitised V1 implementation screenshots.

**Architecture note:** Azure Functions was considered during planning but was not deployed. The V1 API runs on the VM as a Node.js Express service. See [V1 architecture](docs/architecture/v1-architecture.md).

## V2 — Identity, secrets and governance

V2 adds security controls to the existing VM-based application:

- System-assigned Managed Identity on the Azure VM.
- Azure Key Vault with RBAC permission model, soft delete and purge protection.
- TinyFish API credential stored in Key Vault and retrieved by the Node.js API through Azure Identity and Key Vault SDKs.
- Key Vault network access restricted to the selected VNet/subnet and an explicitly allowed administrator IP address.
- Key Vault service endpoint enabled on the application subnet.
- Key Vault Secrets User assigned to the VM identity; Key Vault Secrets Officer assigned to the authorised operator.
- Resource-group-scoped `Project=Togetherly` tag policy assignment configured with **DoNotEnforce**.
- Policy compliance scan reported 8 of 8 resources compliant.
- TinyFish Search, Fetch and Agent workflows re-tested after the security integration.

V2 was configured manually through the Azure Portal and verified from the VM and application. **GitHub OIDC, a Key Vault private endpoint, Azure Functions, and Bicep deployment were not implemented in V2.** See [V2 implementation record](docs/versions/v2.md) and [V2 architecture](docs/architecture/v2-architecture.md).

## Roadmap

| Version | Focus | Status |
|---|---|---|
| V1 | Azure hosting, networking, HTTPS and application integration | Implemented; VM-based workload |
| V2 | Managed Identity, Key Vault, RBAC, network restrictions and Azure Policy | Implemented; 14 evidence files uploaded |
| V3 | Network hardening, monitoring, backup and operations | Planned |
| V4 | Microsoft Sentinel, KQL and security investigation | Planned |
| V5 | Availability, disaster recovery and resilience | Planned |

The versions build on one shared resource group, `rg-togetherly-platform`. Preserve shared resources during version-specific cleanup. V1 and V2 were configured manually through the Azure Portal. Azure Bicep is reserved for a later infrastructure-as-code milestone. Terraform is not part of this project.

## Repository guide

- [Project overview](PROJECT_OVERVIEW.md): purpose, architecture and roadmap.
- [Architecture](docs/architecture/): V1 and V2 architecture and network design.
- [Version records](docs/versions/): implementation status and plans.
- [Security review](docs/security/): baseline and V2 review notes.
- [Cost and operations review](docs/runbooks/v2-cost-and-operations-review.md): cost drivers and safe shutdown guidance.
- [Infrastructure](infra/): provisioning records and shared resource inventory.
- [Evidence](evidence/): versioned implementation evidence.
- [Deployment runbook](docs/runbooks/deployment.md): V1 deployment record.

The Togetherly Workforce application source is maintained separately: [togetherly-workforce](https://github.com/Faizankhan-cyber/togetherly-workforce).

## Safety

- Never commit credentials, API keys, tokens, private keys, `.env` files or personal identifiers.
- Keep TinyFish credentials server-side and retrieve them through Managed Identity.
- Review screenshots and logs before publishing.
- Verify resource dependencies before deleting anything.
- Do not delete the shared `rg-togetherly-platform` resource group during version-specific cleanup.
- Do not treat a compliant tag-policy result as a complete security audit.

## License

No open-source license has been selected. All rights reserved unless a license is added.
