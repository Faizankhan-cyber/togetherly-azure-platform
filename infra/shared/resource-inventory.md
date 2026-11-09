# Shared Azure Resource Inventory

**Last updated:** 2026-11-10  
**Scope:** Resources recorded in the shared `rg-togetherly-platform` resource group. This is a documentation inventory, not a live Azure export. Verify live state and dependencies before any change or deletion.

| Resource name | Resource type | Version introduced | Used by versions | Region | Recorded status | Cleanup notes |
|---|---|---|---|---|---|---|
| `rg-togetherly-platform` | Resource Group | Shared | V1–V5 | Central India | Created | Never delete as part of version-specific cleanup |
| `vnet-togetherly-platform` | Virtual Network | V1 | V1–V2; confirm future dependencies | Central India | Created; selected for Key Vault network access | Inspect subnet and NIC dependencies before changes |
| `snet-app` | Subnet | V1 | V1–V2 | Central India | Created; Microsoft.KeyVault service endpoint enabled | Address range `10.10.1.0/24` |
| `nsg-togetherly-v1` | Network Security Group | V1 | V1 | Central India | Created and associated with VM NIC during deployment | Verify current association before changes |
| `vm-togetherly-v1` | Linux Virtual Machine | V1 | V1–V2 | Central India | Running during V2 API service verification on 2026-11-09; current state must be checked | Deallocate when not testing; verify state before any action |
| `vm-togetherly-v1762` | Network Interface | V1 | V1 | Central India | Created | Inspect dependencies before deletion |
| `pip-togetherly-v1` | Public IP address | V1 | V1 | Central India | Created | Verify allocation, SKU and DNS association before deletion |
| `vm-togetherly-v1_OsDisk_1_09bea16dceb540d6bb259d747345f9b0` | Managed disk | V1 | V1 | Central India | Created | Verify attachment and backup requirements before deletion |
| `vm-togetherly-v1-ssh-key` | SSH key resource | V1 | V1 | Central India | Created | Confirm whether retained or retired; never publish private key material |
| `togetherly-key-2026` | Azure Key Vault, Standard | V2 | V2 | Central India | Created; soft delete and purge protection enabled | Preserve while the VM/application depends on it; secret values must never be published |

## V2 configuration notes

- Key Vault uses the Azure RBAC permission model.
- VM system-assigned Managed Identity has Key Vault Secrets User at vault scope.
- Authorised operator has Key Vault Secrets Officer at vault scope.
- Key Vault network access is configured for selected networks/IPs, including `vnet-togetherly-platform` / `snet-app` and a single administrator IP allow-list entry. Do not publish the actual address.
- The `Microsoft.KeyVault` service endpoint is enabled on `snet-app`.
- No Key Vault Private Endpoint was deployed.
- The resource-group Project tag policy assignment is named `V2-Require-Project-Tag-Togetherly` and uses DoNotEnforce. It is a policy assignment, not a separate resource row in this inventory.

## Application services on the VM

These are software services, not separate Azure resources:

- Nginx serves the frontend and reverse-proxies `/api/tinyfish/*`.
- Node.js 22 Express TinyFish API runs as `tinyfish-api.service` on loopback port 3001.
- Let's Encrypt certificate was installed for the Azure-provided DNS hostname.
- Togetherly Workforce frontend build was deployed from the separate application repository.

The API service was active during the V2 verification screenshot. Check the current VM state in Azure before assuming compute charges have stopped.

## Not provisioned as part of V1 or V2

- Azure Function App.
- Azure Functions storage account.
- Application Insights for a Function App.
- Key Vault Private Endpoint or private DNS zone.
- GitHub OIDC federation to Azure.
- Microsoft Sentinel or a dedicated Log Analytics workspace for V2.
- Bicep deployment of the V2 resources.

Confirm resource names, state, attachments and dependencies in the Portal before cleanup. Do not store subscription IDs, tenant IDs, public IP addresses, secrets or personal identifiers here.
