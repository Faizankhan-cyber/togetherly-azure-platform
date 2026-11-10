# Shared Azure Resource Inventory

**Last updated:** 2026-11-10  
**Scope:** Historical inventory for the temporary `rg-togetherly-platform` Azure environment used by V1 and V2. This is not a live Azure export. Check the Azure Portal before deletion or rebuild.

The project owner has chosen a **rebuild-and-destroy lifecycle**: delete the current V1/V2 Azure environment after preserving its configuration and evidence, then recreate V1 and V2 using Azure Bicep before adding V3 after AZ-104. The resource group is temporary, not a permanent resource group shared across all future versions.

| Resource name | Resource type | Version introduced | Recorded configuration / status | Cleanup and rebuild notes |
|---|---|---|---|---|
| `rg-togetherly-platform` | Resource Group | V1 | Central India; current environment boundary | Planned for deletion after final owner checks; recreate for V3 |
| `vnet-togetherly-platform` | Virtual Network | V1 | Central India; `10.10.0.0/16` | Recreate from preserved configuration |
| `snet-app` | Subnet | V1 | `10.10.1.0/24`; Microsoft.KeyVault service endpoint enabled in V2 | Recreate endpoint setting before Key Vault network rule |
| `nsg-togetherly-v1` | Network Security Group | V1 | SSH TCP/22 restricted to administrator source; TCP/80 and TCP/443 allowed; default inbound deny; associated with VM NIC during deployment | Exact rule names/priorities and source CIDR not published; capture/re-derive before rebuild |
| `vm-togetherly-v1` | Linux Virtual Machine | V1 | Ubuntu Server 24.04 LTS; `Standard_B2als_v2`; V1 deallocated on 2026-11-09; V2 service screenshot showed API running during capture | Recreate from Bicep; verify VM delete options and OS disk handling |
| `vm-togetherly-v1762` | Network Interface | V1 | VM NIC recorded in historical inventory | Verify current association/dependencies |
| `pip-togetherly-v1` | Public IP address | V1 | Public IP used by Azure DNS hostname `togetherly-v1-faizan.centralindia.cloudapp.azure.com` | Exact IP/SKU/allocation not published; recreate a new public IP and DNS label |
| `vm-togetherly-v1_OsDisk_1_09bea16dceb540d6bb259d747345f9b0` | Managed disk | V1 | VM OS disk recorded | Exact size/SKU/caching not captured in this inventory; verify before teardown |
| `vm-togetherly-v1-ssh-key` | SSH public-key resource | V1 | Resource recorded | Private key is not stored in GitHub; create/securely retrieve a replacement |
| `togetherly-key-2026` | Azure Key Vault, Standard | V2 | Central India; Azure RBAC; 90-day soft delete; purge protection enabled | Deletion leaves a recoverable vault; name may remain reserved. Do not attempt to bypass recovery safeguards |
| `TINYFISH-API-KEY` | Key Vault secret | V2 | Secret exists and was Enabled in supplied screenshot; value omitted | Recreate separately using provider credential; never commit value |
| VM system-assigned identity | Managed Identity | V2 | Enabled on `vm-togetherly-v1` | New VM gets a new principal ID; recreate role assignment |
| `V2-Require-Project-Tag-Togetherly` | Azure Policy assignment | V2 | Resource-group scope; `Project=Togetherly`; enforcement mode DoNotEnforce | Recreate at new resource-group scope; scan showed 8/8 compliant at capture time |

## V2 configuration notes

- Key Vault uses the Azure RBAC permission model.
- VM system-assigned Managed Identity has **Key Vault Secrets User** at vault scope.
- Authorised operator has **Key Vault Secrets Officer** at vault scope.
- Two inherited subscription-level Owner assignments appeared in the IAM screenshot; they were not V2-specific changes.
- Key Vault network access is configured for selected networks/IPs, including `vnet-togetherly-platform` / `snet-app` and one administrator IP allow-list entry. The actual address is deliberately omitted.
- The `Microsoft.KeyVault` service endpoint is enabled on `snet-app`.
- No Key Vault Private Endpoint was deployed.
- The policy assignment is named `V2-Require-Project-Tag-Togetherly`, uses parameters `Project` / `Togetherly`, and is configured in DoNotEnforce mode.
- The Key Vault overview screenshot showed tags `Environment=Development`, `Project=Togetherly`, and `Version=V2`. Exact tags on every other resource were not independently captured.

## Application services on the VM

These are software services, not separate Azure resources:

- Nginx serves the frontend and reverse-proxies `/api/tinyfish/*`.
- Node.js 22 Express TinyFish API runs as `tinyfish-api.service` on loopback port 3001.
- Let's Encrypt certificate was installed for the Azure-provided DNS hostname.
- Togetherly Workforce frontend build was deployed from the separate application repository.
- Supabase and TinyFish are external dependencies and are not part of the Azure resource group.

The API service was active during the V2 verification screenshot. The VM's current state must be checked in Azure before teardown.

## Not provisioned as part of V1 or V2

- Azure Function App.
- Azure Functions storage account.
- Application Insights for a Function App.
- Key Vault Private Endpoint or private DNS zone.
- GitHub OIDC federation to Azure.
- Microsoft Sentinel or a dedicated Log Analytics workspace for V2.
- Bicep deployment of V1 or V2.

## Source and confidence notes

- This inventory is a historical configuration record, not a live Azure resource query.
- Exact NSG rule names/priorities, VM NIC and disk properties, public IP SKU/allocation, full tags, and external resources are not fully captured. Check them in Azure before deletion.
- The full preservation checklist and rebuild notes are in [V2 Infrastructure Preservation Pack](../../docs/preservation/v2-infrastructure-preservation-pack.md).
- Never store subscription IDs, tenant IDs, personal IP addresses, secrets, private keys, access tokens or billing account identifiers here.
