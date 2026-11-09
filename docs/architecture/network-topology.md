# V1 Network Topology

**Status:** Deployed configuration recorded from V1 implementation evidence. Verify the live Azure Portal state before making changes.

| Item | Recorded configuration |
|---|---|
| Region | Central India |
| Virtual Network | `vnet-togetherly-platform` |
| VNet address space | `10.10.0.0/16` |
| Subnet | `snet-app` |
| Subnet address range | `10.10.1.0/24` |
| Network Security Group | `nsg-togetherly-v1` |
| Workload | Ubuntu Linux VM `vm-togetherly-v1` |

## Inbound traffic

- SSH (TCP 22) was restricted to an administrator source address during deployment. Do not publish the actual address.
- HTTP (TCP 80) and HTTPS (TCP 443) were allowed for web traffic.
- The default inbound deny rule remained in effect.
- The NSG was associated with the VM network interface during deployment.
- The Node.js API listened on loopback port 3001 and was not intended to be exposed publicly.

The VM was user-confirmed deallocated on 9 November 2026. Verify current resource associations and dependencies in Azure before cleanup. Never publish subscription identifiers, public IP addresses or personal network addresses.
