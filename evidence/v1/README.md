# V1 Evidence

Twelve screenshots document the V1 implementation. Sanitised public copies must replace the current originals before this evidence folder is considered safe for public release.

| Folder | Evidence |
|---|---|
| `azure-resources/` | Resource Group and VNet overview |
| `networking/` | Subnet, NSG inbound rules, VM overview and VM network settings |
| `deployment/` | SSH connection, Nginx/API service state and HTTPS deployment |
| `testing/` | TinyFish search, fetch and agent workflows |

## Implementation summary

The V1 frontend and TinyFish API ran on the Azure VM. Nginx served the frontend and reverse-proxied API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed. The VM was user-confirmed stopped and deallocated on 9 November 2026.

## Publication privacy

The current repository screenshots have not yet been replaced with the sanitised copies. The reviewed originals show account email, subscription/network identifiers, public IP information and the SSH source IP. Do not treat the current images as publication-ready.

Keep original screenshots privately. Publish only reviewed, sanitised copies. Never commit `.env` files, private keys, access tokens, API keys, raw authentication headers or unredacted account details.
