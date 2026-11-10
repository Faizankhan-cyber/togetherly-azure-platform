# Togetherly Azure Platform

## Purpose

Togetherly Azure Platform is a cumulative cloud engineering project built around the Togetherly Workforce application. This repository contains Azure architecture, infrastructure records, deployment documentation, version plans and implementation evidence. The application source remains in its [own repository](https://github.com/Faizankhan-cyber/togetherly-workforce).

- **Cloud:** Microsoft Azure
- **Previous temporary resource group:** `rg-togetherly-platform` (V1/V2 environment; deleted on 2026-11-10)
- **Region:** Central India
- **Current milestone:** V1 and V2 officially completed. All 14 V2 screenshots are uploaded, the V2 release is published, preservation documentation is committed, and the owner confirmed deletion of `rg-togetherly-platform` on 2026-11-10. Check for retained resources and delayed charges after billing data refreshes. Recreate V1 and V2 through Bicep before V3. See the [V1/V2 Infrastructure Preservation Pack](docs/preservation/v2-infrastructure-preservation-pack.md).

This project is separate from Togetherly CloudSec Platform / CloudSec Engine and the older Azure Enterprise Infrastructure project.

## V1 implementation

The V1 frontend and TinyFish API were hosted on one Ubuntu Linux VM in Central India.

```text
Browser
  |
 HTTPS
  v
Azure public endpoint
  |
 Network Security Group
  |
Ubuntu Linux VM
  |
  +-- Nginx --> React/Vite frontend
  |
  +-- Nginx /api/tinyfish/* --> Node.js Express API
                                  |
                                  +--> Supabase
                                  +--> TinyFish
```

Nginx served the frontend, handled HTTPS and reverse-proxied API requests to the Express service on `127.0.0.1:3001`. The API was managed by `tinyfish-api.service` under systemd.

**Azure Functions was considered but not deployed.** Do not describe it as part of the deployed platform.

## V2 implementation

V2 strengthens the existing VM-hosted API rather than replacing the V1 hosting design.

- A system-assigned Managed Identity is enabled on `vm-togetherly-v1`.
- The Key Vault `togetherly-key-2026` stores the TinyFish API credential.
- The VM identity has the Key Vault Secrets User role at vault scope. The authorised operator has Key Vault Secrets Officer at vault scope.
- The Node.js API uses `DefaultAzureCredential` and `SecretClient` to retrieve `TINYFISH-API-KEY`. The value is cached in process memory for a short interval and is not printed in logs.
- Key Vault public network access is restricted to selected network paths, including the application VNet/subnet and an administrator IP allow-list entry. The Key Vault service endpoint is enabled on the application subnet. No Private Endpoint is deployed.
- A resource-group-scoped tag policy requires `Project=Togetherly`. The assignment uses `DoNotEnforce`, so it evaluates compliance without blocking resource operations.
- The policy compliance scan reported 8 of 8 evaluated resources compliant.
- Search, Fetch, Agent execution and Agent status were re-tested after the Key Vault integration and network restriction.

V2 was implemented manually through the Azure Portal. GitHub OIDC and Bicep deployment were not implemented as part of this version.

## V2 architecture

See [V2 architecture and trust boundaries](docs/architecture/v2-architecture.md). The design keeps the TinyFish credential on the server and uses the VM identity for Key Vault access. The browser does not receive the TinyFish API credential.

## Version roadmap

| Version | Certification milestone | Focus | Main capabilities | Status |
|---|---|---|---|---|
| V1 | After AZ-900 | Initial hosting | VM, VNet, subnet, NSG, Nginx, frontend, TinyFish API and HTTPS | Implemented |
| V2 | After SC-500 | Identity and governance | Managed Identity, Key Vault, RBAC, Key Vault network ACLs, service endpoint and tag policy | Completed; resource group deleted; all 14 screenshots uploaded |
| V3 | After AZ-104 | Hardening and operations | Network controls, Azure Monitor, Log Analytics, alerts, backup, restore and patching | Planned |
| V4 | After SC-500 | Detection and investigation | Microsoft Sentinel, KQL, Python posture checks and incident investigation | Planned |
| V5 | After AZ-305 | Architecture and resilience | Availability, disaster recovery, measured RTO/RPO and Zero Trust review | Planned |

V1 and V2 were originally configured manually through the Azure Portal. From V3 onward, the project will use Azure Bicep to recreate the previous completed versions and deploy the new version. After each version is completed and its evidence/configuration is safely preserved, the Azure environment will be deleted to avoid ongoing charges until the next milestone. Terraform is not part of this project.

## Repository responsibilities

- `docs/`: architecture, version records, decisions, security notes and runbooks.
- `infra/`: Azure provisioning records and shared resource inventory.
- `application/`: Azure-specific deployment notes; not the application source.
- `evidence/`: versioned implementation screenshots.
- Application source: [togetherly-workforce](https://github.com/Faizankhan-cyber/togetherly-workforce).

## Security and operational boundaries

- Keep API keys and credentials out of Git and browser bundles.
- Do not commit subscription IDs, tenant IDs, personal IP addresses, private keys or raw authentication data.
- Do not modify production Supabase data, policies or authentication settings without separate approval.
- Test only project-owned or explicitly authorised systems.
- Delete Azure resources only after the completed version's source, Bicep templates, configuration records, documentation, release and sanitised evidence are safely preserved.
- The temporary `rg-togetherly-platform` environment was deleted after V2 closure on 2026-11-10; V3 will recreate the required infrastructure through Bicep.
- Check for resources outside the resource group and soft-deleted Key Vaults that may remain after deletion.
- Azure Policy tag compliance is a governance check, not proof that every resource is secure.
- Billing amounts and personal credit balances are private and are intentionally not recorded here.
