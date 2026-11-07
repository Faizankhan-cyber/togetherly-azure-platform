# Changelog

All notable project changes will be recorded here.

## Unreleased

- Confirmed one shared Azure Resource Group for the cumulative V1–V5 platform: `rg-togetherly-platform`.
- Set V1 infrastructure provisioning to manual Azure Portal work; reserved Azure Bicep for V2.
- Replaced the incorrect local Node.js API-server assumption with the proposed Azure Functions hosting design for the existing Vercel-style TinyFish handlers.
- Added the V1 architecture diagram, request flows, trust boundaries, and open implementation decisions.
- Added V1 Portal provisioning notes and a reserved V2 Bicep folder.
- Updated README, project overview, V1 plan, and continuation prompt to reflect the current architecture and provisioning approach.
- Removed unused Terraform and V1 automation placeholder files.
