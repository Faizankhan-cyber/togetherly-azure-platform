# V1 Evidence

Evidence is grouped by deployment area:

| Folder | Evidence |
|---|---|
| `azure-resources/` | Resource Group and VNet overview |
| `networking/` | Subnet, NSG inbound rules, VM overview and VM network settings |
| `deployment/` | SSH connection, Nginx/API service state and HTTPS deployment |
| `testing/` | TinyFish search, fetch and agent workflows |
| `cost/` | Actual Azure cost view, remaining credit and final cleanup record |
| `architecture/` | Architecture-specific evidence, if needed |

Twelve screenshots have been uploaded across the resource, networking, deployment and testing folders.

## Deployed implementation

The V1 frontend and TinyFish API run on the Azure VM. Nginx serves the frontend and reverse-proxies API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed.

## Evidence privacy review

**Do not treat uploaded screenshots as publication-ready until they have been reviewed.** Some Azure Portal captures may show subscription identifiers, public IP addresses, SSH source IP addresses or account details. Redact these details before keeping the screenshots in this public repository. Keep original copies privately.

Never commit `.env` files, private keys, access tokens, API keys, raw authentication headers or unredacted account details.
