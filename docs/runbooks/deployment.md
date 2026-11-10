# V1 Deployment Runbook

**Deployment model:** Manual Azure Portal provisioning; application deployment over SSH.  
**Record type:** Historical procedure for the V1 deployment.

## Prerequisites

- Access to the Azure subscription; create a temporary deployment resource group named `rg-togetherly-platform` for the V1/V2 environment.
- Confirm available student credit and estimated costs before creating or starting resources.
- Verify resource region, VM SKU, public IP, disk, VNet, subnet, and NSG settings.
- Keep the SSH private key and all application secrets outside Git.
- Confirm the Togetherly Workforce source repository and required environment variable names.

## Deployed sequence

1. Create or verify the temporary V1/V2 environment resource group.
2. Create the VNet `vnet-togetherly-platform` and subnet `snet-app`.
3. Create `nsg-togetherly-v1`; allow SSH only from the administrator's current public IP and allow HTTP/HTTPS for the public web application.
4. Provision `vm-togetherly-v1` with Ubuntu Server 24.04 LTS in Central India and attach it to the subnet/NSG.
5. Install Nginx, Node.js 22, Git, and required utilities.
6. Clone the Togetherly Workforce repository, install dependencies, configure the frontend environment locally, and build the production frontend.
7. Deploy the frontend build to the Nginx document root.
8. Deploy the separate TinyFish Express API under `/opt/togetherly/tinyfish-api`.
9. Create a permission-restricted API `.env` file on the VM. Never paste secret values into shared chat, Git, screenshots, or logs.
10. Configure the API to listen on `127.0.0.1:3001`; manage it with `tinyfish-api.service` under systemd.
11. Configure Nginx to serve the SPA and proxy `/api/tinyfish/*` to the local API.
12. Install the Let's Encrypt certificate with Certbot and verify HTTP redirects to HTTPS.

## Verification

```bash
sudo nginx -t
sudo systemctl is-enabled tinyfish-api
sudo systemctl is-active tinyfish-api
curl -i http://127.0.0.1:3001/health
curl -I https://togetherly-v1-faizan.centralindia.cloudapp.azure.com/
```

Run unauthenticated smoke tests for search, fetch, agent, and agent-run. Each should return 401 without a valid session. Then sign in through the real application and verify the authenticated search, fetch, and agent flows. Do not use real user credentials in terminal commands or screenshots.

## Actual hosting decision

The initial proposal to host the TinyFish API on Azure Functions was not implemented. V1 used a Node.js Express service on the VM. Azure Functions and its storage account were not deployed.

## Rollback and cleanup

- If Nginx configuration fails, restore the last known-good configuration before reloading.
- If the API service fails, inspect `systemctl status tinyfish-api` and `journalctl -u tinyfish-api`; never paste secret-bearing logs into GitHub.
- Before deallocating or deleting resources, inspect dependencies and Azure Cost Management.
- The owner intends to delete the temporary `rg-togetherly-platform` environment after V2 closure and preservation. Follow the [V1/V2 Infrastructure Preservation Pack](../preservation/v2-infrastructure-preservation-pack.md); do not delete it until all pre-deletion checks are complete and the owner approves.
- A deallocated VM may still incur disk and public IP charges. Review each resource separately.

The V1 VM was user-confirmed deallocated on 9 November 2026. This runbook documents the historical deployment and does not mean the services are currently running.
