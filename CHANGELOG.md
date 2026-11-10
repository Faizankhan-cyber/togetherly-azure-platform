# Changelog

## 2026-11-10 — Documentation corrections and V3 scope

- Corrected the V2 certification milestone from SC-500 to SC-900. V4 remains SC-500.
- Updated the V2 evidence filename tables to match the files in `evidence/v2/`.
- Renamed the V1 SSH evidence file to remove the double dot.
- Recorded that Azure Bicep is the infrastructure method from V3 onward and Terraform is not used.
- Moved the items excluded from V2 into the V3 plan: Bicep rebuild, GitHub OIDC, Key Vault private endpoint, Azure Monitor and Log Analytics, Sentinel workspace and security testing.
- No controls were added. All V3 items remain planned.

## 2026-11-10 — V2 implementation documentation

- Recorded the V2 Managed Identity, Key Vault, RBAC, network restriction and Azure Policy implementation.
- Documented the VM-side Azure SDK secret retrieval and TinyFish API integration.
- Added V2 architecture, trust boundaries, verification record and residual-risk notes.
- Added cost and operations guidance for the VM, managed disk, public IP and Key Vault.
- Added the 14-item V2 evidence checklist; sanitised screenshots are pending owner upload.
- Clarified that V2 was configured manually through the Azure Portal. GitHub OIDC, Key Vault Private Endpoint, Azure Functions and Bicep deployment were not implemented.

## 2026-11-09 — V1 evidence publication and documentation update

- Confirmed that all twelve sanitised V1 screenshots have been uploaded to `evidence/v1/`; unredacted originals remain private.
- Updated project status to show V1 deployment and testing complete, with the VM deallocated and resource cleanup pending.
- Removed public billing amounts, credit-balance tracking and cost-analysis documentation; personal cost records remain private.
- Updated architecture and network records to describe the deployed VM, Nginx and Node.js Express design.
- Marked the original Azure Functions hosting proposal as superseded.
- Removed unused future-version evidence placeholders and redundant README-only folders.
- Kept the shared resource inventory and V1 provisioning records for safe resource cleanup.

## 2026-11-08 — V1 deployment

- Provisioned the shared resource group, VNet, application subnet, NSG and Ubuntu VM in Central India.
- Deployed the Togetherly Workforce React/Vite production build to the VM and served it with Nginx.
- Configured a Let's Encrypt TLS certificate for the Azure-provided DNS hostname and verified HTTPS plus HTTP-to-HTTPS redirect.
- Deployed the TinyFish API as a Node.js Express service on loopback port 3001 and configured systemd to keep it running.
- Configured Nginx to reverse-proxy `/api/tinyfish/*` requests to the local API service.
- Verified API syntax checks, health endpoint, systemd service state and unauthenticated HTTP 401 responses.
- User confirmed authenticated TinyFish search, fetch and agent workflows in the live application.
- Corrected the V1 architecture and resource inventory to reflect the actual VM-based API deployment. Azure Functions was proposed but not deployed.

## Repository foundation

- Confirmed one shared Azure Resource Group for the cumulative V1–V5 platform: `rg-togetherly-platform`.
- Set V1 infrastructure provisioning to manual Azure Portal work; reserved Azure Bicep for a later milestone.
- Added the V1 architecture diagram, request flows, trust boundaries and implementation notes.
- Added V1 Portal provisioning notes and versioned project documentation.
- Finalised the V1–V5 repository blueprint and documented folder ownership and implementation status.
