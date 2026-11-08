# Changelog

All notable project changes will be recorded here.

## 2026-11-08 — V1 deployment

- Provisioned the shared resource group, VNet, application subnet, NSG, and Ubuntu VM in Central India.
- Deployed the Togetherly Workforce React/Vite production build to the VM and served it with Nginx.
- Configured a Let's Encrypt TLS certificate for the Azure-provided DNS hostname and verified HTTPS plus HTTP-to-HTTPS redirect.
- Deployed the TinyFish API as a Node.js Express service on loopback port 3001 and configured systemd to keep it running.
- Configured Nginx to reverse-proxy `/api/tinyfish/*` requests to the local API service.
- Verified API JavaScript syntax checks, the API health endpoint, and systemd service state.
- Verified unauthenticated search, fetch, agent, and agent-run requests return HTTP 401.
- User confirmed authenticated TinyFish search, fetch, and agent workflows in the live application.
- Corrected the V1 architecture and resource inventory to reflect the actual VM-based API deployment. Azure Functions was proposed but not deployed.
- Remaining V1 closure work: sanitised evidence capture, actual Azure cost review, and safe cleanup/resource-state decision.

## Repository foundation

- Confirmed one shared Azure Resource Group for the cumulative V1–V5 platform: `rg-togetherly-platform`.
- Set V1 infrastructure provisioning to manual Azure Portal work; reserved Azure Bicep for V2.
- Added the V1 architecture diagram, request flows, trust boundaries, and implementation notes.
- Added V1 Portal provisioning notes and a reserved V2 Bicep folder.
- Finalised the canonical V1–V5 repository folder blueprint and documented folder ownership and implementation status.
- Added documentation areas for architecture diagrams, ADRs, security, runbooks, cost management, application integration, scripts, tests, and version-specific evidence.
- Kept V1 manual Azure Portal provisioning and reserved Bicep for V2; Terraform is not part of this roadmap.
