# V1 Architecture

**Implementation status:** Deployed and smoke-tested; VM deallocated on 2026-11-09  
**Hosting model:** Azure VM, Nginx, static frontend, and local Node.js API service  
**Resource group:** `rg-togetherly-platform` (shared by V1–V5)  
**Region:** Central India

## Integrated application and infrastructure diagram

This diagram represents the V1 implementation that was deployed and tested. The VM is now deallocated, so the application is not expected to be reachable. Azure Functions was part of an earlier proposal but was not provisioned.

Editable source: [V1 integrated Mermaid diagram](diagrams/v1-architecture.mmd).

## Actual request path

1. The browser opened the Togetherly Workforce app through the Azure-provided DNS name over HTTPS.
2. The public endpoint and NSG allowed web traffic to the Ubuntu VM.
3. Nginx terminated TLS and served the React/Vite production build.
4. Requests under `/api/tinyfish/*` were reverse-proxied to the Node.js Express API at `127.0.0.1:3001`.
5. The API validated the Supabase session and application access before calling external services.
6. The API called Supabase and TinyFish over HTTPS using server-side environment configuration.
7. API responses returned through Nginx to the browser.

## Deployed components

| Component | Responsibility | Recorded state |
|---|---|---|
| Resource Group | Shared V1–V5 management boundary | Created |
| VNet / subnet | VM network boundary | Created |
| NSG | Controls inbound SSH, HTTP, and HTTPS | Configured during deployment |
| Ubuntu VM | Hosts Nginx, frontend build, and API process | User-confirmed deallocated on 2026-11-09 |
| Nginx | TLS termination, static hosting, and API reverse proxy | Running during deployment verification |
| React/Vite frontend | Togetherly Workforce user interface | Deployed |
| Node.js Express API | TinyFish search, fetch, agent, and agent-run endpoints | systemd service active during deployment verification |
| Supabase | External managed authentication and PostgreSQL | External dependency |
| TinyFish | External API used by the server-side API | External dependency |

## Network and service boundaries

- Public inbound traffic was limited to the VM's configured NSG rules.
- SSH was restricted to the administrator's source address during deployment.
- Ports 80 and 443 were allowed for web access.
- The Node.js API listened on loopback port 3001. It was not intended to be publicly exposed.
- Nginx proxied the required API path to the loopback service.
- API credentials and Supabase server configuration were kept on the VM, not in the frontend build.
- Supabase and TinyFish remained external services.

## TLS and DNS

The deployed hostname was `togetherly-v1-faizan.centralindia.cloudapp.azure.com`. A Let's Encrypt certificate was installed through Certbot. HTTPS returned HTTP 200, and HTTP returned a redirect to HTTPS during deployment verification. The certificate expiry reported during setup was 2027-02-06; current validity and renewal status have not been rechecked.

## Hosting decision

The initial plan proposed an Azure Function App and storage account. That plan was not implemented. The TinyFish routes were deployed as a separate Node.js Express service on the VM and managed with systemd. The implemented topology was VM + Nginx + local Node API, not VM + Azure Functions.

A later version may consider migrating the API to Azure Functions after reviewing hosting costs, compatibility, secrets, and deployment design.

## Security notes

- API process was shown bound to loopback, not a public interface.
- Nginx forwarded the Authorization header to the API.
- API routes returned 401 for unauthenticated requests.
- The user confirmed authenticated search, fetch, and agent flows in the live application.
- Keep `.env` permission-restricted and excluded from Git.
- Never expose `TINYFISH_API_KEY` in browser code, build variables, screenshots, logs, or repository files.
- Do not alter production Supabase policies or data as part of infrastructure cleanup.

## Not included in V1

Azure Functions, Key Vault, Managed Identity, private endpoints, WAF, Azure Front Door, CI/CD, high availability, and automated infrastructure deployment were not part of the verified V1 deployment. These may be considered in later versions after scope and cost review.
