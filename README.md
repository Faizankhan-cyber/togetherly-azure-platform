# Togetherly Azure Platform

> A five-version cloud engineering project that evolves a real application deployment into a secure, observable, threat-aware, and resilient Azure environment.

[![Cloud](https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![Automation](https://img.shields.io/badge/Automation-Azure%20CLI-0078D4)](https://learn.microsoft.com/cli/azure/)
[![Status](https://img.shields.io/badge/Status-V1%20Planning-yellow)](docs/versions/v1.md)

## Overview

Togetherly Azure Platform is a hands-on engineering project built around the **Togetherly Workforce** application. The goal is to operate one evolving Azure environment through five maturity stages:

1. **Deploy** the application through repeatable infrastructure scripts and CI/CD.
2. **Secure** workload identity, secrets, permissions, and data handling.
3. **Harden and operate** the environment with network controls, monitoring, backup, and recovery.
4. **Detect and investigate** suspicious activity using Microsoft Sentinel, KQL, and controlled security tests.
5. **Design for resilience** through architecture evaluation, high availability, and disaster recovery.

This is one cumulative repository and one continuous engineering history, not five disconnected labs.

> **Project status:** V1 is being prepared. No production-readiness claim is made. Togetherly Workforce is treated as a learning workload, and all security testing is restricted to resources that we own or are explicitly authorised to test.

## Project goals

- Deploy a pinned version of Togetherly Workforce to an Azure-hosted environment.
- Automate infrastructure creation, deployment, verification, and teardown.
- Apply least privilege, secure secret handling, network segmentation, and governance.
- Collect evidence that controls work, rather than relying on configuration screenshots alone.
- Rebuild each version from repository code and verify previous-version behaviour.
- Track cloud costs and remove disposable resources after evidence is committed.
- Document engineering decisions, limitations, test results, and known gaps.

## Version roadmap

| Version | Focus | Main capabilities | Learning alignment | Status |
|---|---|---|---|---|
| **V1** | Repeatable deployment | Azure VM, VNet, NSG, Nginx, Node.js service, HTTPS, storage, CI/CD, cost governance | AZ-900 | Planning |
| **V2** | Identity, secrets, and data governance | Managed Identity, Key Vault, GitHub OIDC, RBAC, data classification, Azure Policy, Defender baseline, STRIDE | SC-900 | Planned |
| **V3** | Hardening and operations | Network segmentation, Azure Monitor, Log Analytics, alerts, backup, restore, patching | AZ-104 | Planned |
| **V4** | Detection and controlled testing | Microsoft Sentinel, KQL detections, Python posture checks, authorised attack simulation, incident report | SC-500 | Planned |
| **V5** | Architecture and resilience | Hosting decision records, high availability, load balancing, disaster recovery, measured RTO/RPO, Zero Trust review | AZ-305 | Planned |

Each version has a dedicated record in the docs/versions directory. The roadmap is a plan, not a claim that these capabilities are already implemented.

## Architecture direction

The intended V1 request path is:

    Internet
       |
    Azure Public IP
       |
    Network Security Group
       |
    Ubuntu VM running Nginx
       |                         |
    Static frontend        /api/tinyfish
                                  |
                           Local Node.js service
                                  |
                           HTTPS outbound
                                  |
                               Supabase

The Azure VM will host the built frontend and a small Node.js service for the required Togetherly API routes. Nginx will serve static assets and reverse-proxy the API path. Supabase remains an external managed service and continues to handle application authentication and PostgreSQL data.

The existing Togetherly Workforce repository is the application source. This repository will deploy a **pinned commit or release** of that application. Application changes are out of scope unless a small, documented compatibility adjustment is essential to run it on the target host.

The exact API process model, environment variables, ports, and runtime requirements must be confirmed by inspecting the existing implementation before deployment. The diagram is the target direction, not proof that the integration has already been validated.

See the architecture documentation in docs/architecture/.

## Repository structure

    togetherly-azure-platform/
    ├── README.md
    ├── CHANGELOG.md
    ├── .gitignore
    ├── .github/
    │   └── workflows/
    ├── infra/
    │   ├── config/
    │   ├── scripts/
    │   │   ├── deploy/
    │   │   ├── teardown/
    │   │   ├── verify/
    │   │   └── evidence/
    │   └── terraform/             # Introduced after Terraform learning milestone
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

Folders are kept in Git with small README or placeholder files until implementation begins. Generated evidence must be sanitised before it is committed.

## Engineering principles

### 1. One repository, cumulative versions

All five versions live here. Each version starts from the previous completed version and adds a clearly defined capability. Git tags identify completed milestones: v1.0 through v5.0.

### 2. Code is cumulative; cloud resources are disposable

Scripts, tests, documentation, and evidence accumulate. Azure resources created for a version are removed after its acceptance checks and evidence capture are complete. The next version rebuilds the previous baseline before adding new capabilities.

### 3. Evidence before teardown

A version is not complete until its acceptance checks pass and its evidence is committed. Never delete resources before capturing required screenshots, command output, test results, and cost evidence.

### 4. Regression before progression

At the beginning of each new version, rebuild the previous version from repository code and run its smoke and regression tests. New work must not silently break existing behaviour.

### 5. Safe, repeatable automation

Until the Terraform milestone, Azure infrastructure is managed with Azure CLI scripts (Bash first, PowerShell where useful). Scripts should be safe to re-run, validate prerequisites, fail clearly, and avoid destructive defaults.

### 6. Least privilege and secret hygiene

Never commit passwords, API keys, tokens, private keys, subscription IDs, tenant IDs, personal IP addresses, or unredacted account details. Use example configuration files with placeholder values. V2 is expected to remove the temporary V1 secret-on-disk pattern by introducing workload identity and Key Vault.

### 7. Cost discipline

Use a dedicated resource group and consistent resource tags. Configure budget notifications, review cost data regularly, time-box expensive services, and verify cleanup after teardown. Azure budgets are alerts, not hard spending caps; cost reporting can also be delayed.

### 8. Authorised testing only

Security tests must target only the project's own Azure resources or systems for which explicit permission has been granted. Follow Microsoft's current cloud penetration-testing rules and service terms.

## Tooling

| Area | Planned technology |
|---|---|
| Cloud platform | Microsoft Azure |
| Host OS | Ubuntu 24.04 LTS |
| Web server / reverse proxy | Nginx |
| Application frontend | React, TypeScript, Vite (existing Togetherly app) |
| API runtime | Node.js (compatibility to be verified) |
| Application data and auth | Supabase (external to Azure) |
| Infrastructure automation | Azure CLI, Bash; PowerShell where appropriate |
| CI/CD | GitHub Actions |
| Workload identity and secrets | Microsoft Entra ID, Managed Identity, Azure Key Vault |
| Governance | Azure RBAC, Azure Policy, Defender for Cloud baseline |
| Monitoring | Azure Monitor, Log Analytics |
| Detection | Microsoft Sentinel, KQL |
| Security automation | Python |
| Infrastructure as Code | Terraform, after the planned learning milestone |
| IaC scanning | Checkov, alongside Terraform adoption |

Tool choices may be adjusted when subscription availability, pricing, or compatibility checks show that the planned option is unsuitable. Any material change should be recorded in docs/decisions/.

## Version workflow

Every version follows the same controlled lifecycle:

1. Create a working branch from the previous version's tag.
2. Rebuild the previous version and run its smoke/regression tests.
3. Implement only the new version's scoped capabilities.
4. Run validation and acceptance tests while the Azure resources are live.
5. Capture and sanitise evidence in evidence/vN/.
6. Update the version record, architecture docs, and changelog.
7. Open a pull request, review the changes, and merge to main.
8. Create the version tag only after the acceptance criteria are met.
9. Tear down the disposable environment and verify cleanup.
10. Record any remaining gaps honestly before starting the next version.

## V1 acceptance criteria

V1 is complete only when all of the following are demonstrated:

- [ ] A pinned Togetherly Workforce build is deployed to the Azure VM.
- [ ] Nginx serves the frontend over HTTPS.
- [ ] Required TinyFish API routes work through the Node.js service and Nginx proxy.
- [ ] Supabase authentication and required application flows work as expected.
- [ ] Network access is limited to the documented ports and SSH is restricted to the administrator's current IP.
- [ ] Azure resources have the required project, version, owner, and environment tags.
- [ ] Storage is private and uses TLS-only access, with its purpose and access model documented.
- [ ] GitHub Actions can build and deploy the pinned application revision.
- [ ] Smoke tests pass against the deployed application.
- [ ] The environment can be deleted and recreated by running the repository scripts.
- [ ] Smoke tests pass again after the rebuild.
- [ ] Evidence, cost notes, known gaps, and the V1 version record are committed.
- [ ] Teardown is verified and no unintended billable project resources remain.

The detailed V1 checklist and evidence requirements live in docs/versions/v1.md.

## Cost and resource safety

The initial planning envelope is **US$100 in Azure credit**, subject to the actual subscription terms and regional pricing. The current planning allocation is:

| Stage | Planning target |
|---|---:|
| V1 | US$15 |
| V2 | US$10 |
| V3 | US$35 |
| V4 | US$30 |
| Reserve | US$10 |
| **Total** | **US$100** |

These are estimates, not Azure quotes or guaranteed limits. Validate each deployment with the Azure Pricing Calculator and the subscription's current offers before creating resources.

Cost controls:

- Configure budget notifications at the planned thresholds before deployment.
- Prefer small, eligible VM sizes and stop or delete compute when not in use.
- Time-box Sentinel and any paid Defender capabilities.
- Avoid leaving backups, disks, public IPs, or other billable resources behind.
- Review cost data after deployment and after teardown.
- Treat budget alerts as notifications, not automatic spending caps.

## Security and disclosure

This repository is intended as a public learning portfolio. Before committing evidence:

- Redact subscription and tenant identifiers, public IPs, email addresses, resource IDs where sensitive, and all credentials.
- Never upload .env files, private keys, service-principal secrets, access tokens, or raw authentication logs.
- Use least-privilege permissions and short-lived credentials wherever supported.
- Keep test activity limited to authorised project resources.
- Report security issues privately to the repository owner rather than publishing exploitable details.

See docs/runbooks/ for operational procedures as they are added.

## Getting started

At this initial stage, the repository contains project documentation and structure only. No Azure resources are created by cloning the repository.

Prerequisites for V1 implementation will include:

- An Azure subscription with available credit and permission to create the required resources.
- Azure CLI installed and authenticated.
- Git and Bash available locally.
- Access to the Togetherly Workforce repository and its deployment configuration.
- A GitHub account with permission to configure repository secrets and Actions settings.

The first implementation task is to inspect the Togetherly API and authentication code, verify Azure region and VM availability, then create and test the infrastructure scripts. Do not copy real credentials into the repository.

## Project status

| Milestone | Status |
|---|---|
| Repository created | Complete |
| Five-version roadmap documented | Complete |
| Initial folder structure | Complete |
| V1 architecture and acceptance criteria | Documented |
| Azure cost and subscription checks | Pending |
| Togetherly runtime compatibility review | Pending |
| V1 infrastructure implementation | Not started |
| V1 deployment and acceptance | Not started |

## License

No open-source license has been selected yet. Unless and until a license is added, all rights are reserved by the repository owner.