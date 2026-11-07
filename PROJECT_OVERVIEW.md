# Togetherly Azure Platform — Project Overview

**Repository:** https://github.com/Faizankhan-cyber/togetherly-azure-platform  
**Application workload:** Togetherly Workforce  
**Cloud:** Microsoft Azure  
**Default branch:** main  
**Current stage:** V1 repository foundation and architecture planning  
**Azure resources:** None provisioned for this project as of this document update  
**Shared resource group:** `rg-togetherly-platform`

## 1. Executive summary

Togetherly Azure Platform is a five-version, cumulative cloud engineering project built around the existing Togetherly Workforce application. One repository and one evolving Azure platform will be used to demonstrate deployment, identity and data security, operations, detection, and resilience.

This repository manages the Azure platform and its documentation. It is separate from the Togetherly Workforce application repository and the separate Togetherly CloudSec Platform / CloudSec Engine project.

A capability is complete only when it is implemented, tested, documented, and supported by sanitised evidence. Diagrams and placeholder folders are not proof of implementation.

## 2. Five-version roadmap

| Version | Goal | Main focus | Learning alignment | Infrastructure approach |
|---|---|---|---|---|
| V1 | Initial Azure deployment | Linux VM, Nginx, frontend, Azure Functions API, networking, HTTPS, testing | AZ-900 | Manual Azure Portal |
| V2 | Identity, secrets, and data governance | Managed Identity, Key Vault, RBAC, OIDC, Policy, data classification, threat model | SC-900 | Azure Bicep |
| V3 | Hardening and operations | Network controls, Azure Monitor, Log Analytics, alerts, backup, restore, patching | AZ-104 | Extend platform |
| V4 | Detection and investigation | Sentinel, KQL, Python posture checks, authorised testing, incident report | Verify current certification mapping before relying on it | Extend platform |
| V5 | Enterprise architecture and resilience | Availability, scaling, disaster recovery, measured RTO/RPO, Zero Trust review | AZ-305 | Extend platform |

All versions remain in this repository and build on the previous completed version. Use one shared resource group, `rg-togetherly-platform`, for the cumulative environment. Version-specific cleanup must never delete the shared group or resources still used by another version.

## 3. V1 architecture

V1 proposes the following topology:

- Azure Linux VM with Nginx serves the existing React/Vite frontend.
- Nginx proxies `/api/tinyfish/*` requests to an Azure Function App over HTTPS.
- Azure Functions hosts the TinyFish API routes.
- Azure Functions communicates with Supabase and TinyFish over HTTPS.
- Supabase remains the external managed authentication and PostgreSQL service.

**Important compatibility finding:** the current Togetherly Workforce TinyFish endpoints are Vercel-style serverless handlers. The application repository does not contain a conventional Express server or Node.js API service. The handlers must be adapted and tested for the Azure Functions Node.js v4 programming model. Do not copy the existing handlers unchanged and assume they will run.

The detailed proposed topology, request flows, trust boundaries, and open decisions are in [docs/architecture/v1-architecture.md](docs/architecture/v1-architecture.md).

## 4. V1 infrastructure approach

V1 resources will be created manually through the Azure Portal. Terraform and Bicep will not be used to provision V1.

V2 is the planned Azure Bicep learning milestone. The repository reserves `infra/bicep/v2/` for that future work. Terraform is not part of the current project plan.

The shared resource group is:

`rg-togetherly-platform`

Do not create Azure resources until the subscription, available credit, region, service availability, VM size, Functions hosting plan, network exposure, DNS/TLS design, and costs have been checked.

### Planned V1 resource types

- Resource Group (shared by V1–V5)
- Virtual Network and subnet
- Network Security Group
- Public IP address
- Ubuntu Linux Virtual Machine
- Nginx
- Azure Function App using Node.js v4
- Storage account required by the Functions hosting configuration
- HTTPS and DNS configuration

Exact SKUs, region, pricing, hosting plan, storage redundancy, and certificate approach remain open until validated.

## 5. Application and service boundaries

- Togetherly Workforce remains the application source repository.
- This repository contains Azure platform planning, configuration notes, architecture, evidence, and version records.
- Supabase remains external and continues to provide application authentication and PostgreSQL.
- TinyFish remains an external API.
- `TINYFISH_API_KEY` must remain server-side. It must never be included in Vite/browser variables or committed to Git.
- Preserve existing Supabase session validation, employee/role checks, URL validation, methods, status codes, and response behaviour when adapting the API routes.
- Do not change production Supabase data, policies, or authentication settings as part of this infrastructure task without separate explicit approval.

