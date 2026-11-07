# Togetherly Azure Platform — Project Overview

> A cumulative, five-version cloud engineering project that takes the Togetherly Workforce application from a repeatable Azure deployment to a secure, observable, threat-aware, and resilient cloud environment.

**Repository:** https://github.com/Faizankhan-cyber/togetherly-azure-platform  
**Application workload:** Togetherly Workforce  
**Cloud provider:** Microsoft Azure  
**Default branch:** main  
**Current project stage:** V1 planning and repository foundation  
**Document status:** Project reference; update as implementation changes

---

## 1. Executive summary

Togetherly Azure Platform is a hands-on cloud engineering project built around the existing Togetherly Workforce SaaS application. It is intended to demonstrate how a real application can be deployed, secured, operated, monitored, tested, and improved on Microsoft Azure.

The project is deliberately organised as **one repository and one cumulative environment roadmap**. Each version builds on the previous version. It is not a collection of unrelated tutorials or five disconnected demo projects.

The five maturity stages are:

1. **V1 — Repeatable, governed deployment:** deploy a pinned Togetherly Workforce revision to Azure using repeatable scripts, a controlled network, HTTPS, CI/CD, smoke tests, cost controls, and teardown.
2. **V2 — Identity, secrets, and data governance:** strengthen workload identity, permissions, secret storage, data handling, policy, and the security baseline.
3. **V3 — Hardening and cloud operations:** add stronger network controls, monitoring, logging, alerting, backup, restore, and patching.
4. **V4 — Detection and investigation:** use Microsoft Sentinel, KQL, Python security checks, and authorised controlled security tests to detect and investigate suspicious activity.
5. **V5 — Enterprise architecture and resilience:** evaluate architecture options and implement appropriate availability, recovery, disaster recovery, and measured RTO/RPO capabilities.

The purpose is to produce working infrastructure and verifiable evidence, not merely diagrams or documentation. A capability is not complete just because a resource was created. It must be tested, documented, and reproducible.

## 2. Project objectives

The project aims to:

- Deploy a pinned version or release of Togetherly Workforce to an Azure-hosted environment.
- Understand and document the application's actual runtime, build, authentication, API, and external-service requirements before deployment.
- Automate infrastructure creation, deployment, verification, and teardown.
- Apply least privilege, secure secret handling, network controls, and governance.
- Integrate GitHub Actions for repeatable build and deployment workflows.
- Collect evidence that controls work, rather than relying only on configuration screenshots.
- Rebuild each version from repository code and verify previous-version behaviour.
- Track Azure spending and remove disposable resources after evidence has been captured.
- Maintain architecture diagrams, decision records, runbooks, test results, known gaps, and version completion records.
- Build practical skills in Azure administration, cloud architecture, cloud security, DevSecOps, automation, monitoring, and incident response.

## 3. Scope and boundaries

### In scope

- Azure infrastructure used to host and operate the Togetherly Workforce learning workload.
- Azure networking, compute, storage, identity, access control, secrets, governance, monitoring, and recovery.
- GitHub Actions and infrastructure/deployment automation.
- Security validation of resources owned by the project or explicitly authorised for testing.
- Documentation, evidence, cost tracking, and repeatability.

### Existing application and external services

Togetherly Workforce is the application source. This repository is responsible for the cloud environment and its deployment process. Application changes are out of scope unless a small, documented compatibility adjustment is essential to run the pinned application revision on the target host.

The current architecture keeps Supabase as an external managed service for application authentication and PostgreSQL data. The V1 target also includes the existing TinyFish API integration. The exact handlers, process model, environment variables, ports, and runtime requirements must be confirmed from the application source before deployment.

### Out of scope / important separation

- This repository is not the Togetherly Workforce application repository.
- This repository is not the separate Togetherly CloudSec Platform / CloudSec Engine project.
- The older Azure Enterprise Infrastructure project is an archive/reference, not the active source of truth for this roadmap.
- No production database changes or application security changes should be made merely as part of Azure infrastructure work. Any such change requires a separately scoped, reviewed, and authorised task.
- Security testing must never target systems without ownership or explicit permission.

## 4. Five-version roadmap

All five versions live in this repository. The version labels describe cumulative maturity stages. The README and version records are the source of truth; update them when scope or implementation changes.

### V1 — Repeatable, Governed Deployment

**Learning alignment:** AZ-900  
**Status:** Planning  
**Goal:** Deploy a pinned Togetherly Workforce revision to Azure with repeatable Azure CLI scripts and a tested deployment process.

