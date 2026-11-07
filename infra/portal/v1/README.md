# V1 — Manual Azure Portal Provisioning

V1 infrastructure is created manually through the Azure Portal. This folder stores the approved configuration plan, portal walkthroughs, and resource-by-resource evidence references. It does not contain deployment scripts.

## Shared platform settings

- Resource Group: `rg-togetherly-platform`
- Resource Group scope: shared by V1–V5
- Provisioning method: Azure Portal
- Version: V1
- Environment: learning

## Planned resource sequence

1. Resource Group (verify existing group before creating)
2. Virtual Network and subnet
3. Network Security Group and reviewed rules
4. Public IP
5. Linux Virtual Machine
6. Nginx configuration on the VM
7. Azure Function App and required storage account
8. HTTPS and reverse-proxy configuration
9. Application integration and verification

## Before creating a resource

Record and verify:

- Active subscription and remaining student credit
- Selected region and service availability
- Resource name and purpose
- SKU/size and estimated cost
- Network exposure and required ports
- Tags: `project=togetherly-azure-platform`, `version=v1`, `environment=learning`
- Whether the resource must remain for later versions

Do not place secrets, access tokens, subscription IDs, tenant IDs, or personal IP addresses in this folder.
