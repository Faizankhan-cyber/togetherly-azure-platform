# Togetherly Azure Platform

## Purpose

Togetherly Azure Platform is a cumulative cloud engineering project built around the Togetherly Workforce application. This repository contains Azure architecture, infrastructure records, deployment documentation, version plans and implementation evidence. The application source remains in its [own repository](https://github.com/Faizankhan-cyber/togetherly-workforce).

- **Cloud:** Microsoft Azure
- **Shared resource group:** `rg-togetherly-platform`
- **Region:** Central India
- **Current milestone:** V1 deployed; V2 identity, Key Vault, network and governance controls implemented and tested; all 14 screenshot files are present; full-size privacy review pending.

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

| Version | Focus | Main capabilities | Status |
|---|---|---|---|
| V1 | Initial hosting | VM, VNet, subnet, NSG, Nginx, frontend, TinyFish API and HTTPS | Implemented |
| V2 | Identity and governance | Managed Identity, Key Vault, RBAC, Key Vault network ACLs, service endpoint and tag policy | Implemented; screenshot publication pending |
| V3 | Hardening and operations | Network controls, Azure Monitor, Log Analytics, alerts, backup, restore and patching | Planned |
| V4 | Detection and investigation | Microsoft Sentinel, KQL, Python posture checks and incident investigation | Planned |
| V5 | Architecture and resilience | Availability, disaster recovery, measured RTO/RPO and Zero Trust review | Planned |

V1 and V2 were configured manually through the Azure Portal. Azure Bicep is reserved for a later infrastructure-as-code milestone. Terraform is not part of this project.

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
- Verify resource dependencies before deleting resources.
- All versions share `rg-togetherly-platform`; never delete it during version-specific cleanup.
- Azure Policy tag compliance is a governance check, not proof that every resource is secure.
- Billing amounts and personal credit balances are private and are intentionally not recorded here.