Planned capabilities:

- Repository and script conventions.
- A dedicated Azure resource group.
- Virtual Network and subnet.
- Network Security Group with documented inbound and outbound rules.
- Public IP and DNS label, subject to availability.
- Eligible small B-series Ubuntu VM, subject to subscription and regional availability.
- Required resource tags, including project, version, owner, and environment.
- Budget notifications and cost review.
- Private, TLS-only storage account for documented build artifacts or backups, with its purpose and access model defined.
- Ubuntu cloud-init provisioning.
- Nginx web server and reverse proxy.
- Node.js runtime, non-root service user, and systemd-managed service, subject to runtime compatibility verification.
- GitHub Actions build and deployment of a pinned Togetherly revision.
- HTTPS using Let's Encrypt on the Azure DNS name.
- Smoke tests, architecture documentation, cost review, evidence capture, and teardown verification.

**Target request path:**

    Internet
       |
    Azure Public IP / DNS
       |
    Network Security Group
       |
    Ubuntu VM
       |
    Nginx
       |-------------------------------|
       |                               |
    Static frontend              /api/tinyfish
                                       |
                                Local Node.js service
                                       |
                                HTTPS outbound
                                       |
                                    Supabase

This is a target architecture, not a statement that the integration is already deployed or validated. Inspect the actual Togetherly implementation before choosing the Node.js process model and configuring the proxy.

**V1 acceptance criteria:**

- [ ] A pinned Togetherly Workforce build is deployed to the Azure VM.
- [ ] Nginx serves the frontend over HTTPS.
- [ ] Required TinyFish API routes work through the documented Node.js/Nginx integration.
- [ ] Supabase authentication and required application flows work as expected.
- [ ] Network access is limited to documented ports; SSH is restricted to the administrator's current IP.
- [ ] Resources carry the required project, version, owner, and environment tags.
- [ ] Storage is private and TLS-only.
- [ ] GitHub Actions build/deploy workflow succeeds.
- [ ] Smoke tests pass against the deployed application.
- [ ] The environment can be deleted and recreated using repository scripts.
- [ ] Smoke tests pass again after a full rebuild.
- [ ] Evidence, cost notes, known gaps, and the V1 version record are committed.
- [ ] Teardown is verified and no unintended billable project resources remain.

**Evidence checklist:**

- Sanitised deployment logs and relevant Azure CLI output.
- Resource inventory showing required tags.
- Budget configuration and cost view.
- Azure Advisor review and documented findings.
- Storage configuration and access settings.
- HTTPS response and browser screenshot.
- Successful GitHub Actions run.
- Smoke-test output before and after rebuild.
- Architecture diagram and known-gaps record.
- Teardown and post-cleanup verification.

**Known V1 risks:**

- Existing Togetherly TinyFish handlers may require a small Node.js compatibility wrapper. Inspect source before choosing a process model.
- The current V1 plan temporarily uses a restricted environment file for the TinyFish credential. V2 must replace this with Managed Identity and Key Vault. Never commit the file or credential.
- Azure region, VM-size availability, DNS label availability, pricing, and subscription restrictions must be checked before deployment.

### V2 — Identity, Secrets, and Data Governance

**Learning alignment:** SC-900  
**Status:** Planned  
**Goal:** Reduce identity and secret risks and establish a stronger cloud security baseline.

Planned capability areas from the project roadmap:

- Microsoft Entra ID concepts and workload identity.
- Managed Identity for Azure-hosted workloads.
- Azure Key Vault for secrets and key material.
- GitHub Actions OpenID Connect (OIDC) federation where supported, avoiding long-lived deployment secrets.
- Azure RBAC and least-privilege role assignments.
- Data classification and documented data-handling requirements.
- Azure Policy baseline and compliance checks.
- Microsoft Defender for Cloud baseline and findings review, subject to subscription entitlement and pricing.
- STRIDE threat modelling and documented trust boundaries.
- Removal of the temporary V1 secret-on-disk approach after the V2 replacement is tested.

V2 must not be considered complete until identity flows and secret retrieval are tested, permissions are demonstrably limited, and the V1 application behaviour still passes regression tests.

### V3 — Hardening and Cloud Operations

**Learning alignment:** AZ-104  
**Status:** Planned  
**Goal:** Improve operational security, visibility, and recovery of the deployed environment.

Planned capability areas:

- Network segmentation and refined network security rules.
- Azure Monitor and Log Analytics.
- Useful platform and workload metrics.
- Actionable alerts and alert routing.
- Backup configuration and restore testing.
- VM patching and update process.
- Operational runbooks and health checks.
- Resource inventory and configuration review.
- Regression checks for the V1 deployment and V2 identity/security controls.

