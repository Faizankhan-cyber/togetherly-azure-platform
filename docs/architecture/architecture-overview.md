# Platform Architecture Overview

Togetherly Azure Platform is a cumulative project built in five versions. V1 and V2 have implementation records; V3–V5 are future plans and must not be presented as deployed capabilities.

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

The V2 design extends the V1 VM-hosted application. It does not introduce a Key Vault Private Endpoint, GitHub OIDC, Azure Functions or Bicep deployment.