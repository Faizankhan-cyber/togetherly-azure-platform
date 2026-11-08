# V1 Architecture

**Status:** Deployed and smoke-tested; see documented limitations  
**Hosting model:** Azure VM, Nginx, static frontend, and local Node.js API service  
**Resource group:** `rg-togetherly-platform` (shared by V1–V5)  
**Region:** Central India

## Integrated application and infrastructure diagram

This diagram represents the implementation that is currently running. Azure Functions was part of an earlier proposal but was not provisioned.

Editable source: [V1 integrated Mermaid diagram](diagrams/v1-architecture.mmd).

## Actual request path

1. The browser opens the Togetherly Workforce app through the Azure-provided DNS name over HTTPS.
2. The public endpoint and NSG allow web traffic to the Ubuntu VM.
3. Nginx terminates TLS and serves the React/Vite production build.
4. Requests under `/api/tinyfish/*` are reverse-proxied to the Node.js Express API at `127.0.0.1:3001`.
5. The API validates the Supabase session and application access before calling external services.
6. The API calls Supabase and TinyFish over HTTPS using server-side environment configuration.
7. API responses return through Nginx to the browser.

## Deployed components

| Component | Responsibility | Observed state |
|---|---|---|
| Resource Group | Shared V1–V5 management boundary | Created |
| VNet / subnet | VM network boundary | Created |
| NSG | Controls inbound SSH, HTTP, and HTTPS | Configured |
| Ubuntu VM | Hosts Nginx, frontend build, and API process | Running during verification |
| Nginx | TLS termination, static hosting, and API reverse proxy | Running |
| React/Vite frontend | Togetherly Workforce user interface | Deployed |
| Node.js Express API | TinyFish search, fetch, agent, and agent-run endpoints | systemd service active |
| Supabase | External managed authentication and PostgreSQL | External dependency |
| TinyFish | External API used by the server-side API | External dependency |

## Network and service boundaries

- Public inbound traffic is limited to the VM's configured NSG rules.
- SSH should remain restricted to the administrator's current public IP.
- Ports 80 and 443 are allowed for web access.
- The Node.js API listens on loopback port 3001. Do not add a public NSG rule for port 3001.
- Nginx proxies the required API path to the loopback service.
- Keep API credentials and Supabase server configuration on the VM, not in the frontend build.
- Supabase and TinyFish remain external services.

## TLS and DNS

The deployed hostname is `togetherly-v1-faizan.centralindia.cloudapp.azure.com`. A Let's Encrypt certificate was installed through Certbot. HTTPS returned HTTP 200, and HTTP returned a redirect to HTTPS. The certificate expiry reported during setup was 2027-02-06; verify renewal status periodically.

## Important architecture decision

The initial plan proposed an Azure Function App and storage account. That plan was not implemented. To complete a working V1 quickly, the TinyFish routes were deployed as a separate Node.js Express service on the VM and managed with systemd. The current topology is therefore VM + Nginx + local Node API, not VM + Azure Functions.

This is a documented scope decision, not a claim that Azure Functions has been deployed. A later version may migrate the API to Azure Functions after reviewing hosting costs, compatibility, secrets, and deployment design.

## Security notes

- API process binds to loopback, not a public interface.
- Nginx forwards the Authorization header to the API.
- API routes return 401 for unauthenticated requests.
- User confirmed authenticated search, fetch, and agent flows in the live application.
- Keep `.env` permission-restricted and excluded from Git.
- Never expose `TINYFISH_API_KEY` in browser code, build variables, screenshots, logs, or repository files.
- Do not alter production Supabase policies or data as part of infrastructure cleanup.

## Not included in V1

Azure Functions, Key Vault, Managed Identity, private endpoints, WAF, Azure Front Door, CI/CD, high availability, and automated infrastructure deployment are not part of the verified V1 deployment. These may be considered in later versions after cost and scope review.
