# V2 Security Review

**Review date:** 2026-11-10  
**Scope:** Public project documentation, recorded V2 Azure configuration and reported implementation tests.  
**Review type:** Documentation and configuration-evidence review for a learning project; not a penetration test or independent Azure tenant audit.

## Controls recorded as implemented

| Area | Recorded control | Evidence basis |
|---|---|---|
| Workload identity | System-assigned Managed Identity enabled on the VM | Azure Portal screenshot and successful secret retrieval |
| Secret storage | TinyFish credential stored in Key Vault | Key Vault secret inventory; value not displayed |
| Secret access | VM identity assigned Key Vault Secrets User at vault scope | RBAC screenshot |
| Operator access | Authorised operator assigned Key Vault Secrets Officer at vault scope | RBAC screenshot |
| Secret retrieval | Node.js uses Azure Identity and Key Vault SDK; retrieval test succeeded without printing the secret | VM terminal test and reported application integration |
| Secret removal | TinyFish API key removed from VM application `.env` | User-confirmed configuration change |
| Network | Key Vault selected-network access, application VNet/subnet rule and administrator IP allow-list | Key Vault networking configuration and successful retrieval |
| Subnet integration | `Microsoft.KeyVault` service endpoint enabled on application subnet | Azure networking configuration |
| Governance | Resource-group Project tag policy assignment configured as DoNotEnforce | Assignment details |
| Policy evaluation | 8 of 8 resources compliant, 0 non-compliant | Azure Policy compliance scan |
| Application regression | Search, Fetch, Agent execution and status re-tested | User-confirmed workflow testing |

## Public repository review

The following repository items were inspected:

- Platform README, project overview, V2 version record, architecture notes, security baseline, resource inventory and Git ignore rules.
- The separate application repository's README, environment example, package manifest and Git ignore rules.

Findings and actions:

1. The platform repository's `.gitignore` excludes `.env`, `.env.*`, private key extensions and local Azure state files.
2. The application repository's `.env.example` contains placeholders and explicitly warns against placing a service-role key in frontend environment variables.
3. The application repository's `.gitignore` previously ignored only a file named `.env`, not all `.env.*` variants. The ignore rule is being strengthened to exclude environment variants while allowing the placeholder `.env.example`.
4. The reviewed public platform documentation did not contain the TinyFish secret value, subscription ID or tenant ID in its prose.
5. The V1 evidence folder contains sanitised public screenshots according to its evidence record. V2 screenshots have not yet been uploaded and must be reviewed before publication.

This review did not scan all Git history, every binary/image, every commit in both repositories, GitHub Actions secrets, the live VM filesystem, Supabase production settings or Azure tenant-wide IAM. A clean current README is not proof that a secret was never committed in history.

## Residual risks and follow-up

- **Subscription-level Owner roles:** two inherited Owner assignments were observed during the V2 configuration work and were not changed. Review them separately in Azure IAM; this project did not attempt to change subscription ownership.
- **Public Key Vault endpoint:** Key Vault uses its public endpoint with network ACL restrictions and a VNet service endpoint. It does not use a Private Endpoint.
- **Administrator IP allow-list:** the configured address can become stale if the administrator's public IP changes. Keep it private and update only when necessary.
- **Secrets Officer role:** this is a powerful secret-management role. Keep the assignment limited to the authorised operator and review it periodically.
- **DoNotEnforce policy:** the tag policy evaluates compliance but does not block non-compliant resource operations.
- **Public VM web endpoint:** retain only required inbound web rules; SSH should remain limited to a trusted source. Do not expose API port 3001.
- **Credential rotation:** rotate the TinyFish credential if it is suspected to have been exposed. Removing it from the current `.env` file does not remove copies from backups or Git history.
- **Application authorization:** this Azure review does not re-audit Supabase RLS, employee/role authorization or production data access.
- **Host hardening:** current SSH daemon settings, host firewall, package patch level, Nginx TLS configuration and file permissions were not independently inspected during this review.

## Release status and remaining checks

The Git tag and published GitHub release `v2.0.0` already exist. The release notes disclose that the 14 V2 screenshot files are pending owner upload. This review records the implementation release while keeping evidence publication and live operational checks open.

Before publishing the V2 screenshot evidence:

- Upload all 14 screenshots only after redacting account emails, subscription/tenant identifiers, principal IDs, public IP addresses, secret values and tokens.
- Confirm the Key Vault networking screenshot shows the intended VNet/subnet rule and service endpoint without revealing the administrator IP.
- Confirm Search, Fetch and Agent evidence reflects the post-V2 application, not only the earlier V1 tests.
- Check current Azure VM state and Cost Management actuals/forecast. This documentation review did not access the Azure subscription or billing data.
- Recheck the repository for accidental secrets and sensitive image content.

## Conclusion

The V2 implementation evidence supports that Managed Identity, Key Vault secret retrieval, selected-network access, RBAC and tag-policy evaluation were configured and functionally tested. The controls reduce credential exposure and improve governance. Residual risks remain, and this review should not be described as a formal security certification.
