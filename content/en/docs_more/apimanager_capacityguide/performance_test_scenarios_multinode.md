---
title: Performance test multi-node HA scenarios
linkTitle: Performance test multi-node HA scenarios
weight: "5"
date: 2019-09-26
description:  Introduces the multi-node performance scenarios that were tested.
---
Multi-node HA tests concentrate on the performance of API Manager. The following, more constrained set of scenarios is used to test the scalability.

## API Manager proxy scenarios

The following scenarios test the performance of API Manager as a proxy in both multi-node configurations. The maximum number of nodes in all scenarios is six.

### API Manager proxy – pass through

The purpose of this test is to assess the impact to transactions per second when invoking an unsecured API in API Manager.

A back-end REST service is virtualized in API Manager and exposed as an API with the inbound security profile set to **Pass Through**. The traffic generator invokes the API, and API Manager proxies the traffic to the back-end.

### API Manager proxy – API key

The purpose of this test is to measure the performance of invoking a single API secured with an API Key and virtualized in API Manager.

The traffic generator invokes the API with the inbound security profile set to **API Key** in API Manager. API Manager validates the API Key and proxies the request to the back-end service.

## API Manager OAuth token generation and validation scenarios

These scenarios tests OAuth token validation when the client application details are stored in the client application registry on API Manager. API Gateway is acting both as the authorization server and the resource server. APIs are configured to use the OAuth inbound security profile. In the token validation flow, a successful request is routed on to the back-end server like in a real-world scenario.

### Client credentials

This test uses the OAuth client credentials grant type when invoking the APIs. Each generated token is reused 50 times.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### Resource owner

This test uses the OAuth resource owner grant type when invoking the APIs. Each generated token is reused 50 times.

The traffic generator requests an OAuth token from API Gateway, and API Gateway returns the generated OAuth token to the traffic generator. The traffic generator the invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.

### JWT

This test uses the OAuth JWT grant type when invoking the APIs. Each generated token is reused 50 times.

The traffic generator generates a JWT bearer token and sends the token in a request to API Gateway that acts as the authorization server. API Gateway exchanges the JWT bearer token for an OAuth token and returns the OAuth token to the traffic generator. The traffic generator then invokes the protected API in API Manager, the request header containing the OAuth Token. API Gateway validates the OAuth token and routes the request to the back-end service.
