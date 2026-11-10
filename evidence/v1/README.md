# V1 Evidence

**V1 status: Implemented; evidence uploaded.** Twelve sanitised screenshots document the V1 implementation. All 12 public copies are present in this evidence folder.

| Folder | Evidence |
|---|---|
| `azure-resources/` | Resource Group and VNet overview |
| `networking/` | Subnet, NSG inbound rules, VM overview and VM network settings |
| `deployment/` | SSH connection, Nginx/API service state and HTTPS deployment |
| `testing/` | TinyFish search, fetch and agent workflows |

## Implementation summary

The V1 frontend and TinyFish API ran on the Azure VM. Nginx served the frontend and reverse-proxied API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed. The VM was user-confirmed stopped and deallocated on 9 November 2026. The shared temporary V1/V2 resource group was owner-confirmed deleted on 10 November 2026.

## Publication privacy

The sanitised replacement screenshots have been uploaded to this folder. The original unredacted screenshots are kept privately and are not intended for public release.

Keep original screenshots privately. Publish only reviewed, sanitised copies. Never commit `.env` files, private keys, access tokens, API keys, raw authentication headers or unredacted account details.
