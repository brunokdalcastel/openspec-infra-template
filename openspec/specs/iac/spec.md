# Infrastructure-as-Code Specification

## Requirement: Terraform Standards
The system SHALL follow consistent Terraform practices.

### Scenario: Module structure
- GIVEN a new infrastructure pattern
- WHEN a module is created
- THEN follows standard structure (main.tf, variables.tf, outputs.tf, README.md)
- AND all variables have descriptions and type constraints
- AND module is versioned with semver

### Scenario: State management
- GIVEN a Terraform project
- WHEN state is managed
- THEN remote backend is used (Azure Storage, S3, etc.)
- AND state locking enabled
- AND state NEVER committed to Git
- AND state access restricted to authorized users/pipelines

### Scenario: Import existing resources
- GIVEN unmanaged existing infrastructure
- WHEN brought under IaC
- THEN `terraform import` captures current state
- AND HCL written to match (plan shows zero drift)
- AND the import is documented

## Requirement: Code Quality
The system SHALL enforce IaC quality gates.

### Scenario: Pre-commit
- GIVEN a Terraform change
- WHEN committed
- THEN `terraform fmt` applied
- AND `terraform validate` passes
- AND linter (tflint) clean
- AND security scan (tfsec/checkov) passes

### Scenario: Plan review in PR
- GIVEN a pull request with infra changes
- WHEN PR is created
- THEN `terraform plan` output posted as comment
- AND destructive changes highlighted
- AND reviewer sees exact diff

## Requirement: Ansible Standards
The system SHALL follow consistent Ansible practices.

### Scenario: Playbook quality
- GIVEN a configuration management need
- WHEN playbook is created
- THEN it is idempotent
- AND uses roles for reusability
- AND sensitive vars use ansible-vault
- AND tested in staging first

## Requirement: GitOps Workflow
The system SHALL follow GitOps for deployments.

### Scenario: Deploy flow
- GIVEN an approved infra change (merged PR)
- WHEN pipeline triggers
- THEN plan runs automatically
- AND production apply requires manual approval gate
- AND deployment recorded with audit trail
