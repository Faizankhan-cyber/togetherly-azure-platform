# Togetherly Azure Platform

## Purpose

Togetherly Azure Platform is a cumulative cloud engineering project built around the existing Togetherly Workforce application. This repository contains Azure architecture, infrastructure records, deployment documentation, version plans and implementation evidence. The application source remains in its [own repository](https://github.com/Faizankhan-cyber/togetherly-workforce).

- **Cloud:** Microsoft Azure
- **Shared resource group:** `rg-togetherly-platform`
- **Current milestone:** V1 deployed and tested; sanitised evidence published; resource cleanup pending.

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

**Azure Functions was considered but not deployed.** Do not describe it as part of V1.

## V1 status

- Azure network and Ubuntu VM provisioned.
- React/Vite production frontend deployed and served by Nginx.
- HTTPS and HTTP-to-HTTPS redirect verified during deployment.
- TinyFish Express API deployed on loopback and managed by systemd.
- Unauthenticated API requests returned HTTP 401.
- Authenticated search, fetch and agent workflows were confirmed in the live application.
- Twelve V1 implementation screenshots are stored in `evidence/v1/`; sanitised public copies have been uploaded and replace the originals.
- VM was user-confirmed deallocated on 9 November 2026.
- The public application is not expected to be reachable while the VM is deallocated.
- Final Azure resource review and cleanup remain.

## Roadmap

| Version | Focus | Main planned capabilities |
|---|---|---|
| V1 | Initial Azure deployment | VM, VNet, NSG, Nginx, frontend, TinyFish API and HTTPS |
| V2 | Identity and governance | Managed Identity, Key Vault, RBAC, GitHub OIDC, Azure Policy, data classification and threat modelling |
| V3 | Hardening and operations | Network controls, Azure Monitor, Log Analytics, alerts, backup, restore and patching |
| V4 | Detection and investigation | Microsoft Sentinel, KQL, Python posture checks and authorised testing |
| V5 | Architecture and resilience | Availability, disaster recovery, measured RTO/RPO and Zero Trust review |

V2–V5 are plans, not deployed capabilities. V1 used manual Azure Portal provisioning. Azure Bicep is planned for V2; Terraform is not part of the current roadmap.

## Repository responsibilities

- `docs/`: architecture, version records, decisions, security notes and runbooks.
- `infra/`: Azure provisioning records and shared resource inventory.
- `application/`: Azure-specific deployment notes; not the application source.
- `evidence/`: V1 implementation screenshots with sanitised public copies uploaded.

## Safety rules

- Keep API keys and credentials out of Git and browser bundles.
- Do not commit subscription IDs, tenant IDs, personal IP addresses, private keys or raw authentication data.
- Do not modify production Supabase data, policies or authentication settings without separate approval.
- Test only project-owned or explicitly authorised systems.
- Verify resource dependencies before deleting resources.
- All versions share `rg-togetherly-platform`; never delete it during version-specific cleanup.

Billing amounts, credit balances and personal cost records are intentionally kept private and are not part of this repository.
