# Platform Architecture Overview

Togetherly Azure Platform is a cumulative project built in five versions, with each version scheduled after its associated Azure certification milestone. V1 and V2 are implemented; V3–V5 are future plans and must not be presented as deployed capabilities.

## Certification-driven version sequence

| Version | Certification milestone |
|---|---|
| V1 | After AZ-900 |
| V2 | After SC-500 |
| V3 | After AZ-104 |
| V4 | After SC-500 |
| V5 | After AZ-305 |

This is the planned sequence supplied for the project. Keep the milestone labels as recorded unless the certification plan is deliberately revised.

## Version direction

- **V1 — Initial hosting:** Azure VM, VNet, subnet, NSG, Nginx, React/Vite frontend, Node.js Express API and HTTPS.
- **V2 — Identity and governance:** system-assigned Managed Identity, Key Vault, RBAC, Key Vault network ACLs, VNet service endpoint, Azure Policy tag evaluation and server-side TinyFish secret retrieval. V2 was configured manually through the Azure Portal.
- **V3 — Operations:** planned network hardening, Azure Monitor, Log Analytics, alerts, backup, restore and patching.
- **V4 — Detection:** planned Microsoft Sentinel, KQL, Python posture checks and incident investigation.
- **V5 — Resilience:** planned availability, disaster recovery, measured RTO/RPO and Zero Trust review.

## Architecture documents

- [V1 deployed architecture](v1-architecture.md)
- [V2 identity and Key Vault architecture](v2-architecture.md)
- [Network topology](network-topology.md)
- [Version records](../versions/)

The V2 design extends the V1 VM-hosted application. It does not introduce a Key Vault Private Endpoint, GitHub OIDC, Azure Functions or Bicep deployment. V1 and V2 were originally deployed manually. After each version is closed and its evidence/configuration is preserved, the temporary Azure environment is planned to be deleted. From V3 onward, Azure Bicep will recreate the completed earlier versions and deploy the new version. The current V1/V2 settings and deletion checklist are recorded in the [Infrastructure Preservation Pack](../preservation/v2-infrastructure-preservation-pack.md). Key Vault soft-delete retention and purge protection must be accounted for when choosing vault names during rebuilds.