# V1 Configuration Record

Record approved, non-secret configuration after each resource is created. Verify the live Portal state before changing or deleting resources.

| Resource | Name | Region | SKU / size | Status |
|---|---|---|---|---|
| Resource Group | `rg-togetherly-platform` | Central India | N/A | Created; shared V1–V5 |
| VNet | `vnet-togetherly-platform` | Central India | `10.10.0.0/16` | Created |
| Subnet | `snet-app` | Central India | `10.10.1.0/24` | Created |
| NSG | `nsg-togetherly-v1` | Central India | TCP 22 restricted to admin IP; TCP 80/443 allowed | Created and attached to VM NIC |
| Linux VM | `vm-togetherly-v1` | Central India | Ubuntu Server 24.04 LTS; `Standard_B2als_v2`, 2 vCPU / 4 GiB RAM | Created |
| Public DNS | `togetherly-v1-faizan.centralindia.cloudapp.azure.com` | Central India | Azure-provided DNS | In use |
| Nginx | VM software | Central India | Ubuntu package | Running; HTTPS configured |
| Node.js | VM software | Central India | v22.23.4 observed | Installed |
| TinyFish API | VM software | Central India | Express service on loopback port 3001 | systemd enabled and active |
| Azure Function App | Not created | N/A | N/A | Not deployed |
| Functions storage account | Not created | N/A | N/A | Not deployed |

## Application tags

Resources were tagged with project/environment/version/owner/purpose/cost-centre metadata where supported. Verify exact live tags in Azure Portal.

Never record subscription IDs, tenant IDs, personal IP addresses, passwords, keys, tokens, or secret values here.
