# Platform Architecture Overview

Togetherly Azure Platform is a cumulative project built in five planned versions. V1 is deployed; V2–V5 describe future work and must not be presented as existing capabilities.

## Version direction

- **V1 — Initial hosting:** Azure VM, VNet, subnet, NSG, Nginx, React/Vite frontend, Node.js Express API and HTTPS. The VM is currently deallocated.
- **V2 — Identity and governance:** planned Managed Identity, Key Vault, RBAC, GitHub OIDC, Azure Policy, data classification and threat modelling. Azure Bicep is planned for infrastructure deployment.
- **V3 — Operations:** planned network hardening, Azure Monitor, Log Analytics, alerts, backup, restore and patching.
- **V4 — Detection:** planned Microsoft Sentinel, KQL, Python posture checks and incident investigation.
- **V5 — Resilience:** planned availability, disaster recovery, measured RTO/RPO and Zero Trust review.

See the [deployed V1 architecture](v1-architecture.md) and [version records](../versions/).
