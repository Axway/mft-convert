---
title: "SSO using SAML"
linkTitle: "SSO using SAML"
weight: 120
--- {{< TransferCFT/axwayvariablesComponentLongName  >}} supports a web browser single sign- on, SSO, profile that enables users to use the same logging details for {{< TransferCFT/axwayvariablesComponentLongName  >}} as other Axway products (for example, Flow Manager), eliminating the need to log in multiple times on different web- based UIs.

## Single sign- on using SAML

Single sign- on is a session/user authentication process that can be used to enhance interoperability. The SAML 2.0 standard describes how to exchange authentication and authorization data between entities.

### Security Assertion Markup Language

The Security Assertion Markup Language, SAML, is an XML- based solution for exchanging user security information (authentication, authorization) between an Identity Provider and a Service Provider. SAML is a product of the OASIS Security Services Technical Committee.

### Service Provider

A Service Provider, SP, protects access to requested resources, such as web sites and applications by applying a security policy. For example, the SP blocks all access to an unauthenticated user and routes the request to the Identity Provider. As such, {{< TransferCFT/axwayvariablesComponentLongName  >}} acts as an SP.

### Identity Provider

An Identity Provider, IdP, is a system that creates, maintains, and manages identity information for users, services, or systems, and provides authentication to other service providers (applications) within a network. An IdP is a trusted entity that users and servers can rely on when they are establishing a dialog that must be authenticated. The IdP sends an attribute assertion containing trusted information about the user to the SP. In an Axway deployment, the IdP is a third- party product.

### User agent

A user agent is usually a web browser. The person who uses the browser can be referred to as a user or as a principal.

## Limitations

- SAML is not presently available on OpenVMS platforms.
- You cannot log on Transfer CFT via CFTUTIL if you are using SAML (am.type=saml).

## Prerequisites

To configure and use SAML SSO with {{< TransferCFT/axwayvariablesComponentLongName  >}}, you must:

- Have a third- party IdP, such as Keycloak, installed and running.
- Map the user roles between the IdP and Transfer CFT roles ([CFTROLE](../conf_intro/cftrole)). To view the CFTROLES/CFTPRIV sample, click [here](), or navigate locally in your {{< TransferCFT/axwayvariablesComponentLongName >}} installation to:
    - distrib/template/conf/roles- smp.conf
    - runtime/conf/roles- smp.conf
- If you use {{< TransferCFT/suitevariablesTransferCFTName >}} with Flow Manager, you must manually set the uconf parameter am.type=saml on each {{< TransferCFT/suitevariablesTransferCFTName >}} after registering.
- If you use {{< TransferCFT/suitevariablesTransferCFTName >}} with {{< TransferCFT/suitevariablesCentralGovernanceName >}}, you must manually set the uconf parameter am.type=saml on each {{< TransferCFT/suitevariablesTransferCFTName >}} and import all {{< TransferCFT/suitevariablesTransferCFTName >}} roles and privileges (CFTROLE and CFTPRIV, respectively) after registering.

## Parameters

This section describes the UCONF parameter settings required for SAML implementation. To set SAML parameters during installation, refer to the settings as indicated in the initialize.properties file page that corresponds with your operating system. The examples below were based on a setup with Keycloak as the IdP.

## Set up SAML 

1. Configure the {{< TransferCFT/axwayvariablesComponentLongName >}} REST API server, as Transfer CFT UI relies on the REST API. See [Configure the REST API server](../../../app_integration_intro/using_apis/api_intro/api_configure).
1. Insert the IdP certificate, used to sign SAML messages, in the PKI database:  
    PKIUTIL PKICER id=idp, iname=&lt;path to the idp certificate>
1. Set the following UCONF parameters.
1. Define the roles that you require for your {{< TransferCFT/axwayvariablesComponentLongName >}} users. To view the CFTROLES sample, click [here](), and edit using your favorite text editor.
1. Start the Copilot server.
1. <span id="step6"></span>Export the SAML SP ({{< TransferCFT/axwayvariablesComponentLongName >}}) metadata.

- From a web browser, enter the URL https://[Transfer CFT host]:[REST API port]/saml2/metadata to extract the XML configuration data required to configure your IdP.
- Save the displayed XML content in a file.

Create your {{< TransferCFT/axwayvariablesComponentLongName  >}} client in the IdP by importing the saved XML file. Remember that you must create a client for each {{< TransferCFT/axwayvariablesComponentLongName  >}}.  
**Note**: If you are using Keycloak, set the **Front Channel Logout** option to **OFF**.

Set up the single logout.  
When the IdP connects to Transfer CFT using an HTTPS connection, it validates the Transfer CFT’s certificate to ensure it is connecting to a trusted server. This is necessary in order to prevent man- in- the- middle attacks.

- Put the Transfer CFT certificate, or the CA that signed the certificate, in the truststore used by the IdP.
- Refer the specific IdP documentation for more information. For Keycloak details, go to [www.keycloak.org/docs/latest/server_installation/index.html](https://www.keycloak.org/docs/latest/server_installation/index.html#_truststore).

## Test

From a web browser, enter the URL https://[Transfer CFT host]:[REST API port]/cft/ui - you are redirected to your IdP and prompted to enter your ID and credentials.

## Revoked user rights

If, as a {{< TransferCFT/axwayvariablesComponentLongName  >}} administrator or super user, you revoke the rights of a user who is currently logged in, this user can continue their session until they log out or the token expires, unless you expressly revoke all tokens for this user. This is true for both UI and REST API usage.

## Signed SAML headers

Access tokens and SAML headers are signed using the key associated with the certificates referenced by `copilot.ssl.sslcertfile`. If this parameter is not set, but {{< TransferCFT/PrimaryCGorUM  >}} or FM is enabled, the governance certificate is used. If you change the certificate that is referenced in `copilot.ssl.sslcertfile` or you modify the   `cg.certificate.governance.key_len `value, the impact is that the corresponding private key changes. This means that once the new key is generated, access tokens and SAML exchanges with the SAML IDP no longer work.

You can see the [Change the private key length](../../../governance_services_intro/cg_postregister#Change) section for details.

If you change the private key, you must repeat the steps 6 - 8 as described [above](#step6).

## Troubleshooting

****Error from IDP****

After performing the "Set up SAML" steps, sometimes the exported SAML SP (Transfer CFT) metadata is incorrect when imported into the IDP (for example Keycloak) generating an error from the IDP.

****Solution****: This issue seems to occur when using Firefox. We recommend that you check using another internet browser, such as Chrome.
