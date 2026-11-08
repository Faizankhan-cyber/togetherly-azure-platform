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

The V1 frontend and TinyFish API ran on the Azure VM. Nginx served the frontend and reverse-proxied API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed. The VM was user-confirmed stopped and deallocated on 2026-11-09.

## Evidence privacy review

The twelve supplied screenshots were reviewed. They are relevant and useful evidence, but **not ready for public release without redaction**. Redact account email, subscription and network identifiers, public IP information, and the SSH source IP in sanitised copies. Keep original copies privately.

Never commit `.env` files, private keys, access tokens, API keys, raw authentication headers or unredacted account details.
