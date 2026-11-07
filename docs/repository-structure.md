# Repository Structure

This is the canonical folder blueprint for Togetherly Azure Platform. It covers all five cumulative versions in one repository.

## Complete layout

```text
togetherly-azure-platform/
├── README.md, PROJECT_OVERVIEW.md, CHANGELOG.md, PROMPT.md, .gitignore
├── .github/workflows/README.md
├── docs/
│   ├── repository-structure.md
│   ├── architecture/{README.md, architecture-overview.md, network-topology.md, diagrams/}
│   ├── decisions/{README.md, ADR-*.md}
│   ├── versions/{v1.md, v2.md, v3.md, v4.md, v5.md}
│   ├── security/{README.md, security-baseline.md, threat-model/, data-classification/, access-control/, security-reviews/}
│   ├── runbooks/{deployment, troubleshooting, backup-and-restore, incident-response, disaster-recovery}.md
│   └── cost-management/{README.md, budget-and-alerts.md, resource-cost-tracking.md, cleanup-procedures.md}
├── infra/
│   ├── portal/v1/
│   ├── bicep/{v2/, v3/, v4/, v5/}
│   └── shared/{README.md, naming-conventions.md, resource-inventory.md}
├── application/{README.md, functions/README.md, nginx/README.md}
├── scripts/{deployment/, verification/, maintenance/, cleanup/}
├── tests/{unit/, integration/, security/, smoke/}
└── evidence/{v1/, v2/, v3/, v4/, v5/}
```

The detailed tree and folder responsibilities are also summarised in the root README and PROJECT_OVERVIEW.

## Ownership rules

| Area | Owns |
|---|---|
| `docs/` | Plans, architecture, decisions, security guidance, runbooks, and cost records |
| `infra/` | Azure provisioning notes, infrastructure definitions, and shared resource inventory |
| `application/` | Azure-specific integration components only |
| `scripts/` | Reusable operational and deployment scripts |
| `tests/` | Automated validation |
| `evidence/` | Sanitised implementation proof, grouped by version |
| `.github/workflows/` | Configured and tested repository automation |

## Version rules

- V1 infrastructure is created manually through the Azure Portal under `infra/portal/v1/`.
- V2 is the planned Azure Bicep learning milestone under `infra/bicep/v2/`.
- V3–V5 infrastructure methods and templates are selected when those versions are designed. The reserved Bicep directories do not imply that Bicep has been chosen for every future stage.
- All versions share the resource group `rg-togetherly-platform`. Version cleanup must preserve shared resources still in use.
- Version plans remain in `docs/versions/`; evidence remains in matching `evidence/vN/` directories.

## Application boundary

The Togetherly Workforce application remains in its own repository. Do not copy its full frontend or duplicate its application source here. The `application/functions/` directory is reserved for an Azure Functions adapter only if the proposed V1 Functions architecture is approved and implemented. The `application/nginx/` directory holds platform-specific Nginx configuration.

## Placeholder policy

A README placeholder documents a future responsibility; it is not an implementation. Do not add empty workflow YAML, fake source files, untested deployment scripts, or sample secrets merely to populate the tree. Add implementation files when the corresponding work is actually started.
