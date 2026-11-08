# Shared Azure Resource Inventory

**Last updated:** 2026-11-09  
**Scope:** Resources in the shared `rg-togetherly-platform` resource group. Verify live Azure state before any deletion or cost decision.

| Resource name | Resource type | Version introduced | Used by versions | Region | Status | Cleanup notes |
|---|---|---|---|---|---|---|
| `rg-togetherly-platform` | Resource Group | Shared | V1–V5 | Central India | Created | Never delete as part of version-specific cleanup |
| `vnet-togetherly-platform` | Virtual Network | V1 | V1; confirm future dependencies | Central India | Created | Inspect subnet/NIC dependencies before changes |
| `snet-app` | Subnet | V1 | V1 | Central India | Created | Address range `10.10.1.0/24` |
| `nsg-togetherly-v1` | Network Security Group | V1 | V1 | Central India | Created and associated with VM NIC during deployment | SSH source was restricted to an administrator /32; verify current association before cleanup |
| `vm-togetherly-v1` | Linux Virtual Machine | V1 | V1 | Central India | User-confirmed stopped and deallocated on 2026-11-09 | Compute billing should stop while deallocated; disk and public IP may still incur charges |
| `vm-togetherly-v1762` | Network Interface | V1 | V1 | Central India | Created | Exact name visible in supplied Azure Portal screenshot; inspect dependencies before deletion |
| `pip-togetherly-v1` | Public IP address | V1 | V1 | Central India | Created | May continue billing; verify allocation/SKU and DNS association before deletion |
| `vm-togetherly-v1_OsDisk_1_09bea16dceb540d6bb259d747345f9b0` | Managed disk | V1 | V1 | Central India | Created | May continue billing after VM deallocation; verify before deletion |
| `vm-togetherly-v1-ssh-key` | SSH key resource | V1 | V1 | Central India | Created | Confirm whether retained or delete with V1 resources; never publish private key material |

## Application services on the VM

These are software services, not separate Azure resources:

- Nginx served the production frontend and reverse-proxied `/api/tinyfish/*`.
- Node.js 22 Express TinyFish API ran as `tinyfish-api.service` on loopback port 3001.
- Let's Encrypt certificate was installed for the Azure-provided DNS hostname.
- Togetherly Workforce frontend build was deployed from the separate application repository.

The VM is now user-confirmed deallocated, so these services are not currently running.

## Not provisioned

- Azure Function App: not created.
- Azure Functions storage account: not created.
- Application Insights for a Function App: not created as part of this deployment.
- Key Vault: reserved for a later version.

Add rows only from verified Azure resource information. Do not store subscription IDs, tenant IDs, public IP addresses, secrets, or personal identifiers here. Before deleting a resource, inspect its dependencies and confirm it is not shared with another version.
