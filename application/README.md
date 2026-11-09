# Azure Application Components

This directory documents Azure-specific deployment components. The full Togetherly Workforce source remains in its separate repository: https://github.com/Faizankhan-cyber/togetherly-workforce

## V1 deployed components

- **Frontend:** React/Vite production build deployed on the Azure VM and served by Nginx from `/opt/togetherly/togetherly-workforce/dist`.
- **TinyFish API:** Separate Node.js Express service deployed at `/opt/togetherly/tinyfish-api`.
- **Process management:** `tinyfish-api.service` managed by systemd.
- **Reverse proxy:** Nginx served the frontend and proxied `/api/tinyfish/*` to `127.0.0.1:3001`.
- **Secrets:** API environment values were stored on the VM in a permission-restricted `.env` file excluded from Git.

The VM was user-confirmed deallocated on 9 November 2026. These services were active during deployment verification and are not currently expected to be running.

Azure Functions was considered in the initial design but was not deployed in V1. See the [V1 architecture](../docs/architecture/v1-architecture.md) and [V1 record](../docs/versions/v1.md).

Do not copy secrets or the full application source into this repository.
