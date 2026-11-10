# V1 — Azure Portal Provisioning Record

V1 infrastructure was provisioned manually through the Azure Portal. This folder records the resource configuration and completion checklist; it does not contain deployment scripts.

## Shared platform settings

- Resource group: `rg-togetherly-platform`
- Scope: temporary V1/V2 environment; planned for deletion after preservation
- Provisioning method: Azure Portal
- Region: Central India
- Environment: learning

## Implemented sequence

1. Verified the shared resource group.
2. Created the VNet and application subnet.
3. Created the Network Security Group and configured inbound rules.
4. Created the public IP and Linux VM.
5. Installed Nginx and Node.js on Ubuntu.
6. Deployed the React/Vite frontend build.
7. Deployed the TinyFish Node.js Express API as a systemd service on loopback.
8. Configured Nginx to serve the frontend and reverse-proxy `/api/tinyfish/*`.
9. Configured HTTPS and the HTTP-to-HTTPS redirect.
10. Tested unauthenticated API rejection and authenticated application workflows.

**Architecture clarification:** Azure Functions and its storage account were considered during planning but were not deployed. The V1 API runs on the VM as Node.js Express.

See the [V1/V2 Infrastructure Preservation Pack](../../../docs/preservation/v2-infrastructure-preservation-pack.md), [V1 configuration record](configuration-record.md), [resource checklist](resource-checklist.md) and [shared resource inventory](../../shared/resource-inventory.md).

Do not commit secrets, subscription identifiers, tenant identifiers, personal IP addresses or private key material.
