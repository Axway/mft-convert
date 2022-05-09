{
    "title": "Test your APIs",
    "linkTitle": "Test your APIs",
    "weight": "17",
    "date": "2020-04-23",
    "description": "Learn how to use tests to ensure that you have a clean API lifecycle."
}

## Why testing an API is important

Testing of APIs is essential, especially in the long run, to successfully manage the API lifecycle of an API. The goal of an API is to provide an interface that can be used by a range of consumers. To get the most value out of an API, it is important that as many consumers as possible use the API instead of having a separate version of the API for each consumer. Therefore, if a number of consumers use an API, it is important that this API remains stable, even if changes are made to the implementation or interface.

There are two types of changes; breaking and non-breaking changes. A breaking change forces a consumer to adapt their own implementation, which is not always feasible in practice because you do not have control over all applications. Therefore, an API should remain stable as long as possible and only non-breaking changes should be introduced.

Changes to an API are normal and intended to add more functionality, corrections, and so on, to the API. With each iteration the value of the API increases. But in order to be able to recognize in the long run whether a change is breaking or non-breaking, tests should be provided for each API from the beginning. For example, if an API is changed six months after the last change or introduction by another developer, the developer must be sure that the change is non-breaking.

## What it means to test an API

A REST API defines a set of endpoints/methods to be tested. In this case, the following should be tested:

* All endpoints
    * All parameters
    * Enforce all defined return codes (for example, with wrong parameters)
* The payload should be tested with schema validations
    * For example JSON schema
    * GET check the response
    * Send corresponding payload for POST, PUT, and so on
    * Payload content should not be tested

These tests are combined in a test suite and can be executed manually if required or as part of the pipeline. The example refers to REST APIs but the principles are also applicable to other interfaces and formats.

## Test tools

Amplify API Management does not offer dedicated testing tools, and this section provides recommendations only.

### Postman

Postman is an established and popular REST client that allows any form of REST interface to be addressed with various parameters and security formats.

In addition to simple requests, Postman enables you to perform the following:

* Define different tests per endpoint
* Define assertions, for example to check return codes or payload
* Run scripts for individual use cases
* Use environment variables to run tests on different stages
* The test suite can be exported, versioned, and executed as part of the CI/CD pipeline (see [Continuous API Testing with Postman](https://blog.postman.com/continuous-api-testing-with-postman/))

To learn more about how to test your APIs with Postman, see [Writing tests with Postman](https://learning.postman.com/docs/postman/scripts/test-scripts/).