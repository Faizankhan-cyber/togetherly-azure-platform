# Repository Structure

This document describes the maintained layout for the five-version Togetherly Azure Platform repository.

## Top-level layout

```text
togetherly-azure-platform/
├── README.md
├── PROJECT_OVERVIEW.md
├── CHANGELOG.md
├── .gitignore
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── versions/
│   ├── security/
│   ├── runbooks/
│   └── cost-management/
├── infra/
│   ├── portal/v1/
│   ├── bicep/v2/
│   └── shared/
├── application/
│   └── nginx/
└── evidence/
    ├── v1/
    │   ├── azure-resources/
    │   ├── networking/
    │   ├── deployment/
    │   ├── testing/
    │   └── cost/
    └── v2-v5/
```

## Ownership

| Area | Responsibility |
|---|---|
| `docs/` | Plans, architecture, decisions, security guidance, runbooks and cost records |
| `infra/` | Azure provisioning notes, infrastructure definitions and shared resource inventory |
| `application/` | Azure-specific integration components and deployment notes |
| `evidence/` | Sanitised proof grouped by version |

## Version rules

- V1 infrastructure is provisioned manually through the Azure Portal.
- V2 is the planned Azure Bicep milestone.
- V3–V5 infrastructure methods are selected when those versions are designed.
- All versions share `rg-togetherly-platform`. Version-specific cleanup must preserve shared resources still in use.
- Version plans live in `docs/versions/`; evidence lives in the matching `evidence/vN/` folder.

## Application boundary

The Togetherly Workforce application source remains in its own repository. Do not copy the full application or duplicate its source here. The deployed V1 TinyFish API is a Node.js Express service on the VM, not an Azure Functions app.

## Placeholder policy

A README-only folder documents a planned responsibility; it is not an implementation. Add code, scripts or workflow files only when the corresponding feature is actually built and tested. Keep future-version placeholders only where they support the agreed roadmap.
