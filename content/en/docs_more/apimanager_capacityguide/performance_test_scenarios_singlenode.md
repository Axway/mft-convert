---
title: Performance test single node scenarios
linkTitle: Performance test single node scenarios
weight: "4"
date: 2019-09-26
description: Introduces the single node performance scenarios that were tested.
---

The API Management performance test scenarios that were run depend on the configuration. A full set of realistic customer scenarios covering both REST and SOAP cases for API Manager and API Gateway were run on the single node configuration. A more constrained set of the same scenarios were run on the multi-node HA configurations to test the scalability, see [Performance test Multi-node HA scenarios](/docs/apimanager_capacityguide/performance_test_scenarios_multinode).

The following test cases represent realistic customer scenarios and cover both REST and SOAP cases for API Manager and API Gateway.

## API Manager proxy scenarios

The following scenarios test the performance of API Manager as a proxy.

### API Manager proxy pass through

The purpose of this test is to assess the impact to transactions per second when invoking an unsecured API in API Manager.

A back-end REST service is virtualized in API Manager and exposed as an API with the inbound security profile set to **Pass Through**. The traffic generator invokes the API, and API Manager proxies the traffic to the back-end.

### API Manager proxy API key

The purpose of this test is to measure the performance of invoking a single API secured with an API Key and virtualized in API Manager.

The traffic generator invokes the API with the inbound security profile set to **API Key** in API Manager. API Manager validates the API Key and proxies the request to the back-end service.

### API Manager proxy HTTP Basic

The purpose of this test is to measure the performance of invoking a single API secured with an HTTP Basic authentication and virtualized in API Manager.

In this test, the authentication is based on an API Key and a secret key configured for the application in API Manager.

The traffic generator invokes the API with the inbound security profile set to **HTTP Basic** in API Manager. API Manager validates the API Key and proxies the request to the back-end service.

## API Manager OAuth token generation and validation scenarios

This scenario tests OAuth token generation and validation when the client application details are stored in the client application registry on API Manager. API Gateway is acting both as the authorization server and the resource server. APIs are configured to use the OAuth inbound security profile. In the token validation flow, a successful request is routed on to the back-end server like in a real-world scenario.

### API Manager OAuth client credentials

This test uses the OAuth client credentials grant type when invoking the APIs.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### API Manager OAuth resource owner

This test uses the OAuth resource owner grant type when invoking the APIs.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator the invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### API Manager OAuth JWT

This test uses the OAuth JWT grant type when invoking the APIs.

The traffic generator generates a JWT bearer token and sends the token in a request to API Gateway that acts as the authorization server. API Gateway exchanges the JWT bearer token for an OAuth token and returns the OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

## API Gateway OAuth token generation and validation scenarios

This scenario tests OAuth token validation when the client application details are stored in the client application registry on API Gateway. API Gateway is also acting both as the authorization server and the resource server. APIs are configured to use the OAuth inbound security profile. In token validation flow, a successful request is routed on to the back-end server like in a real-world scenario.

### API Gateway OAuth client credentials

This test uses the OAuth client credentials grant type when invoking the APIs.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Gateway, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### API Gateway OAuth resource owner

This test uses the OAuth resource owner grant type when invoking the APIs.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator the invokes the protected API in API Gateway, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### API Gateway OAuth JWT

This test uses the OAuth JWT grant type when invoking the APIs.

The traffic generator generates a JWT bearer token and sends the token in a request to API Gateway that acts as the authorization server. API Gateway exchanges the JWT bearer token for an OAuth token and returns the OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Gateway, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

## API Gateway SOAP scenarios

The following scenarios test the performance of API Gateway in SOAP requests.

### API Gateway SOAP pass through

The purpose of this test is to assess the impact to transactions per second when placing API Gateway between the traffic generator and the back-end service.

A simple policy uses a **Connect to URL** filter to route requests directly to the back-end service.

The traffic generator sends requests to API Gateway. API Gateway uses a **Connect to URL** filter to route requests directly to the back-end service and returns the received responses back to the traffic generator.

### API Gateway SOAP reflect

The purpose of this test is to measure the reliability and performance between the traffic generator and API Gateway.

The traffic generator sends requests to API Gateway, and API Gateway responds back to the traffic generator. No traffic goes to the back-end service.

### API Gateway SOAP WSDL virtualization

The purpose of this test is to measure the performance of sending traffic through a WSDL service virtualized in API Gateway.

In this scenario, a back-end WSDL web service is imported into API Gateway and deployed using the default configuration. An automatically generated policy (the Web Service filter) resolves the request, validates the schema of the request, and then routes the request to the back-end service.

The traffic generator sends requests to API Gateway. API Gateway forwards requests to the back-end service and returns the received responses back to the traffic generator. Incoming requests are parsed, validated, and schema-validated before being sent to the back-end service.

### API Gateway SOAP HTTP Basic

The purpose of this test is to measure the performance of authenticating a user against the local user store on API Gateway using a **HTTP Basic** filter.

The traffic generator invokes the authentication policy in API Gateway. After the authentication, API Gateway routes the request to the back-end service.

### API Gateway SOAP WSS10 user name token encryption

The purpose of this test is to measure the performance of authenticating a user against the local user store on API Gateway using a WS-Security user name.

The traffic generator invokes the policy in API Gateway. API Gateway validates the username token included in the request using the **WS-Security Username Token** filter, and encrypts the request using a symmetric key from the certificate store on API Gateway. Finally, API Gateway routes the request to the back-end service.

### API Gateway SOAP XSLT conversion

The purpose of this test is to measure the performance of message transformation using the **XSLT** filter.

The traffic generator invokes the XSLT stylesheet transformation policy in API Gateway. API Gateway removes the username token from the incoming payload and routes the request to the back-end service.
