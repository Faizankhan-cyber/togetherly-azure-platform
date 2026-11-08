# Azure Functions Adapter

**V1 status:** Not deployed.

An Azure Functions Node.js v4 adapter was considered during architecture planning. The working V1 implementation instead runs the TinyFish API as a Node.js Express service on the Azure VM, managed by systemd and reached through the Nginx reverse proxy.

Do not describe Azure Functions as a deployed V1 component. A future migration may be considered after a separate compatibility, hosting-plan, region, security, and cost review. Preserve session validation, active employee and role checks, URL validation, HTTP methods, status codes, and response behaviour if that migration is approved.
