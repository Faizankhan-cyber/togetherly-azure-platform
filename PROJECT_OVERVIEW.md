# Togetherly Azure Platform — Project Overview

## Purpose

Togetherly Azure Platform is a five-version, cumulative cloud engineering project built around the existing Togetherly Workforce application. This repository manages Azure infrastructure records, architecture, operational documentation, version plans and evidence. The application source remains in its separate repository.

- **Platform repository:** https://github.com/Faizankhan-cyber/togetherly-azure-platform
- **Application repository:** https://github.com/Faizankhan-cyber/togetherly-workforce
- **Cloud:** Microsoft Azure
- **Shared resource group:** `rg-togetherly-platform`
- **Current milestone:** V1 core deployment running; closure reviews pending.

This project is separate from Togetherly CloudSec Platform / CloudSec Engine and the older Azure Enterprise Infrastructure project.

## Current V1 implementation

The deployed V1 topology is:

```text
Browser
  |
  | HTTPS
  v
Azure public DNS / public IP
  |
Network Security Group
  |
Ubuntu Linux VM (Central India)
  |
  +--> Nginx --> React/Vite production frontend
  |
  +--> Nginx /api/tinyfish/* --> Node.js Express API
                                  (127.0.0.1:3001)
                                      |
                                      +--> Supabase
                                      |
                                      +--> TinyFish API
```

The API runs as `tinyfish-api.service` under systemd. Nginx serves the frontend, terminates HTTPS and forwards API requests to the loopback service.

**Azure Functions was an earlier design option and was not deployed in V1.** The current working API is the Node.js Express service on the VM. Do not describe Azure Functions as part of the deployed environment.

## V1 status

Verified from the deployment workflow and user-confirmed application tests:

- Resource group, VNet, subnet, NSG and Ubuntu VM provisioned in Central India.
- Nginx serves the production frontend.
- HTTPS works and HTTP redirects to HTTPS.
- TinyFish API runs on loopback port 3001 under systemd.
- Unauthenticated requests to the four API routes return HTTP 401.
- Authenticated search, fetch and agent workflows were confirmed in the live application.
- Twelve screenshots are uploaded under `evidence/v1/`.

Remaining closure tasks:

1. Review and redact evidence screenshots before public release. Some Azure Portal captures show account or network identifiers.
2. Review actual Azure charges and remaining student credit.
3. Verify the live Azure resource inventory and dependencies.
4. Record whether each V1 resource will be retained, deallocated or deleted. Never delete the shared resource group.

## Five-version roadmap

| Version | Goal | Main capabilities | Planned approach |
|---|---|---|---|
| V1 | Initial Azure deployment | VM, VNet, NSG, Nginx, frontend, TinyFish API, HTTPS and smoke tests | Manual Azure Portal |
| V2 | Identity, secrets and data governance | Managed Identity, Key Vault, RBAC, GitHub OIDC, Azure Policy, data classification and threat model | Azure Bicep |
| V3 | Hardening and operations | Network controls, Azure Monitor, Log Analytics, alerts, backup, restore and patching | Extend the platform |
| V4 | Detection and investigation | Microsoft Sentinel, KQL, Python posture checks and authorised testing | Extend the platform |
| V5 | Enterprise architecture and resilience | Availability, disaster recovery, measured RTO/RPO and Zero Trust review | Extend the platform |

V2–V5 are plans, not deployed capabilities. V3–V5 implementation methods will be selected when those versions are designed. Terraform is not part of the current roadmap.

## Repository responsibilities

- `docs/`: architecture, decisions, version plans, security, runbooks and cost management.
- `infra/`: provisioning records, future Bicep templates and shared resource inventory.
- `application/`: Azure-specific integration and deployment notes only.
- `scripts/`: reviewed reusable automation when implemented.
- `tests/`: repeatable validation when implemented.
- `evidence/`: sanitised implementation proof grouped by version.

README placeholders describe planned areas; they do not represent working code or deployed services.

## Safety and operating rules

- Keep API keys and credentials out of Git and browser bundles.
- Keep the TinyFish API bound to loopback; do not expose port 3001 publicly.
- Do not commit subscription IDs, tenant IDs, personal IP addresses, private keys or raw authentication data.
- Do not modify production Supabase data, policies or authentication settings without separate approval.
- Test only project-owned or explicitly authorised systems.
- Check dependencies and actual cost before deleting or creating resources.
- All versions share `rg-togetherly-platform`; never delete it during a version-specific cleanup.
