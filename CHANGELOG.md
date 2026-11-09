# Changelog

## 2026-11-09 — V1 documentation cleanup

- Updated project status to show V1 deployment and testing complete, with the VM deallocated and resource cleanup pending.
- Removed public billing amounts, credit-balance tracking and cost-analysis documentation; personal cost records remain private.
- Updated architecture and network records to describe the deployed VM, Nginx and Node.js Express design.
- Marked the original Azure Functions hosting proposal as superseded.
- Removed unused future-version evidence placeholders and redundant README-only folders.
- Kept the shared resource inventory and V1 provisioning records for safe resource cleanup.
- Recorded that twelve V1 screenshots are in the repository and sanitised replacement copies are still pending publication.

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
- Set V1 infrastructure provisioning to manual Azure Portal work; reserved Azure Bicep for V2.
- Added the V1 architecture diagram, request flows, trust boundaries and implementation notes.
- Added V1 Portal provisioning notes and a reserved V2 Bicep folder.
- Finalised the V1–V5 repository blueprint and documented folder ownership and implementation status.
