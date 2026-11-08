# Changelog

All notable project changes are recorded here.

## 2026-11-09 — Repository cleanup and V1 evidence update

- Updated the root README, project overview, repository structure, V1 architecture index, V1 implementation record and evidence guide to match the deployed VM + Nginx + Node.js Express architecture.
- Recorded that Azure Functions was not deployed in V1.
- Added the twelve uploaded V1 screenshots to the evidence index.
- Identified duplicate Azure resource screenshots and removed the duplicate copy from the architecture documentation area.
- Corrected the evidence folder name from `deployement` to `deployment`.
- Removed the obsolete continuation prompt, which contained outdated architecture and deployment status.
- Flagged screenshot privacy review, actual cost review and resource-state decisions as remaining V1 closure tasks.

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
