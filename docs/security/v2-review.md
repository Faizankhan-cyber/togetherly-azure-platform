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
3. The application repository's `.gitignore` currently ignores `.env` but does not yet ignore all `.env.*` variants. A direct update was rejected by GitHub branch protection because changes must go through a pull request with required status checks. Treat this as an open repository-hygiene finding; add `.env.*` and an explicit `!.env.example` exception through a reviewed PR.
4. The reviewed public platform documentation did not contain the TinyFish secret value, subscription ID or tenant ID in its prose.
5. The V1 evidence folder contains sanitised public screenshots according to its evidence record. All 14 V2 screenshot files are present under `evidence/v2/`; full-size visual review for redactions and readability remains outstanding.

This review did not scan all Git history, every binary/image, every commit in both repositories, GitHub Actions secrets, the live VM filesystem, Supabase production settings or Azure tenant-wide IAM. A clean current README is not proof that a secret was never committed in history. The 14 V2 image files are present in the platform repository, but their full-size visual contents were not inspected as part of this review.

## Application API source review

The supplied TinyFish Express route and URL-helper source was reviewed alongside the repository records.

### Positive controls observed

- TinyFish routes call the shared authorization helper before using the provider API key.
- The authorization helper verifies the Supabase bearer token, active employee membership and an allowed application role.
- Search query and Agent goal lengths are bounded; Fetch accepts between one and ten URLs.
- The URL helper restricts protocols to HTTP/HTTPS, rejects URL credentials and explicit ports, blocks common local/internal hostnames, and rejects IP-literal hosts.
- Agent goals are wrapped with read-only research instructions that prohibit login, form submission, messaging, purchases and destructive actions.
- The Agent status route checks that the requested run belongs to the authenticated user before returning provider status.
- Provider requests have request timeouts, and returned Search/Fetch results are capped before being sent to the client.
- The Azure-side secret retrieval test succeeded without printing the credential value.

### Findings to track

1. **DNS-based SSRF protection is incomplete in the application URL helper.** The helper rejects IP-literal addresses and known local hostnames, but it does not resolve a hostname and verify every resolved address is public. Its own source comment describes it as defense-in-depth, not complete SSRF prevention. Because TinyFish performs the page retrieval, confirm that TinyFish enforces private-address and redirect protections on its side. If the application later fetches URLs directly, add robust DNS/IP and redirect validation.
2. **No per-user rate limit or usage quota was visible in the reviewed route snippets.** Search, Fetch and Agent calls can consume provider credits. Add server-side per-user limits, concurrency controls and usage logging before treating the API as production-ready.
3. **The read-only Agent prompt is a guardrail, not a hard security boundary.** Keep provider-side action restrictions enabled where available and do not grant the Agent authenticated sessions or credentials for arbitrary target sites.
4. **The separate Togetherly Workforce repository's `.gitignore` ignores `.env` but not all `.env.*` variants.** Add `.env.*` and an explicit `!.env.example` exception through a reviewed pull request. The platform repository already has the broader ignore pattern.
5. **Repository history and uploaded images were not exhaustively scanned.** Current tracked text files and documentation were inspected, but this review does not prove that no credential was ever committed or that every screenshot is free of personal identifiers.

## Residual risks and follow-up

- **Subscription-level Owner roles:** two inherited Owner assignments were observed during the V2 configuration work and were not changed. Review them separately in Azure IAM; this project did not attempt to change subscription ownership.
- **Public Key Vault endpoint:** Key Vault uses its public endpoint with network ACL restrictions and a VNet service endpoint. It does not use a Private Endpoint.
- **Administrator IP allow-list:** the configured address can become stale if the administrator's public IP changes. Keep it private and update only when necessary.
- **Secrets Officer role:** this is a powerful secret-management role. Keep the assignment limited to the authorised operator and review it periodically.
- **DoNotEnforce policy:** the tag policy evaluates compliance but does not block non-compliant resource operations.
- **Public VM web endpoint:** retain only required inbound web rules; SSH should remain limited to a trusted source. Do not expose API port 3001.
- **Credential rotation:** rotate the TinyFish credential if it is suspected to have been exposed. Removing it from the current `.env` file does not remove copies from backups or Git history.
- **Application authorization:** this Azure review does not re-audit Supabase RLS, employee/role authorization or production data access.
- **Environment-file ignore coverage:** the separate Togetherly Workforce repository does not yet ignore all `.env.*` variants. A direct change was blocked by branch protection; fix through a pull request and verify `.env.example` remains trackable.
- **Host hardening:** current SSH daemon settings, host firewall, package patch level, Nginx TLS configuration and file permissions were not independently inspected during this review.

## Release status and remaining checks

The Git tag `v2.0.0` and published GitHub release are now live. All 14 screenshot files are present in `evidence/v2/`; their full-size privacy/readability review and live operational checks remain open. The release does not claim that every screenshot has been independently reviewed.

Before treating the evidence review as closed:

- Inspect all 14 screenshots at full size and redact account emails, subscription/tenant identifiers, principal IDs, public IP addresses, secret values and tokens.
- Confirm the Key Vault networking screenshot shows the intended VNet/subnet rule and service endpoint without revealing the administrator IP.
- Confirm Search, Fetch and Agent evidence reflects the post-V2 application, not only the earlier V1 tests.
- Check current Azure VM state and Cost Management actuals/forecast. This documentation review did not access the Azure subscription or billing data.
- Recheck the repository for accidental secrets and sensitive image content.

## Conclusion

The V2 implementation evidence supports that Managed Identity, Key Vault secret retrieval, selected-network access, RBAC and tag-policy evaluation were configured and functionally tested. The controls reduce credential exposure and improve governance. Residual risks remain, and this review should not be described as a formal security certification.
