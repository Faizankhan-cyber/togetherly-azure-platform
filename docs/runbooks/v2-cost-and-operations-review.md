# V2 Cost and Operations Review

**Scope:** Cost drivers and safe operating practices for the Togetherly Azure V2 learning environment.  
**Subscription:** Azure for Students.  
**Region:** Central India.  
**Last reviewed:** 2026-11-10.

This is a resource-level cost review, not a live billing statement. No exact prices, remaining credit balance or invoice totals are published here. Check Azure Cost Management for current subscription-specific actuals and forecasts.

## Main cost drivers

| Resource or feature | Cost consideration | Operating guidance |
|---|---|---|
| `vm-togetherly-v1` | VM compute is the main variable cost while the VM is running. | Start only for testing or maintenance; deallocate when finished. |
| VM OS managed disk | Storage can continue to incur charges while the VM is deallocated. | Keep only the disk needed for the project; do not delete without a backup and dependency review. |
| Public IP `pip-togetherly-v1` | A retained public IP may continue to incur charges depending on SKU and allocation state. | Check the IP resource's SKU, allocation and billing state separately. Remove only after confirming DNS and VM dependencies are no longer needed. |
| Key Vault `togetherly-key-2026` | Vault tier and secret operations may incur charges. | Keep the Standard tier for the current learning workload; avoid unnecessary polling and extra vaults. |
| Key Vault secret transactions | Reads and other operations may be billable depending on service pricing. | The application helper caches the TinyFish secret for five minutes to reduce repeated reads. |
| Key Vault service endpoint | Does not require a separately provisioned always-on compute resource. | Keep the subnet service endpoint if the VNet-to-vault network rule is required. |
| Managed Identity and RBAC | These access-control features do not require a separately running VM or dedicated identity server. | Retain only the role assignments required by the workload and operator. |
| Azure Policy assignment | Policy evaluation is a governance feature; no separate VM is needed for the assignment. | Keep the assignment in DoNotEnforce mode unless a deliberate future change is reviewed. |

## Safe VM shutdown

After application testing and evidence capture:

1. Stop test traffic and wait for any Agent run to finish.
2. Confirm no maintenance or evidence capture still needs the VM.
3. In Azure Portal, open `vm-togetherly-v1`.
4. Select **Stop** and confirm the status becomes **Stopped (deallocated)**.
5. Recheck the VM status before leaving the environment.
6. Remember that deallocation does not delete the VM, OS disk, Key Vault or public IP. These resources may continue to incur charges.

Do not delete the shared resource group or remove the managed disk as a shortcut for reducing compute charges.

## Monthly cost check

In Azure Portal:

1. Open **Cost Management + Billing** for the Azure for Students subscription.
2. Review **Cost analysis** for the current month and group by resource.
3. Check actual cost and forecast, not just the subscription credit banner.
4. Review the VM, disk, public IP and Key Vault separately.
5. Set a budget and alert threshold appropriate to the remaining student credits.
6. Recheck after any VM start, resource resize, new public IP, Private Endpoint or other deployment.

## V2 cost decisions

- No Key Vault Private Endpoint was deployed. This avoids adding private endpoint and private DNS resources to the current learning setup.
- The Key Vault remains Standard tier.
- The existing VM was reused rather than creating another VM.
- No Azure Functions, Function App, extra storage account or monitoring workspace was created as part of V2.
- No exact monthly cost is claimed because live billing data has not been retrieved for this review.

## Current-state caveat

The VM was running during the V2 API service verification screenshot. Its current state must be checked in the Azure Portal before concluding that compute charges have stopped. The OS disk and public IP must also be reviewed separately.

## Safe cleanup rules

- Preserve `rg-togetherly-platform`; it is shared across project versions.
- Do not delete Key Vault while the VM or application still depends on it.
- Do not delete the VNet, subnet, NSG, NIC, public IP or disk without checking dependencies.
- Keep the Key Vault soft-delete and purge-protection safeguards enabled.
- Keep all credentials and billing screenshots private.
