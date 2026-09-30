---
title: "SSO Setup"
description: "Configure and acceptance-test enterprise SAML SSO for an AtlaSent organization; OIDC remains limited/early access."
---
{/* Generated from the Atlasent docs source (site/guides/sso-setup.md). Do not edit here: this file is overwritten on every publish. */}

<Note>
  **Current status.** AtlaSent has an implemented **SAML 2.0 enterprise integration surface** in the Console/backend. A specific organization must still configure and acceptance-test its identity provider before relying on SSO enforcement. **OIDC remains early access / limited support.** Availability and support terms are deployment and contract specific; this page is not a universal SLA or GA claim.
</Note>

SSO establishes **identity and access context**. It does not by itself grant
organizational Authority for a consequential Action. AtlaSent evaluates that
Authority separately at execution time.

## Before you enable enforcement

- You must be an **org admin** in AtlaSent.
- Your identity provider must support the integration method you are configuring.
- Confirm the feature is enabled for the target enterprise organization.
- Preserve an admin recovery path until at least one administrator has completed a successful end-to-end SSO login.
- Treat production enablement as an acceptance event: configuration existing in the Console is not the same as a validated customer login path.

## SAML 2.0

SAML 2.0 is the primary enterprise SSO path documented for AtlaSent.

### 1. Open SSO settings

In the AtlaSent Console, navigate to the organization's **SSO / Security**
configuration surface and choose SAML configuration.

### 2. Use the service-provider metadata shown by AtlaSent

The Console/backend exposes the service-provider metadata required by the
customer's IdP, including the Entity ID / audience, ACS / reply URL, and NameID
requirements.

Do not copy endpoint values from an old screenshot or document. Use the
metadata generated for your current AtlaSent deployment and configuration.

### 3. Configure the identity provider

In Okta, Microsoft Entra ID, Google Workspace, or another SAML-compatible IdP:

1. Create the AtlaSent SAML application.
2. Enter the current AtlaSent Entity ID and ACS URL.
3. Configure email / NameID as the primary identity mapping.
4. Add only the attributes required for the customer's approved role-mapping design.
5. Export or copy the IdP metadata/certificate needed by AtlaSent.

### 4. Save the IdP configuration in AtlaSent

Enter the IdP metadata (or equivalent issuer/SSO URL/certificate fields) in the
AtlaSent SSO configuration surface. The configuration should be validated by the
backend before activation.

### 5. Acceptance-test login

Test with at least one admin account and confirm:

- the expected IdP is used;
- the user resolves to the intended AtlaSent organization;
- email / identity mapping is correct;
- the expected role/access mapping is applied;
- failed or malformed assertions are rejected;
- an administrator retains a recovery path.

<Warning>
  Do **not** enforce SSO for the organization until the configured IdP login path and an admin recovery path have been proven. A saved configuration is not evidence that enforcement is safe to enable.
</Warning>

### 6. Enable enforcement only after acceptance

When the customer and operator have accepted the SSO path, enable the applicable
organization-level enforcement control. Record the acceptance evidence and the
configuration/issuer used for the test.

## Okta SAML

A typical Okta setup is:

1. **Applications → Create App Integration → SAML 2.0**.
2. Enter the Entity ID / Audience and ACS / Single Sign-On URL supplied by AtlaSent.
3. Use email / NameID as the principal identity mapping.
4. Add approved attribute statements if the deployment requires them.
5. Export Okta's IdP metadata.
6. Save the IdP metadata in AtlaSent.
7. Test a real login before enabling enforcement.

The exact Okta UI labels can change. The AtlaSent-generated metadata and the
customer's tested assertion are authoritative for the deployment.

## Microsoft Entra ID SAML

A typical Entra setup is:

1. Create or open the AtlaSent Enterprise Application.
2. Configure **Single sign-on → SAML**.
3. Enter the Entity ID and Reply/ACS URL supplied by AtlaSent.
4. Map the customer's approved email / NameID claim.
5. Export the Federation Metadata XML.
6. Save that metadata in AtlaSent.
7. Test an assigned user and admin recovery before enforcement.

## Google Workspace SAML

A typical Google Workspace setup is:

1. Create a custom SAML application in the Admin Console.
2. Use the AtlaSent-generated ACS URL and Entity ID.
3. Configure email as NameID unless the approved deployment design says otherwise.
4. Export the Google IdP metadata and save it in AtlaSent.
5. Test the real login path before enforcement.

## OIDC

<Warning>
  **OIDC is early access with limited support.** It is not enabled by default. Contact AtlaSent to confirm that OIDC is enabled for your deployment before you plan a rollout that depends on it.
</Warning>

For an approved OIDC deployment, use the Console's current configuration surface
and the IdP's discovery/issuer metadata. Record the issuer, client configuration,
redirect URI, test evidence, and operator/customer acceptance.

## SCIM 2.0 provisioning

SCIM is no longer accurately described as “not implemented.” AtlaSent has an
implemented enterprise SCIM provisioning surface. It remains separate from SSO:

- **SSO** authenticates users / establishes identity context.
- **SCIM** provisions and deprovisions directory objects.
- neither one, by itself, creates organizational Authority to execute a consequential Action.

See [SCIM 2.0 Provisioning](/enterprise/scim-admin) for setup and acceptance
requirements.

## Identity versus organizational authority

An authenticated user may have technical access without organizational Authority
for a particular action. The execution-time sequence remains:

`Identity / Assertions → Authority → Policy → Evaluation → Permit → Verification → Execution → Evidence`

SSO, SCIM, role membership, and Approval are inputs/facts; they do not replace
the per-request Authorization determination.

## Troubleshooting / support

For a failed enterprise identity test, preserve:

- organization ID;
- IdP/provider and protocol;
- configuration version / issuer;
- request/correlation identifier where available;
- timestamp and environment;
- redacted error detail (never send secrets or raw private keys).

Contact [support@atlasent.io](mailto:support@atlasent.io) with the evidence above.
