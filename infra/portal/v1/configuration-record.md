# V1 Configuration Record

This records the non-secret configuration observed during V1 deployment. It is a historical record, not a live Azure inventory. Verify the current Portal state before changing or deleting resources. See the [V1/V2 Infrastructure Preservation Pack](../../../docs/preservation/v2-infrastructure-preservation-pack.md) for the pre-deletion checklist and rebuild record.

| Resource | Name | Region | SKU / size | Recorded status |
|---|---|---|---|---|
| Resource Group | `rg-togetherly-platform` | Central India | N/A | Created; temporary V1/V2 environment, planned for deletion after preservation |
| VNet | `vnet-togetherly-platform` | Central India | `10.10.0.0/16` | Created |
| Subnet | `snet-app` | Central India | `10.10.1.0/24` | Created; `10.10.1.0/24` |
| NSG | `nsg-togetherly-v1` | Central India | TCP 22 restricted to administrator source; TCP 80/443 allowed | Created and associated with VM NIC during deployment |
| Linux VM | `vm-togetherly-v1` | Central India | Ubuntu Server 24.04 LTS; `Standard_B2als_v2`, 2 vCPU / 4 GiB RAM | Created; user-confirmed stopped and deallocated on 2026-11-09 |
| Public DNS | `togetherly-v1-faizan.centralindia.cloudapp.azure.com` | Central India | Azure-provided DNS | Used during deployment; current association not rechecked |
| Nginx | VM software | N/A | Ubuntu package | Running during deployment verification |
| Node.js | VM software | N/A | v22.23.4 observed | Installed |
| TinyFish API | VM software | N/A | Express service on loopback port 3001 | systemd enabled and active during deployment verification |
| Azure Function App | Not created | N/A | N/A | Not deployed |
| Functions storage account | Not created | N/A | N/A | Not deployed |

## Tags

The original deployment notes indicate project/environment/version/owner/purpose/cost-centre tags were applied where supported. Exact live tag values have not been independently rechecked. Verify tags in Azure Portal before relying on them.

Never record subscription IDs, tenant IDs, personal IP addresses, passwords, keys, tokens, or secret values here.
