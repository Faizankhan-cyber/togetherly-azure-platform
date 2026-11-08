# V1 Evidence

Store sanitised V1 evidence in the matching subfolders:

- `architecture/`: deployed architecture diagram and decision notes
- `azure-resources/`: Resource Group, VM, disk, and public IP overview
- `networking/`: VNet, subnet, NSG, and inbound rule evidence
- `deployment/`: VM, Nginx, Node.js API service, frontend, DNS, and HTTPS evidence
- `testing/`: API syntax check, health endpoint, four unauthenticated 401 checks, and authenticated app workflow evidence
- `cost/`: actual Azure cost view, remaining credit record, and cleanup outcome

## Verified implementation

The V1 frontend and TinyFish API are deployed on the Azure VM. Nginx serves the frontend and reverse-proxies API requests to a local Node.js Express service managed by systemd. Azure Functions was not deployed.

## Evidence still to capture

- [ ] Resource Group and resource overview
- [ ] VNet, subnet, and NSG rules
- [ ] VM configuration and service state
- [ ] Live HTTPS application page
- [ ] Nginx configuration test and API service status
- [ ] Four unauthenticated API checks showing 401
- [ ] Authenticated TinyFish search/fetch/agent workflow
- [ ] Azure Cost Management view and final resource state

Do not commit screenshots until they have been checked for subscription IDs, tenant IDs, public IP addresses, personal email addresses, usernames where unnecessary, access tokens, API keys, private keys, and raw authentication data. Never commit `.env` files or raw authentication headers.
