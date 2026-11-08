# V1 Azure Portal Resource Checklist

**Status:** Core V1 resources provisioned in Central India. Use Azure Portal to verify current state before cleanup.

- [x] Confirm subscription and credit balance before provisioning
- [x] Use shared resource group `rg-togetherly-platform`
- [x] Create VNet `vnet-togetherly-platform` and subnet `snet-app`
- [x] Create NSG `nsg-togetherly-v1` and attach it to the VM NIC
- [x] Restrict SSH to the administrator's public IP; allow HTTP/HTTPS
- [x] Provision Ubuntu VM `vm-togetherly-v1`
- [x] Install Nginx and Node.js 22
- [x] Deploy the frontend build and configure Nginx
- [x] Deploy TinyFish Express API and configure systemd
- [x] Configure Nginx reverse proxy for `/api/tinyfish/*`
- [x] Configure HTTPS and HTTP-to-HTTPS redirect
- [x] Record deployed resources in the shared inventory
- [ ] Capture sanitised evidence
- [ ] Review actual costs and remaining credit
- [ ] Decide whether to retain, deallocate, or delete V1 resources individually

**Architecture note:** Azure Functions and its storage account were not provisioned. The V1 API is a Node.js Express service on the VM. This checklist is a record of work, not permission to create or delete additional resources.
