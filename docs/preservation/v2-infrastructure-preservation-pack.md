# V2 Infrastructure Preservation Pack

**Purpose:** Preserve the known V1/V2 Azure configuration before the current Azure environment is deleted. This is the handover record for rebuilding the platform after the next certification milestone.

**Prepared:** 2026-11-10  
**Environment:** Azure for Students, Central India  
**Current resource group:** `rg-togetherly-platform`  
**Versions covered:** V1 (AZ-900) and V2 (SC-500)  
**Evidence:** [V1 evidence](../../evidence/v1/) · [V2 evidence](../../evidence/v2/)

> **Important:** This pack is based on the repository's implementation records and the Azure screenshots supplied for this closure. It is not a live Azure export. Verify every resource, property, association and dependency in the Portal immediately before deletion. Unknown values are deliberately marked as not captured rather than guessed.

## 1. Rebuild objective and lifecycle

V1 and V2 were originally provisioned manually through the Azure Portal. After V2 closure, the owner intends to delete the current Azure resource group to avoid idle resource charges. Before V3 (after AZ-104), recreate V1 and V2 using Azure Bicep, then implement V3. Repeat the preserve, verify, release and teardown cycle after each version.

- Preserve source code, deployment instructions, non-secret configuration, architecture records, release tags, sanitised evidence and this pack in GitHub.
- Do not preserve or publish secrets, private keys, tokens, personal IP addresses, subscription IDs, tenant IDs or billing account identifiers.
- Bicep templates are **not yet implemented** for V1 or V2. The `infra/bicep/` folder is currently a reserved location, not a working deployment source.
- The separate Togetherly Workforce application source remains in its own repository: [togetherly-workforce](https://github.com/Faizankhan-cyber/togetherly-workforce).

## 2. Environment summary

| Setting | Recorded value |
|---|---|
| Cloud | Microsoft Azure |
| Subscription type/name | Azure for Students |
| Region | Central India |
| Resource group | `rg-togetherly-platform` |
| V1 milestone | After AZ-900 |
| V2 milestone | After SC-500 |
| Provisioning method used | Azure Portal, manual |
| V1/V2 Bicep deployment | Not performed |
| Current intended action | Delete the resource group after owner verifies the checklist |
| Next rebuild | Recreate V1 and V2 using Bicep before/with V3, after AZ-104 |

The resource group was used as the deployment boundary for the current V1/V2 environment. It is **not** a permanent shared resource group that must be retained. Check for dependencies outside it before deletion.

## 3. Resource inventory to recreate

This inventory combines the existing repository records and the supplied Azure Portal screenshots.

| Resource | Name | Configuration captured | Rebuild notes |
|---|---|---|---|
| Resource group | `rg-togetherly-platform` | Central India | Recreate as the environment's deployment boundary; do not assume it survives teardown |
| Virtual network | `vnet-togetherly-platform` | Address space `10.10.0.0/16`; Central India | Recreate before subnet-dependent resources |
| Subnet | `snet-app` | `10.10.1.0/24`; Microsoft.KeyVault service endpoint enabled in V2 | Recreate V2 service endpoint setting before adding Key Vault VNet rule |
| Network Security Group | `nsg-togetherly-v1` | SSH TCP/22 restricted to one administrator source; HTTP TCP/80 and HTTPS TCP/443 allowed; default inbound deny; associated with VM NIC during V1 | Exact rule names, priorities, source prefix, outbound rules and full current association were not captured in the preservation notes. Re-derive and validate before deployment. Never reuse a stale personal IP allow-list |
| Network interface | `vm-togetherly-v1762` | VM NIC recorded in inventory | Confirm exact NIC naming and NSG/subnet association from live Azure before deletion |
| Linux VM | `vm-togetherly-v1` | Ubuntu Server 24.04 LTS; `Standard_B2als_v2` (2 vCPU / 4 GiB); Central India | V1 was user-confirmed deallocated on 2026-11-09; V2 service screenshot later showed service running. Rebuild only when required |
| OS managed disk | `vm-togetherly-v1_OsDisk_1_09bea16dceb540d6bb259d747345f9b0` | Attached OS disk recorded; exact size, SKU, caching and delete option not captured here | Re-check disk properties and VM delete behaviour; do not assume a VM deletion automatically removes every related resource |
| Public IP | `pip-togetherly-v1` | Public IP resource recorded; Azure DNS hostname used: `togetherly-v1-faizan.centralindia.cloudapp.azure.com` | Exact SKU, allocation method, IP address and DNS label settings are intentionally not recorded. Recreate a new public IP/DNS label; do not expect the old address to return |
| SSH public-key resource | `vm-togetherly-v1-ssh-key` | Resource recorded in inventory | Private key is not stored in this repository. Generate or securely retrieve a replacement key; never upload private key material |
| Azure Key Vault | `togetherly-key-2026` | Standard tier; Central India; Azure RBAC permission model; soft delete enabled for 90 days; purge protection enabled | Vault name may remain unavailable during soft-delete retention. Use a new unique vault name if necessary and update application configuration |
| Key Vault secret | `TINYFISH-API-KEY` | Secret name and enabled status captured; secret value deliberately omitted | Recreate the secret securely from the provider; never put its value in Git, Bicep or example parameters |
| VM system-assigned Managed Identity | Attached to `vm-togetherly-v1` | Enabled in V2; identity principal/object ID visible in Portal but deliberately not copied | New VM identity will have a new principal ID. Recreate role assignment against the new identity |
| Key Vault role assignment | VM identity: **Key Vault Secrets User** | Scope: `togetherly-key-2026` vault | Recreate after the VM identity and vault exist |
| Key Vault role assignment | Authorised operator: **Key Vault Secrets Officer** | Scope: `togetherly-key-2026` vault | Reassign only to the intended operator account; don't hardcode personal object IDs in public templates |
| Azure Policy assignment | `V2-Require-Project-Tag-Togetherly` | Built-in **Require a tag and its value on resources**; scope `rg-togetherly-platform`; parameters `tagName=Project`, `tagValue=Togetherly`; enforcement mode **DoNotEnforce** | Recreate at resource-group scope. DoNotEnforce evaluates without blocking create/update operations |
| Key Vault network rule | Selected VNet/subnet | `vnet-togetherly-platform` / `snet-app`; subnet service endpoint `Microsoft.KeyVault` | Recreate the service endpoint and selected-network rule; no Private Endpoint was deployed |
| Key Vault firewall rule | Administrator allow-list | One single-address (/32) administrator entry was visible | Actual IP is intentionally omitted. Add the current trusted admin IP only if required; keep it private |

### Resource count caveat

The Azure Policy compliance screenshot showed **8 of 8 resources compliant** at the resource-group scope at the time of that scan. The shared inventory also records a VM, NIC, public IP, disk, SSH public-key resource, VNet, Key Vault and resource group. Treat the compliance scan as an evaluation snapshot, not a guaranteed count of every billable or dependent resource at deletion time.

## 4. V1 application and VM configuration

### Actual hosting architecture

V1 is a single-VM deployment, not an Azure Functions deployment.

- OS: Ubuntu Server 24.04 LTS.
- VM size: `Standard_B2als_v2`.
- Web server: Nginx.
- Frontend: React/Vite production build from the separate Togetherly Workforce repository.
- API: Node.js 22 / Express TinyFish API.
- API process: `tinyfish-api.service` managed by systemd.
- API bind address: `127.0.0.1:3001`.
- Reverse proxy: Nginx forwards `/api/tinyfish/*` to the local Express API.
- TLS: Let's Encrypt certificate installed using Certbot; HTTP redirected to HTTPS during V1 verification.
- External dependencies: Supabase and TinyFish.
- Azure Functions and a Functions storage account were considered but **not deployed**.

### Host paths and commands recorded

- API working directory: `/opt/togetherly/tinyfish-api`.
- systemd unit: `tinyfish-api.service`.
- Service entry point observed: `/opt/togetherly/tinyfish-api/src/server.js`.
- Service status was shown as enabled and active during the V2 verification screenshot.
- Node runtime version observed in the V1 configuration record: `v22.23.4`.

### Values that must be re-derived during rebuild

The exact Nginx site file path/content, systemd unit file contents, environment variable names other than the known TinyFish key, package lockfile state on the VM, Certbot renewal configuration, SSH authorized-key state, UFW status, OS patch level and VM backup state are not fully captured in this pack. Rebuild these from the application repository and deployment notes; do not infer undocumented values.

## 5. V2 identity, Key Vault and network configuration

### Managed Identity and RBAC

- System-assigned Managed Identity was enabled on `vm-togetherly-v1`.
- The VM identity was assigned **Key Vault Secrets User** at the Key Vault resource scope.
- The authorised operator was assigned **Key Vault Secrets Officer** at the Key Vault resource scope.
- The screenshot showed two inherited subscription-level Owner entries. These were not changed by V2 and are not a V2-specific permission requirement.
- New VM deployments receive a new system-assigned identity. Recreate the Secrets User assignment using the newly created principal, not the old principal ID.

### Key Vault settings

| Setting | Recorded value |
|---|---|
| Name | `togetherly-key-2026` |
| Region | Central India |
| Pricing tier | Standard |
| Permission model | Azure RBAC |
| Soft delete | Enabled |
| Retention | 90 days |
| Purge protection | Enabled |
| Secret name | `TINYFISH-API-KEY` |
| Vault URI | `https://togetherly-key-2026.vault.azure.net/` |
| Public network access | Allow public access from selected virtual networks and IP addresses |
| VNet rule | `vnet-togetherly-platform` / `snet-app` |
| Subnet service endpoint | `Microsoft.KeyVault` |
| Admin firewall rule | One single-address entry; actual IP omitted |
| Private Endpoint | Not deployed |

**Soft-delete warning:** Deleting the resource group does not bypass Key Vault soft delete or purge protection. The deleted vault can remain recoverable for the 90-day retention period and its name may remain reserved. Do not disable recovery protections just to reuse the name. Use a new unique vault name during rebuild if the original name is unavailable.

### Application integration

The TinyFish API uses:

- `@azure/identity` with `DefaultAzureCredential`.
- `@azure/keyvault-secrets` with `SecretClient`.
- Secret name `TINYFISH-API-KEY`.
- A five-minute in-process secret cache.
- Error logging records the error name, not the secret value.

The TinyFish API key was removed from the VM application's `.env` file in V2. The browser does not receive the TinyFish provider key. Supabase remains an external application/authentication dependency. Do not add a Supabase service-role key to the VM or public repo.

## 6. Azure Policy configuration

| Setting | Value |
|---|---|
| Assignment | `V2-Require-Project-Tag-Togetherly` |
| Built-in definition | Require a tag and its value on resources |
| Scope | `rg-togetherly-platform` |
| Parameter `tagName` | `Project` |
| Parameter `tagValue` | `Togetherly` |
| Enforcement mode | `DoNotEnforce` |
| Recorded compliance | 8/8 compliant, 0 non-compliant at scan time |

The policy's definition effect is Deny, but the assignment's DoNotEnforce mode means it evaluated resources without blocking create/update operations. Preserve this behaviour unless a deliberate change is designed and tested.

The Key Vault overview screenshot showed tags `Environment=Development`, `Project=Togetherly`, and `Version=V2`. Exact tag values on every other resource were not captured and must be checked before rebuilding.

## 7. Cost evidence supplied by the project owner

The supplied November 2026 Cost Analysis screenshot showed a total of **₹189.65** for the selected scope and resource group filter, broken down as follows:

| Resource category/name as shown | Captured cost |
|---|---:|
| Virtual network | ₹76.18 |
| Virtual machine | ₹74.69 |
| Public IP address | ₹21.02 |
| Disk | ₹17.75 |
| Key Vault | Less than ₹0.01 |
| **Displayed total** | **₹189.65** |

A separate Azure Education overview screenshot displayed November costs of **₹159.43**. These two screenshots show different totals and must not be merged or treated as an invoice reconciliation. They may reflect different scope, refresh timing or view settings. Use Azure Cost Management after deletion to review final billed usage and any delayed charges.

These figures are historical screenshots, not a forecast or guarantee of future cost. No subscription IDs, billing account identifiers or credit balance are reproduced in this public preservation file.

## 8. Dependencies and deletion impact

| Item | Dependency / impact |
|---|---|
| Resource group | Deleting it removes the resources contained in it, subject to Azure resource deletion and recovery behaviour |
| VM | Hosts Nginx, frontend build and TinyFish API; app endpoint becomes unavailable |
| VM identity | Tied to VM lifecycle; recreated VM has a new principal |
| NIC / NSG / VNet / subnet | Network dependencies must be removed or detached in the correct order if deleting individually; resource-group deletion is the chosen cleanup boundary |
| OS disk | May remain if delete settings or separate resources retain it; verify after deletion |
| Public IP / DNS label | Old public IP/DNS association is not guaranteed to be reusable |
| Key Vault | Soft-deleted for 90 days; purge protection prevents early purge |
| Key Vault RBAC | Vault-scoped role assignments disappear with the live vault; reassign on rebuild |
| Azure Policy assignment | Resource-group-scoped assignment is part of the environment and must be recreated |
| Supabase | External service; do not delete or modify it as part of Azure resource-group cleanup |
| TinyFish | External provider account/API; revoke or rotate the provider key separately if required |
| GitHub repositories/releases | Independent of Azure resource-group deletion; retain as project records |

## 9. Pre-deletion owner checklist

Complete these checks in Azure Portal immediately before deleting the resource group:

- [ ] Confirm the current GitHub default branch contains this preservation pack and all version records.
- [ ] Confirm V1 and V2 release tags/releases `v1.0.0` and `v2.0.0` remain published.
- [ ] Confirm the twelve V1 sanitised evidence screenshots are present.
- [ ] Confirm all fourteen V2 sanitised evidence screenshots are present.
- [ ] Confirm the Togetherly Workforce application source repository is accessible and contains the required deployment source.
- [ ] Save any missing non-secret configuration that is only visible in the live Portal (especially exact NSG rules/priorities, VM NIC and disk properties, public IP settings, VM delete options, tags and any resources outside the resource group).
- [ ] Confirm no important data exists only on the VM's OS disk. The app source should remain in GitHub; do not rely on the VM disk as the only copy.
- [ ] Confirm no required backup, snapshot, DNS, storage, monitoring or other billable resource exists outside `rg-togetherly-platform`.
- [ ] Confirm no TinyFish Agent run or other workload is still running.
- [ ] Confirm the owner accepts that the application endpoint will stop working after teardown.
- [ ] Confirm the Key Vault's 90-day soft-delete and purge-protection behaviour is understood.
- [ ] Delete `rg-togetherly-platform` only after all checks above are complete.
- [ ] After deletion, inspect Azure Resource Groups, Key Vault deleted-vault recovery, Public IPs, Disks, Snapshots and Cost Management for remaining resources/charges.

## 10. Post-deletion verification

Do not mark cleanup complete merely because the delete operation was submitted.

- [ ] Azure Portal no longer lists `rg-togetherly-platform`.
- [ ] No orphaned NIC, disk, public IP, snapshot or other billable resource remains from this environment unless intentionally retained and documented.
- [ ] Check Key Vault deleted-vault state; retain soft-delete and purge protection.
- [ ] Check current-month Cost Analysis after billing data refreshes; delayed charges can appear.
- [ ] Update the resource inventory to reflect actual deletion and any retained recovery objects.
- [ ] Record the deletion date and Azure confirmation evidence without publishing personal identifiers or billing details.

## 11. Rebuild sequence for V3

After AZ-104, implement and test the Bicep modules before deploying:

1. Resource group and shared naming/tag conventions.
2. VNet, subnet and NSG.
3. VM NIC, public IP and Linux VM with explicit OS disk/delete settings.
4. VM bootstrap/deployment for Nginx, Node.js, frontend and TinyFish API.
5. HTTPS and DNS configuration with a newly allocated public IP and certificate.
6. System-assigned Managed Identity.
7. Key Vault with soft delete and purge protection.
8. Key Vault VNet/subnet service endpoint access and administrator access rule.
9. Key Vault Secrets User role for the new VM identity and Secrets Officer for the authorised operator.
10. Secret creation as a separate secure step, never from a committed plaintext parameter.
11. Resource-group Azure Policy assignment with the intended `Project=Togetherly` parameters and DoNotEnforce mode.
12. Smoke tests, security checks, cost checks and evidence capture.

Use separate Bicep modules and parameter files for reusable infrastructure. Keep environment-specific values non-secret. Never commit secret values, private keys, tokens, personal IPs, subscription IDs or tenant IDs. Add a deployment/teardown runbook and test the deployment in a controlled scope before relying on it.

**This file is a preservation record, not an executable deployment. No Azure resources have been deleted or recreated by creating this document.**
