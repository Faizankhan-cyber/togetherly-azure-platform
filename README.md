# Togetherly Azure Platform

> A cumulative five-version cloud engineering project that evolves Togetherly Workforce into a secure, observable, and resilient Azure environment.

[![Azure](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![V1](https://img.shields.io/badge/V1-Core%20deployment%20running-success)](docs/versions/v1.md)
[![Roadmap](https://img.shields.io/badge/Roadmap-V1%E2%80%93V5-blue)](docs/versions/)

## Project status

**V1 core deployment is running and the main application workflows have been tested.** Twelve evidence screenshots have been uploaded. Final closure is pending screenshot privacy review/redaction, an actual Azure cost review, and a resource-by-resource cleanup decision.

Live application: https://togetherly-v1-faizan.centralindia.cloudapp.azure.com

## What is deployed in V1

- Azure Resource Group: `rg-togetherly-platform` (shared by V1–V5).
- Central India VNet, application subnet, NSG, Linux VM, public IP, NIC and OS disk.
- Ubuntu Server 24.04 LTS VM: `vm-togetherly-v1`.
- Nginx serves the React/Vite production frontend over HTTPS.
- Nginx reverse-proxies `/api/tinyfish/*` to a Node.js Express API bound to `127.0.0.1:3001`.
- The API runs as `tinyfish-api.service` under systemd.
- Supabase and TinyFish are external services accessed by the server-side API.

**Architecture clarification:** Azure Functions was considered during planning but was not deployed. The working V1 API is a local Node.js Express service on the VM. See [V1 architecture](docs/architecture/v1-architecture.md).

## Five-version roadmap

| Version | Focus | Approach | Status |
|---|---|---|---|
| V1 | Initial Azure deployment, networking, HTTPS, frontend and TinyFish API | Manual Azure Portal provisioning | Core deployment running; closure reviews pending |
| V2 | Identity, secrets, RBAC, policy and data governance | Azure Bicep | Planned |
| V3 | Network hardening, monitoring, alerts, backup and patching | Extend the platform | Planned |
| V4 | Microsoft Sentinel, KQL, Python posture checks and investigation | Extend the platform | Planned |
| V5 | Availability, disaster recovery and measured RTO/RPO | Extend the platform | Planned |

This is one cumulative platform, not five unrelated labs. The shared resource group must not be deleted during version-specific cleanup. Terraform is not part of the current roadmap; Bicep is planned for V2.

## Repository map

- `docs/`: architecture, decisions, security, runbooks, cost records and version plans.
- `infra/`: Portal configuration records, future Bicep work and shared resource inventory.
- `application/`: Azure-specific deployment notes. The full Togetherly Workforce source remains in its [own repository](https://github.com/Faizankhan-cyber/togetherly-workforce).
- `evidence/`: sanitised, version-specific implementation evidence.

See [Project Overview](PROJECT_OVERVIEW.md) and [Repository Structure](docs/repository-structure.md).

## V1 verification summary

- [x] Frontend deployed and reachable over HTTPS.
- [x] HTTP redirects to HTTPS.
- [x] Nginx and TinyFish API services running.
- [x] API bound to loopback; port 3001 is not intended for public exposure.
- [x] Unauthenticated API requests rejected with HTTP 401.
- [x] Authenticated TinyFish search, fetch and agent workflows confirmed in the live app.
- [x] Twelve evidence screenshots uploaded to `evidence/v1/`.
- [ ] Review and redact screenshots for subscription IDs, IP addresses, email addresses and other personal/account details.
- [ ] Review actual Azure spend and remaining student credit.
- [ ] Verify the live resource inventory and decide whether to retain, deallocate or delete V1 resources individually.

## Security and cost rules

- Never commit credentials, API keys, tokens, private keys, `.env` files, subscription IDs, tenant IDs or personal IP addresses.
- Keep `TINYFISH_API_KEY` server-side only.
- Sanitise screenshots and logs before publishing them.
- Do not modify production Supabase data, policies or authentication settings without separate approval.
- Verify dependencies and costs before deleting or provisioning resources. Budget alerts are notifications, not spending caps.
- Do not delete the shared `rg-togetherly-platform` resource group as part of V1 cleanup.

## License

No open-source license has been selected. All rights are reserved unless a license is added.
