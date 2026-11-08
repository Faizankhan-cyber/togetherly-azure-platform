# Resource Cost Tracking

Maintain a resource-by-resource cost record using verified Azure Cost Management and subscription data. Do not treat portal estimates as actual charges.

## V1 resources to review

| Resource | Cost consideration | Review action |
|---|---|---|
| `vm-togetherly-v1` | Compute charges while running; exact price depends on offer, region, and billing | Record actual billed amount and runtime window |
| VM OS disk | May continue billing when VM is deallocated | Check disk SKU and actual daily/monthly cost |
| VM public IP | May incur charges depending on SKU/allocation and subscription terms | Verify public IP SKU and billed amount |
| VNet, subnet, NSG | No charge for some basic constructs, but associated services may cost | Confirm actual billing rather than assuming |
| TLS certificate | Let's Encrypt certificate itself is free; VM/network resources remain billable | No separate certificate purchase expected |
| Node.js API | Runs on the VM; compute cost is included in VM runtime | Keep service stopped if VM is deallocated |

The VM size displayed an estimated price of approximately US$17.96/month at creation time. This is not a quote or actual charge. It assumes a particular pricing display and does not represent all resource costs.

## Final V1 cost review

1. Open Azure Cost Management for the correct subscription.
2. Set the date range to cover the V1 deployment period.
3. Group costs by resource.
4. Record the actual amount and currency for the VM, disk, and public IP.
5. Check remaining Azure for Students credit in the Sponsorships portal.
6. Record the review date and whether resources remain running, deallocated, or deleted.

Budget alerts are notifications, not spending caps. Do not write secrets, subscription identifiers, or account-specific identifiers into this file.
