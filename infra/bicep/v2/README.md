# Azure Bicep — Reserved Infrastructure-as-Code Work

The deployed V2 environment was configured manually through the Azure Portal. **No Bicep template was used to deploy the V2 resources.**

This folder is reserved for a future Infrastructure-as-Code milestone. The [V1/V2 Infrastructure Preservation Pack](../../../docs/preservation/v2-infrastructure-preservation-pack.md) records the current manual configuration and rebuild requirements. The project roadmap introduces Bicep after the manual Portal work has been documented and understood. Do not treat the existence of this folder as evidence of an IaC deployment.

Possible future contents:

- `main.bicep`
- `modules/`
- `parameters/` with non-secret example parameter files
- Deployment, validation and rollback instructions

No deployable V1/V2 Bicep templates exist yet. Do not treat the preservation pack or this README as executable infrastructure. Before implementing templates, review resource dependencies, Azure Policy behaviour, naming, region, network access and cost. Never commit credentials or secret parameter values. Terraform is not part of this project.