## 6. Repository structure

```text
togetherly-azure-platform/
├── README.md
├── PROJECT_OVERVIEW.md
├── PROMPT.md
├── CHANGELOG.md
├── .gitignore
├── .github/
│   └── workflows/
├── infra/
│   ├── portal/
│   │   └── v1/
│   └── bicep/
│       └── v2/
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── data-classification/
│   ├── runbooks/
│   ├── threat-model/
│   └── versions/
├── evidence/
│   ├── v1/
│   ├── v2/
│   ├── v3/
│   ├── v4/
│   └── v5/
└── tests/
```

A folder or README placeholder does not mean implementation exists. `infra/portal/v1/` stores manual provisioning notes. `infra/bicep/v2/` is reserved for future Bicep templates.

## 7. Engineering principles

1. **One cumulative project:** all five versions build on the same platform and repository.
2. **Manual V1, Bicep V2:** use the Azure Portal for V1; introduce Bicep in V2 after learning it.
3. **Evidence before cleanup:** capture and sanitise evidence before deleting disposable resources.
4. **Least privilege:** restrict network access and protect credentials.
5. **Cost discipline:** validate pricing before provisioning. Azure budget alerts are notifications, not hard spending caps.
6. **Honest status:** distinguish planned, deployed, tested, and verified capabilities.
7. **Authorised testing:** test only project-owned or explicitly authorised systems.
8. **No unapproved production changes:** infrastructure work must not silently modify production data or auth settings.

## 8. V1 acceptance summary

- [ ] Resources are created in the shared resource group and tagged consistently.
- [ ] Network access is restricted; SSH is limited to the administrator's current public IP.
- [ ] Nginx serves the pinned frontend build over HTTPS.
- [ ] Nginx proxies TinyFish API paths to Azure Functions over HTTPS.
- [ ] Azure Functions preserve authentication and authorization checks.
- [ ] Supabase login and required application flows work.
- [ ] TinyFish routes are tested and secrets remain server-side.
- [ ] Function execution limits and request timeouts are validated.
- [ ] Cost, evidence, known gaps, and cleanup are documented.
- [ ] Version-specific cleanup is tested without deleting shared resources.

The detailed checklist is in [docs/versions/v1.md](docs/versions/v1.md).

## 9. Cost planning

The initial planning envelope is US$100 in Azure credit, subject to the actual Azure for Students offer and current regional pricing.

| Stage | Planning target |
|---|---:|
| V1 | US$15 |
| V2 | US$10 |
| V3 | US$35 |
| V4 | US$30 |
| Reserve | US$10 |
| **Total** | **US$100** |

These are planning estimates, not quotes, guaranteed limits, or hard spending caps. Check the Azure Pricing Calculator and subscription entitlements before provisioning. Budget alerts do not stop spending automatically.

## 10. Security and evidence rules

- Never commit passwords, API keys, tokens, private keys, `.env` files, subscription IDs, tenant IDs, or personal IP addresses.
- Use placeholder example configuration only.
- Keep TinyFish credentials server-side.
- Prefer least privilege and short-lived credentials where supported.
- Restrict SSH and expose only documented ports.
- Sanitise screenshots, logs, and terminal output before committing.
- Do not run destructive actions without checking the exact subscription and target resources.
- Record limitations and failed tests honestly.

## 11. Current verified status

| Item | Status |
|---|---|
| GitHub repository and Git history | Existing |
| Five-version roadmap | Documented |
| Shared resource group naming | Decided: `rg-togetherly-platform` |
| V1 manual Azure Portal approach | Decided |
| V2 Azure Bicep approach | Planned |
| V1 Azure Functions architecture | Proposed; implementation not started |
| V1 architecture diagram | Documented as proposed |
| Azure resource creation | Not started |
| Application/API adaptation | Not started |
| V1 deployment and acceptance tests | Not started |
| V2–V5 implementation | Not started |

## 12. Immediate next steps

1. Finish and review the repository structure and documentation.
2. Confirm the V1 architecture and Azure Functions hosting choice.
3. Validate subscription, region, service availability, and costs.
4. Create V1 resources manually through the Azure Portal only after the checks and approvals.
5. Adapt and test the TinyFish handlers for Azure Functions.
6. Deploy, test, document, capture sanitised evidence, and clean up safely.

Update this document whenever an approved scope, architecture, safety, or verified implementation status changes. Keep it consistent with README.md, PROMPT.md, and docs/versions/v1.md.
