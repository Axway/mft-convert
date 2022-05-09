{
"title": "Add a custom filter to API Gateway",
"linkTitle": "Add a custom filter",
"weight":"20",
"date": "2019-11-27",
"description": "Extend the capability of API Gateway by adding a custom filter."
}

You can extend the capability of API Gateway by adding a custom filter. There are several options for adding a custom filter:

* Write your custom requirement in Java and invoke it using the **Scripting Language** filter. You can use this approach to develop your business logic in a standard IDE and debug and test it in standalone mode before integrating with API Gateway. See [Use JavaScript to call existing Java code](#use-javascript-to-call-existing-java-code).
* Write your custom requirement using the **Scripting Language** filter alone. See [Use JavaScript for custom requirements](#use-javascript-for-custom-requirements).
* Write your custom filter using the API Gateway developer extension kit. Using this approach, a fully integrated filter is created that has the API Gateway runtime capability and appears in the filter palette in Policy Studio. See [Write a custom filter using the extension kit](/docs/apigtw_devguide/custom_filter_extension_kit/).

The following summarizes the different approaches for adding a custom filter:

| Scripting                                                                                          | Writing a Java Filter  |
|----------------------------------------------------------------------------------------------------|------------------------|
| Quick way to reuse some functionality exposed in Java                                              | Enterprise integration   |
| No major development skills required                                                               | Development skills required   |
| Does not appear in filter palette in Policy Studio                                                 | Filter appears in filter palette in Policy Studio   |

Before creating your own custom filter you should understand exactly what message filters are, and how they are chained together to create a message policy. These concepts are documented in detail in the [API Gateway Policy Developer Guide](/docs/apim_policydev/apigw_poldev/).

## Use JavaScript to call existing Java code

Write your custom requirement in Java and invoke it using JavaScript in a **Scripting Language** filter.

Follow these guidelines:

1. Create a Java class that meets your custom requirement.
2. Build a JAR file from the Java class and add it, and any third-party dependencies, to the API Gateway CLASSPATH and to the runtime dependencies in Policy Studio.
3. Create a policy (for example, called **InvokeJava**) in Policy Studio that contains only a **Scripting Language** filter. Configure the filter to invoke the Java code using JavaScript.

    We recommend that you select `JavaScript` in the **Language** field of the **Scripting Language** filter, and ensure that the JavaScript syntax in the script conforms with Nashorn engine syntax. For more information about migrating from Rhino to Nashorn, see the [Rhino Migration Guide](https://wiki.openjdk.java.net/display/Nashorn/Rhino+Migration+Guide).

4. Configure API Gateway to invoke the policy.
5. Test the policy using API Tester.

## Use JavaScript for custom requirements

Write your custom requirement using the **Scripting Language** filter alone.

Follow these guidelines:

1. Create a policy (for example, called **InvokeScript**) in Policy Studio that contains only a **Scripting Language** filter. Configure the filter to invoke JavaScript that meets your custom requirement.

    We recommend that you select `JavaScript` in the **Language** field of the **Scripting Language** filter, and ensure that the JavaScript syntax in the script conforms with Nashorn engine syntax. For more information about migrating from Rhino to Nashorn, see the [Rhino Migration Guide](https://wiki.openjdk.java.net/display/Nashorn/Rhino+Migration+Guide).

2. Configure API Gateway to invoke the policy.
3. Test the policy using API Tester.

## Invoke the policy

To configure the API Gateway to invoke the new policy for a Java Code, follow these steps:

1. Under the **Environment Configuration > Listeners** node in Policy Studio, select the path (for example, **API Gateway > Default Services > Paths**).
2. On the resolvers window on the right, click **Add > Relative Path**.
3. Enter the following values on the dialog and click **OK**:
    * **When a request arrives that matches the path:** `/invokejava`
    * **Path Specific Policy:** Click the browse button and select the **InvokeJava** policy. This sends all requests received on the path configured above to your newly configured policy.

    Replace `/invokejava` with `/invokescript`, and  **InvokeJava** policy with **InvokeScript** policy if using with JavaScript.
4. To deploy the new configuration to API Gateway, click the **Deploy** button on the toolbar or press **F6** and follow the instructions.

## Test the policy

To test the configuration, follow these steps:

1. Start API Tester.
2. Click the arrow next to the Play icon and select **Request Settings**.
3. In the **Url** field, enter `http://localhost:8080/invokejava` to send the message to the relative path you configured above.
4. Click **Run** to send the message to API Gateway.

Alternatively, you can test the policy by entering the URL `http://localhost:8080/invokejava` into any web browser.
