# GitHub Actions Workflows

This directory is reserved for repository automation such as documentation checks, tests, security scans, and approved deployment workflows.

No workflow implementation is currently configured here. Add workflow YAML only when the checks are defined, permissions are minimised, and the workflow has been tested.

V1 Azure resource provisioning remains manual through the Azure Portal. If later automation needs Azure access, prefer a reviewed OIDC design rather than long-lived credentials; the identity approach is planned for V2.
