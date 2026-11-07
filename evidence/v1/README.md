# V1 Evidence

Store sanitised V1 evidence in the matching subfolders:

- `architecture/`: reviewed architecture snapshots and decision references
- `azure-resources/`: Azure resource overview and configuration
- `networking/`: VNet, subnet, NSG, and ingress evidence
- `deployment/`: VM, Nginx, frontend, Functions, DNS, and HTTPS evidence
- `testing/`: test plans and verified results
- `cost/`: pricing assumptions, actual cost, alerts, and cleanup records

No Azure resources have been provisioned for this project yet, so deployment evidence does not exist. Never commit secrets, tokens, subscription IDs, tenant IDs, personal IP addresses, or raw authentication data.
