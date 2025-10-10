# Identity Providers (IDP) Configuration for OpenShift Prow CI

## Introduction

This guide outlines the procedures and scripts for configuring Identity Providers (IDP) within the OpenShift Prow CI ecosystem. Focused on automating IDP setups for Htpasswd, OpenLDAP, FreeIPA, and External OIDC providers, our goal is to streamline user authentication processes across CI/CD operations.

## Current Focus

We are automating the configuration of four primary IDP solutions:

- **Htpasswd and OpenLDAP**: Both are fully automated, providing seamless setup of user/password pairs.
- **FreeIPA**: Fully automated for user configuration.
- **External OIDC**: Fully automated with support for Entra ID (Microsoft Azure AD) and Keycloak providers.

## Available IDP Configurations

### Htpasswd

- **Automation Status**: Fully Automated
- **Purpose**: User setup via Htpasswd Identity.
- **Script URL**: [Htpasswd IDP Script](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/htpasswd/idp-htpasswd-ref.yaml)

### OpenLDAP

- **Automation Status**: Fully Automated
- **Purpose**: User setup via OpenLDAP Identity.
- **Script URL**: [OpenLDAP IDP Script](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/openldap/idp-openldap-ref.yaml)

### FreeIPA

- **Automation Status**: Fully Automated
- **Purpose**: User configuration through FreeIPA Identity.
- **Script URL**: [FreeIPA IDP Script](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/freeipa/idp-freeipa-ref.yaml)

### External OIDC

- **Automation Status**: Fully Automated
- **Purpose**: Configure external OIDC authentication for clusters using various OIDC providers.
- **Supported Providers**:
  - **Entra ID (Microsoft Azure AD)**: Full support including console app redirect URI management and user role assignment. Available workflows for AWS and Azure platforms.
    - **Chain**: [Entra ID Chain](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/entraid/idp-external-oidc-entraid-chain.yaml)
    - **Workflows**:
      - [AWS Workflow](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/entraid/aws/idp-external-oidc-entraid-aws-workflow.yaml)
      - [Azure Workflow](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/entraid/azure/idp-external-oidc-entraid-azure-workflow.yaml)
  - **Keycloak**: Sets up a Keycloak server including clients and test users.
    - **Chain**: [Keycloak Chain](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/keycloak/idp-external-oidc-keycloak-chain.yaml)
- **Base Step**: [External OIDC Script](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/idp-external-oidc-ref.yaml)
- **Additional Steps**:
  - **Grant User Role**: [Grant user role step](https://github.com/openshift/release/blob/master/ci-operator/step-registry/idp/external-oidc/grant-user-role/idp-external-oidc-grant-user-role-ref.yaml) - Grants ClusterRole permissions to external users

## Pre-requisites

Before configuring an IDP, ensure:

- A deployed OpenShift cluster.
- The `$USERS` environmental variable is not set and no other IDP configuration is set. The scripts check `$USERS` environment variable from runtime environment and check existing IDP configuration.

## Features

### Runtime Environment Checks and Configuration

To simplify test coverage for Auth IDPs, the scripts only configures one single IDP in a single Prow job.

### Password Management

Passwords for test users are dynamically generated.

### Additional Features

- **Environment Variable Export**: Generated user/password pairs are added to the shared runtime environment file to be exported as `$USERS` environment variable.

## Integration in CI Chains

### Htpasswd Identity Configuration

Included in the `openshift-e2e-test-qe*` chains by default, highlighting our commitment to secure user authentication in CI processes.

### OpenLDAP, FreeIPA, and External OIDC Identity Configuration

Configured for selected Prow jobs for selected CI profiles. To configure it in a new Prow job, add the `idp-openldap`, `idp-freeipa`, or external OIDC chain (e.g., `idp-external-oidc-entraid`, `idp-external-oidc-keycloak`) before executing the `openshift-e2e-test-qe*` chain, which will make the `idp-htpasswd` step not configure an htpasswd IDP any more.

## Future Directions & Contributions

We're extending support for user additions via IDP to Ginkgo test cases and expanding external OIDC provider support. Contributions and feedback are welcome to enhance our testing environment within OpenShift Prow CI.
