{
"title": "Certifications and compliance",
"linkTitle": "Certifications and compliance",
"weight": 20,
"date": "2019-11-25",
"description": "Certifications API management products have received, and standards they comply with."
}

## Common Criteria certification

API Gateway 7.4.1 with Service Pack 2 has received Common Criteria certification for the following Protection Profiles:

* Enterprise Security Management Access Control v2.1
* Enterprise Security Management Policy Management v2.1

This certification is valid on the following platforms:

* Windows
* OEL Linux
* Appliance (SUSE Linux Enterprise Server)

Certification number: 10778

Date of certification: 13 January 2017

For more details, see [National Information Assurance Partnership (NIAP) web page](https://www.niap-ccevs.org/Product/Compliant.cfm?pid=10778).

## FIPS-140-2 compliance

The current version of API Gateway is Federal Information Processing Standards (FIPS) 140-2 compliant when running in FIPS mode on the following platforms:

* OEL Linux

When operating in FIPS mode, the following FIPS-certified cryptographic modules are enabled and invoked for all FIPS-compliant cryptographic algorithms:

| Cryptographic module                                          | FIPS 140-2 certificate number |
|---------------------------------------------------------------|-------------------------------|
| Entrust Authority Security Toolkit for the Java Platform v8.0 | 1839                          |
| OpenSSL FIPS Object Module                                    | 1747                          |

The ability to run API Gateway in FIPS mode is a licensable option of the standard version and must be specifically ordered.  It is possible to enable or disable FIPS mode as required. For more information on running in FIPS mode, see
[Run API Gateway in FIPS mode](/docs/apim_administration/apigtw_admin/admin_fips/).

You can run the FIPS Compliance Validation Tool from Policy Studio to check that a configuration is FIPS compliant. This tool is available from the **Tools > Check Security Constraints > FIPS** menu option.

For more information on running the tool, see [Compliance validation tools](/docs/apim_policydev/apigw_poldev/general_validation_tools/). For guidance on compliant settings for Policy Studio filters, see [Compliant settings for filters](/docs/apimgmt_security/compliance_appendix/).

### API Portal FIPS compliance

API Portal is not FIPS-140-2 compliant.

## NIST Suite B compliance

You can run the National Institute of Standards and Technology (NIST) Suite B and Suite B Top Secret Compliance Validation Tools from Policy Studio to ensure that a configuration is Suite B or Suite B Top Secret compliant. These tools are available from the **Tools > Check Security Constraints > SuiteB** and the **Tools > Check Security Constraints > SuiteBTS** menu options.

### API Portal NIST compliance

API Portal is not NIST SUITE-B compliant.

## SAML 2.0 compliance

API Gateway and API Manager support all of the mandatory features of the SAML 2.0 standard:

* Web SSO, AuthRequest, HTTP redirect
* Web SSO, Response, HTTP POST
* Web SSO, Response, HTTP artifact
* Artifact Resolution, SOAP
* Enhanced Client/Proxy SSO, PAOS
* Single Logout (IdP initiated), HTTP redirect
* Single Logout (SP initiated), HTTP redirect
