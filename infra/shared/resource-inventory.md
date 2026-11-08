# Shared Azure Resource Inventory

**Last updated:** 2026-11-08  
**Scope:** Resources in the shared `rg-togetherly-platform` resource group. Verify live Azure state before any deletion or cost decision.

| Resource name | Resource type | Version introduced | Used by versions | Region | Status | Cleanup notes |
|---|---|---|---|---|---|---|
| `rg-togetherly-platform` | Resource Group | Shared | V1–V5 | Central India | Created | Never delete as part of version-specific cleanup |
| `vnet-togetherly-platform` | Virtual Network | V1 | V1; confirm future dependencies | Central India | Created | Inspect subnet/NIC dependencies before changes |
| `snet-app` | Subnet | V1 | V1 | Central India | Created | Address range `10.10.1.0/24` |
| `nsg-togetherly-v1` | Network Security Group | V1 | V1 | Central India | Created and associated with VM NIC | SSH source is an administrator IP; verify current value in Portal, do not record it here |
| `vm-togetherly-v1` | Linux Virtual Machine | V1 | V1 | Central India | Created; running during final application tests | Review VM, disk, and public IP costs before deallocation or deletion |
| VM NIC (Portal-assigned name) | Network Interface | V1 | V1 | Central India | Created | Identify exact resource name in Portal before cleanup |
| VM public IP (Portal-assigned name) | Public IP address | V1 | V1 | Central India | Created | Verify allocation/SKU and whether DNS is attached before deletion |
| OS disk (Portal-assigned name) | Managed disk | V1 | V1 | Central India | Created | May continue billing after VM deallocation; verify before cleanup |

## Application services on the VM

These are software services, not separate Azure resources:

- Nginx serves the production frontend and reverse-proxies `/api/tinyfish/*`.
- Node.js 22 Express TinyFish API runs as `tinyfish-api.service` on loopback port 3001.
- Let's Encrypt certificate is installed for the Azure-provided DNS hostname.
- Togetherly Workforce frontend build is deployed from the separate application repository.

## Not provisioned

- Azure Function App: not created.
- Azure Functions storage account: not created.
- Application Insights for a Function App: not created as part of this deployment.
- Key Vault: reserved for a later version.

Add rows only from verified Azure resource information. Do not store subscription IDs, tenant IDs, public IP addresses, secrets, or personal identifiers here. Before deleting a resource, inspect its dependencies and confirm it is not shared with another version.
