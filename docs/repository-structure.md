# Repository Structure

This repository stores the Azure platform's architecture, deployment records, infrastructure notes and implementation evidence. The Togetherly Workforce application source is maintained separately.

```text
togetherly-azure-platform/
├── README.md
├── PROJECT_OVERVIEW.md
├── CHANGELOG.md
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── runbooks/
│   └── versions/
├── infra/
│   ├── portal/v1/
│   ├── bicep/v2/
│   └── shared/
├── application/
└── evidence/
    └── v1/
        ├── azure-resources/
        ├── networking/
        ├── deployment/
        └── testing/
```

## Directory purpose

| Directory | Purpose |
|---|---|
| `docs/architecture/` | Deployed architecture and editable diagrams |
| `docs/decisions/` | Important technical decisions |
| `docs/runbooks/` | Deployment and operational procedures |
| `docs/versions/` | Status and scope for V1–V5 |
| `infra/portal/v1/` | V1 manual provisioning records |
| `infra/bicep/v2/` | Reserved for planned V2 Bicep templates |
| `infra/shared/` | Shared Azure resource inventory and naming notes |
| `application/` | Azure-specific application deployment notes |
| `evidence/v1/` | Sanitised screenshots from the completed V1 deployment |

## Version rules

- V1 was provisioned manually through the Azure Portal.
- Azure Bicep is planned for V2.
- V3–V5 implementation methods will be selected when those versions are designed.
- All versions use the shared `rg-togetherly-platform` resource group. Preserve shared resources during version-specific cleanup.
- Only add implementation files and evidence when the related work has actually been completed and verified.

Do not create placeholder folders or README files merely to reserve space for possible future work. Add them when the corresponding version begins.
