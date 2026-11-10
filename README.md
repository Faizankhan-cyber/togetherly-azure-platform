# Togetherly Azure Platform

A cumulative Azure cloud engineering project built around the Togetherly Workforce application. Each version adds a practical layer of cloud engineering, security, operations and resilience.

[![Microsoft Azure](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1](https://img.shields.io/badge/V1-Implemented%20%7C%20Evidence%20Uploaded-blue)](docs/versions/v1.md)
[![V2](https://img.shields.io/badge/V2-Implemented%20%7C%20Evidence%20Uploaded-blue)](docs/versions/v2.md)
[![Roadmap](https://img.shields.io/badge/Roadmap-V1%E2%80%93V5-informational)](docs/versions/)

## Current status

**V1 and V2 are officially complete.** V2 implementation, functional verification, all 14 evidence uploads, GitHub release `v2.0.0`, preservation documentation and Azure resource-group teardown are complete. The owner confirmed that `rg-togetherly-platform` was deleted from Azure Portal on 2026-11-10. A post-teardown check for retained/orphaned resources and delayed billing remains an operational follow-up; resource-group deletion alone does not prove that every separately retained resource or charge is gone.

## Releases

- **V1.0.0:** [Release notes](docs/releases/v1.0.0.md) · [GitHub release](https://github.com/Faizankhan-cyber/togetherly-azure-platform/releases/tag/v1.0.0). The tag points to the V1 completion commit, before V2 work.
- **V2.0.0:** [Release notes](docs/releases/v2.0.0.md) · [GitHub release](https://github.com/Faizankhan-cyber/togetherly-azure-platform/releases/tag/v2.0.0). V2 implementation, verification, evidence upload and Azure teardown are complete.

## V1 — Initial Azure deployment

- Azure Virtual Network, application subnet and Network Security Group.
- Ubuntu 24.04 Linux VM in Central India.
- Nginx serving the React/Vite frontend over HTTPS.
- Node.js Express TinyFish API running as a systemd service on `127.0.0.1:3001`.
- Nginx reverse proxy for `/api/tinyfish/*`.
- Integration with external Supabase and TinyFish services.
- **V1 status:** Implemented; all 12 sanitised evidence screenshots uploaded under [`evidence/v1/`](evidence/v1/) the temporary Azure resource group was deleted on 10 November 2026.

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
- Fourteen V2 evidence files present in [`evidence/v2/`](evidence/v2/);.

V2 was configured manually through the Azure Portal and verified from the VM and application. **GitHub OIDC, a Key Vault private endpoint, Azure Functions, and Bicep deployment were not implemented in V2.** See [V2 implementation record](docs/versions/v2.md) and [V2 architecture](docs/architecture/v2-architecture.md).

## Roadmap

| Version | Certification milestone | Focus | Status |
|---|---|---|---|
| V1 | After AZ-900 | Azure hosting, networking, HTTPS and application integration | Implemented; evidence uploaded; resource group deleted |
| V2 | After SC-900 | Managed Identity, Key Vault, RBAC, network restrictions and Azure Policy | Completed; Azure environment deleted; all 14 screenshots uploaded |
| V3 | After AZ-104 | Network hardening, monitoring, backup and operations, plus Bicep, GitHub OIDC and a Key Vault private endpoint deferred from V2 | Planned |
| V4 | After SC-500 | Microsoft Sentinel, KQL and security investigation | Planned |
| V5 | After AZ-305 | Availability, disaster recovery and resilience | Planned |

The project will use a **rebuild-and-destroy lifecycle** for each version milestone. See the [V1/V2 Infrastructure Preservation Pack](docs/preservation/v2-infrastructure-preservation-pack.md) for the captured configuration, known gaps, deletion checklist and V3 rebuild sequence. After a version is completed, tested, documented, released and its evidence is safely preserved in GitHub, the Azure resource group and its billable resources will be deleted to prevent idle infrastructure costs. Before the next milestone, the completed earlier versions will be recreated from source-controlled **Azure Bicep** templates, then the new version will be added. V1 and V2 were originally configured manually through the Azure Portal; beginning with V3, Bicep becomes the source of truth for rebuilding V1, V2 and the new V3 infrastructure. Terraform is not part of this project.

## Repository guide

- [Project overview](PROJECT_OVERVIEW.md): purpose, architecture and roadmap.
- [Architecture](docs/architecture/): V1 and V2 architecture and network design.
- [Version records](docs/versions/): implementation status and plans.
- [Security review](docs/security/v2-review.md): V2 controls, limitations and residual risks.
- [Cost and operations review](docs/runbooks/v2-cost-and-operations-review.md): cost drivers and safe shutdown guidance.
- [Infrastructure](infra/): provisioning records and temporary environment resource inventory.
- [Infrastructure preservation pack](docs/preservation/v2-infrastructure-preservation-pack.md): V1/V2 configuration, pre-deletion checklist and V3 Bicep rebuild sequence.
- [Evidence](evidence/): versioned implementation evidence.
- [Deployment runbook](docs/runbooks/deployment.md): V1 deployment record.

The Togetherly Workforce application source is maintained separately: [togetherly-workforce](https://github.com/Faizankhan-cyber/togetherly-workforce).

## Safety

- Never commit credentials, API keys, tokens, private keys, `.env` files or personal identifiers.
- Keep TinyFish credentials server-side and retrieve them through Managed Identity.
- Review screenshots and logs before publishing.
- Delete Azure resources only after the version's code, configuration, documentation, release and sanitised evidence are preserved and verified.
- The temporary V1/V2 resource group `rg-togetherly-platform` was deleted on 2026-11-10 after evidence and configuration preservation. Check Azure for retained disks, public IPs, snapshots, soft-deleted Key Vault state and delayed charges. Recreate the environment from Bicep for V3.
- Check for resources outside the resource group and soft-deleted Key Vaults that may remain after resource-group deletion.
- Never commit secrets or live personal/billing identifiers to the Bicep templates or parameter files.
- Do not treat a compliant tag-policy result as a complete security audit.
- Keep live Azure billing screenshots and remaining credit balances private.

## License

No open-source license has been selected. All rights reserved unless a license is added.
