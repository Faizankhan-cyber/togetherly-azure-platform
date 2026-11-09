# V1 Evidence

Twelve sanitised screenshots document the V1 implementation.

| Folder | Evidence |
|---|---|
| `azure-resources/` | Resource Group and VNet overview |
| `networking/` | Subnet, NSG inbound rules, VM overview and VM network settings |
| `deployment/` | SSH connection, Nginx/API service state and HTTPS deployment |
| `testing/` | TinyFish search, fetch and agent workflows |

## Implementation summary

The V1 frontend and TinyFish API ran on the Azure VM. Nginx served the frontend and reverse-proxied API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed. The VM was user-confirmed stopped and deallocated on 9 November 2026.

## Privacy

The published screenshots are sanitised copies. Keep original screenshots privately. Review any future screenshots and logs before publishing them.

Never commit `.env` files, private keys, access tokens, API keys, raw authentication headers or unredacted account details.
