# V1 Azure Resource Checklist

**Status:** Core V1 resources provisioned and application workflows tested. The VM was user-confirmed deallocated on 9 November 2026. The temporary V1/V2 environment is planned for deletion after preservation and final owner checks.

- [x] Create temporary environment resource group `rg-togetherly-platform`
- [x] Create VNet `vnet-togetherly-platform` and subnet `snet-app`
- [x] Create NSG `nsg-togetherly-v1` and attach it to the VM NIC
- [x] Restrict SSH to the administrator source; allow HTTP/HTTPS
- [x] Provision Ubuntu VM `vm-togetherly-v1`
- [x] Install Nginx and Node.js 22
- [x] Deploy the frontend build and configure Nginx
- [x] Deploy TinyFish Express API and configure systemd
- [x] Configure Nginx reverse proxy for `/api/tinyfish/*`
- [x] Configure HTTPS and HTTP-to-HTTPS redirect
- [x] Record deployed resources in the shared inventory
- [x] Upload twelve sanitised evidence screenshots
- [x] Deallocate the V1 VM
- [ ] Verify current resource state and dependencies
- [ ] Complete the V1/V2 Infrastructure Preservation Pack checks and capture any missing live Portal configuration
- [ ] Delete the temporary resource group after final owner approval; verify orphaned resources and Key Vault recovery separately

**Architecture note:** Azure Functions and its storage account were not provisioned. The V1 API is a Node.js Express service on the VM.

This checklist records project work. It is not permission to create or delete Azure resources.
