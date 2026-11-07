# V1 Azure Portal Resource Checklist

**Status:** Planning only. No resources have been provisioned.

Before creating each resource, verify the active subscription, remaining credit, region, availability, SKU, estimated cost, network exposure, tags, dependencies, and whether later versions will share it.

- [ ] Confirm subscription and credit balance
- [ ] Verify the shared resource group `rg-togetherly-platform` exists or create it only after approval
- [ ] Review VNet and subnet design
- [ ] Review NSG rules
- [ ] Review public IP and DNS choice
- [ ] Review Linux VM size and disk
- [ ] Review Nginx configuration and TLS plan
- [ ] Validate Azure Functions runtime, hosting plan, storage, timeout, and cost
- [ ] Record deployed resources in the shared inventory
- [ ] Capture sanitised evidence

This checklist is not permission to provision resources.