Monitoring must be tested with real, controlled signals. Backup completion is not enough; a restore must be performed and verified.

### V4 — Detection and Investigation

**Learning alignment:** SC-500 (confirm the current certification code and exam name before using it as a formal learning milestone)  
**Status:** Planned  
**Goal:** Detect and investigate suspicious activity using Azure security telemetry.

Planned capability areas:

- Microsoft Sentinel workspace and data connectors, after pricing and retention review.
- KQL queries for useful security signals.
- Detection rules and alert tuning.
- Python-based cloud posture checks.
- Authorised, controlled security tests against project-owned resources.
- Incident investigation workflow and evidence collection.
- Incident report documenting detection, triage, timeline, impact, response, and lessons learned.

Avoid generating unnecessary telemetry or leaving paid Sentinel/Defender capabilities running without a clear test window and cost review.

### V5 — Enterprise Architecture and Resilience

**Learning alignment:** AZ-305  
**Status:** Planned  
**Goal:** Evaluate and implement appropriate architecture and recovery improvements based on measured needs.

Planned capability areas:

- Hosting and architecture decision records.
- High-availability options and trade-offs.
- Load balancing and scaling where justified.
- Disaster recovery design and implementation.
- Measured Recovery Time Objective (RTO) and Recovery Point Objective (RPO).
- Backup and restore validation.
- Zero Trust architecture review.
- Cost, security, availability, and operational trade-off analysis.
- Final architecture diagram and end-to-end project review.

Do not add expensive high-availability components simply to make the architecture diagram look impressive. Choose components based on documented requirements, subscription limits, cost, and measured value.

## 5. Current architecture and technology direction

| Area | Planned technology / approach |
|---|---|
| Cloud | Microsoft Azure |
| Host operating system | Ubuntu 24.04 LTS |
| Web server / reverse proxy | Nginx |
| Frontend | Existing Togetherly React, TypeScript, and Vite application |
| API runtime | Node.js, subject to compatibility verification |
| Application authentication and data | Supabase, external managed service |
| Infrastructure automation | Azure CLI and Bash; PowerShell where useful |
| CI/CD | GitHub Actions |
| Identity | Microsoft Entra ID and Managed Identity |
| Secrets | Azure Key Vault |
| Governance | Azure RBAC, Azure Policy, Defender for Cloud baseline |
| Monitoring | Azure Monitor and Log Analytics |
| Detection | Microsoft Sentinel and KQL |
| Security automation | Python |
| Infrastructure as Code | Terraform after the planned learning milestone |
| IaC scanning | Checkov alongside Terraform adoption |

Technology choices may change if subscription availability, pricing, security, or compatibility checks show that a planned option is unsuitable. Record material changes in an Architecture Decision Record under docs/decisions/.

## 6. Repository structure

The intended repository layout is:

    togetherly-azure-platform/
    ├── README.md
    ├── PROJECT_OVERVIEW.md
    ├── PROMPT.md
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
    │   └── terraform/
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

The actual repository may contain placeholder files to keep directories in Git. Do not assume a folder means its planned implementation is complete. Inspect the current tree and files before editing or creating anything.

## 7. Engineering workflow

Each version follows this lifecycle:

1. Create a working branch from the previous completed version tag.
2. Rebuild the previous version and run its smoke and regression tests.
3. Implement only the new version's scoped capabilities.
4. Run validation and acceptance tests while Azure resources are live.
5. Capture and sanitise evidence in evidence/vN/.
6. Update the version record, architecture documentation, runbooks, decision records, and changelog.
7. Open a pull request and review the changes.
8. Merge to main only after the required checks and review are successful.
9. Create the version tag only after the acceptance criteria are met.
10. Capture final cost and teardown evidence, then delete the disposable environment.
11. Verify cleanup and record remaining gaps before beginning the next version.

### Cumulative versioning

- One repository contains all five versions.
- Each version builds on the previous completed version.
- Scripts, tests, documentation, and evidence accumulate.
- Azure resources are disposable and may be recreated for each version.
- A version is not complete until acceptance criteria pass and evidence is committed.
- Do not create a vN.0 tag based on planning or partial implementation.

## 8. Security rules and safe operating boundaries

