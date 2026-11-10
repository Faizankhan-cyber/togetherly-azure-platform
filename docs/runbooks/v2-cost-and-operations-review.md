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

The project owner confirmed that the temporary V1/V2 resource group `rg-togetherly-platform` was deleted on 2026-11-10 after the preservation checks. Do not assume separately retained disks, public IPs, snapshots or the soft-deleted Key Vault were permanently removed.

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

## Final review outcome

The documented design avoids additional always-on compute for V2: it reuses the V1 VM, uses a Standard Key Vault, relies on a subnet service endpoint rather than a Private Endpoint, and does not add Azure Functions or a dedicated monitoring workspace. This keeps the V2 resource footprint relatively small, but it does not establish a zero-cost environment.

This final review is based on the repository's resource inventory and operating records. The VM was confirmed running during the V2 service-status verification, but its state was not re-queried during this final documentation review. No live Azure Cost Management data or live resource query was available, so no current charge, remaining student-credit balance, or current VM state is asserted.

## Current-state caveat

The VM was running during the V2 API service verification screenshot. The project owner must check the current state in the Azure Portal before concluding that compute charges have stopped. Confirm **Stopped (deallocated)** when not testing. The OS disk, public IP and Key Vault must be reviewed separately because they may continue to incur charges even when the VM is deallocated.

### Final owner checks

- [ ] Confirm `vm-togetherly-v1` is stopped and deallocated when not in use.
- [ ] Review the attached OS disk and retained public IP for ongoing charges and project dependencies.
- [ ] Review Key Vault operations and the current Standard-tier charge in Cost Analysis.
- [ ] Review current-month actual cost and forecast in Azure Cost Management.
- [ ] Confirm a budget and alert threshold are configured; remember that alerts are not hard spending caps.

## Planned rebuild-and-destroy lifecycle

The project owner has completed the V2 teardown by deleting the temporary Azure environment on 2026-11-10 after preserving the configuration, documentation, release and evidence in GitHub. The full captured V1/V2 configuration and teardown record are maintained in the [V1/V2 Infrastructure Preservation Pack](../preservation/v2-infrastructure-preservation-pack.md).

For V2 closure, the completed sequence and remaining follow-up are:

1. Complete and review the [V1/V2 Infrastructure Preservation Pack](../preservation/v2-infrastructure-preservation-pack.md), including its list of configuration values that still need to be checked in the Portal.
2. Confirm all secrets can be recreated securely. Never commit secret values, credentials, private keys or tokens to Git, Bicep files or parameter files.
3. Check DNS, resource dependencies and resources outside `rg-togetherly-platform` that may be billed separately.
4. **Completed:** Delete the temporary resource group after preservation and owner approval; deletion was confirmed by the owner on 2026-11-10.
5. **Follow-up:** Check Azure Cost Management and the resource inventory after billing data refreshes to confirm which charges have stopped and whether any retained resources remain.
6. Before V3, implement and test Azure Bicep templates to recreate V1 and V2, then deploy V3 after AZ-104. The current Bicep directory is only reserved documentation; deployable templates do not yet exist. Repeat the same preservation and teardown process after future version milestones.

**Key Vault recovery caveat:** `togetherly-key-2026` has 90-day soft-delete retention and purge protection enabled. Resource-group deletion does not immediately purge a soft-deleted vault. The vault name may remain reserved during the retention period, and purge protection prevents early permanent deletion. Plan to use a new versioned Key Vault name during the Bicep rebuild if the original name is unavailable. Keep recovery protections enabled.

The resource-group teardown is complete according to the owner's Azure Portal confirmation on 2026-11-10. A follow-up review of retained resources and delayed charges is still required before declaring the subscription free of V1/V2 costs.

## Safe cleanup rules

- Do not delete resources until the version's source, configuration, documentation, release and sanitised evidence are preserved.
- Check dependencies before deleting the VNet, subnet, NSG, NIC, public IP, VM or disk.
- Keep the Key Vault soft-delete and purge-protection safeguards enabled and account for retained vaults when rebuilding.
- Check for billable resources outside the resource group after deletion.
- Keep all credentials and billing screenshots private.
