# Security Baseline

**Status:** Project-level baseline. Controls must be checked against the live environment for each version. This document is not a formal audit or compliance attestation.

## Identity and access

- Apply least privilege to Azure identities and application permissions.
- Prefer Managed Identity over long-lived credentials for Azure workload access.
- Scope RBAC assignments to the smallest practical resource scope.
- Review inherited subscription-level roles separately; do not assume resource-level review covers them.
- Keep application authentication, employee status and role checks on the server side.

## Secrets

- Keep API keys and credentials out of Git, frontend bundles, logs and screenshots.
- Store the TinyFish API credential in Azure Key Vault for the V2 VM workload.
- Use the VM system-assigned Managed Identity to retrieve the secret.
- Do not print secret values. Log only safe error metadata.
- Rotate provider credentials if exposure is suspected; deleting a leaked value from the current branch does not remove it from Git history.

## Network

- Restrict inbound VM access to documented requirements.
- Keep SSH limited to trusted administrator source addresses.
- Expose only the required web ports to the public internet.
- Keep the TinyFish API bound to loopback behind Nginx; do not expose port 3001 publicly.
- Apply Key Vault network ACLs and review the administrator IP allow-list when the public IP changes.
- Distinguish a VNet service endpoint from a Private Endpoint. V2 uses the service endpoint and Key Vault public endpoint with network ACLs.

## Governance

- Use Azure Policy to evaluate required resource tags.
- Record the policy assignment enforcement mode accurately. V2's Project tag assignment uses DoNotEnforce and is not a deployment blocker.
- Treat policy compliance as one governance signal, not a complete security assessment.

## Application and data

- Keep Supabase service-role credentials out of browser code.
- Keep private evidence in private storage and use scoped access.
- Keep row-level security enabled and test access with separate role accounts.
- Do not modify production Supabase data, policies or authentication settings without separate approval.
- Use real employee data only in approved and protected environments.

## Evidence and operations

- Review screenshots and logs for secrets and personal information before committing.
- Record threat assumptions, residual risks and test evidence.
- Deallocate the VM when it is not needed for testing, and verify disk and public IP costs separately.
- Verify resource dependencies before deleting resources.