- Never commit passwords, API keys, tokens, private keys, service-principal secrets, .env files, subscription IDs, tenant IDs, personal IP addresses, or unredacted account details.
- Use example configuration files with placeholder values.
- Prefer short-lived credentials and federated identity where supported.
- Use least-privilege permissions.
- Restrict SSH to the administrator's current IP and only expose documented application ports.
- Do not run destructive commands without explaining the target and checking the resource group and subscription.
- Do not modify production Togetherly data or authentication policies as part of infrastructure work without a separately authorised and reviewed change.
- Security testing must target only project-owned or explicitly authorised resources.
- Follow current Microsoft cloud penetration-testing rules and service terms.
- Sanitise screenshots, logs, terminal output, and evidence before committing them.
- Record security findings responsibly; do not publish exploitable details unnecessarily.

## 9. Cost and resource governance

The initial planning envelope recorded in the repository is **US$100 in Azure credit**, subject to the actual subscription terms and current regional pricing.

| Stage | Planning target |
|---|---:|
| V1 | US$15 |
| V2 | US$10 |
| V3 | US$35 |
| V4 | US$30 |
| Reserve | US$10 |
| **Total** | **US$100** |

These are planning estimates, not quotes, guarantees, or spending caps. Validate each deployment with the Azure Pricing Calculator and the subscription's current offers before creating resources.

Cost controls:

- Configure budget notifications before deployment.
- Prefer small eligible VM sizes and stop or delete compute when not in use.
- Time-box Sentinel and paid Defender capabilities.
- Check for disks, public IPs, backups, storage, and other billable resources after teardown.
- Review cost data after deployment and cleanup.
- Remember that Azure budget alerts are notifications, not hard spending caps, and cost reporting can be delayed.
- Do not create paid preview resources or paid branches merely for convenience without explicit approval.

## 10. Current implementation status

The following status is grounded in the repository's current README, V1 specification, and changelog at the time this document was written.

| Milestone | Current status |
|---|---|
| Repository created | Complete |
| Five-version roadmap documented | Complete |
| Initial folder structure | Complete |
| V1 target architecture documented | Complete |
| V1 acceptance criteria documented | Complete |
| Engineering principles and safe workflow documented | Complete |
| Initial cost plan documented | Complete |
| Azure region, VM size, and price validation | Pending |
| Togetherly runtime and API compatibility review | Pending |
| Azure resource creation | Not started |
| V1 deployment scripts | Not started |
| Azure VM, VNet, NSG, Nginx deployment | Not started |
| HTTPS and application deployment | Not started |
| GitHub Actions deployment pipeline | Not started |
| V1 smoke tests and evidence | Not started |
| V2 implementation | Not started |
| V3 implementation | Not started |
| V4 implementation | Not started |
| V5 implementation | Not started |

**Overall status:** Repository foundation and planning are complete; cloud implementation has not started. Do not report a percentage as an audited metric unless it is calculated from a defined task checklist. The presence of documentation or directories is not evidence of deployed infrastructure.

## 11. Immediate next steps

The next work should remain within V1:

1. Inspect the current repository tree and V1 files to confirm the exact starting point.
2. Inspect Togetherly Workforce source code and deployment configuration to establish its actual build process, Node.js requirements, TinyFish API routes, environment variables, and Supabase integration.
3. Identify the Azure subscription type, available credit, allowed regions, VM sizes, and relevant service restrictions without creating resources.
4. Validate the V1 architecture, network exposure, DNS and HTTPS plan, and current price estimates.
5. Design idempotent Azure CLI scripts with explicit prerequisites, validation, safe teardown, and clear output.
6. Add automated checks and smoke tests before provisioning.
7. Only then provision the V1 resource group and infrastructure, capture evidence, test, and tear down.

Do not jump to V2 or create Azure resources before the V1 design, compatibility, cost, and safety checks are ready.

## 12. Definition of project success

The project will be successful when the repository demonstrates a complete, repeatable engineering journey:

- A pinned application revision can be deployed and rebuilt from code.
- Infrastructure is documented, automated, and appropriately secured.
- Identity and secret handling follow least privilege.
- Monitoring and detections produce useful, tested signals.
- Backups and recovery are actually tested.
- Security tests are authorised and documented.
- Each version has acceptance results, sanitised evidence, cost notes, and known gaps.
- The final architecture decisions are justified by requirements and trade-offs.
- The environment can be safely removed without leaving unintended billable resources.

---

## Document maintenance

Update this file whenever the roadmap, architecture, scope, technology choices, safety boundaries, or verified project status changes. Keep it consistent with README.md and docs/versions/. If two documents conflict, inspect the latest approved repository changes and correct the inconsistency rather than silently assuming one is right.
