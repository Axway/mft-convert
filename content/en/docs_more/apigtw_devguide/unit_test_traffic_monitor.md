{
"title": "Test and debug a custom filter",
"linkTitle": "Test and debug a custom filter",
"weight":"40",
"date": "2019-11-27",
"description": "Create JUnit tests, debug custom Java code, and get detailed diagnostic information for a custom filter."
}

## Create JUnit tests to test a custom filter using the Traffic Monitor API

The following example shows you how to create JUnit tests to test a custom filter using the Traffic Monitor API. Any JUnit test classes you write should extend and use the existing test classes that are shipped with API Gateway, as these test classes provide several assertions that are used to evaluate the responses returned.

The two main classes to use are:

* `TestClientResponse` – Uses Jersey client APIs to send GET and POST requests to API Gateway. The JAR files can be found in the `INSTALL_DIR/apigateway/system/lib/modules` directory.
* `TrafficMonitorClient` – Used to invoke the Traffic Monitor REST API, which monitors the traffic in and out of the API Gateway, and evaluate the responses returned. These classes are contained in `testClient.jar`, which can be found in the `INSTALL_DIR/apigateway/system/lib/` directory.

{{< alert title="Note" color="primary" >}}A Node Manager and an API Gateway instance must be running before the JUnit tests can be run.{{< /alert >}}

### Write a JUnit test for the Health Check policy filters

Perform the following steps to write a JUnit test for the Health Check policy filters:

1. Create a test class called `TestHealthCheck`. It should extend the `TestClientResponse` utility class, which contains several assertion methods that can be used to test the client responses returned from a web resource. For example:

    ```java
    import com.vordel.ops.TestClientResponse;

    public class TestHealthCheck extends TestClientResponse {
    ...
    }
    ```

2. Within the `setup` method, create a new instance of a `com.vordel.ops.TrafficMonitorClient`. This client contains several assertion methods that can be used to evaluate the response based on the traffic information in and out of the API Gateway, and the CorrelationId.

    ```java
    @BeforeClass
    public static void setup() throws NodeManagerAPIException {
      client = new TrafficMonitorClient("https", "localhost", "8090", SERVER_ID, USERNAME, PASSWORD);
    }
    ```

3. Create a test case that invokes a request and evaluates the response returned using the `TrafficMonitorClient`. Each filter of the policy can be evaluated to determine if it passed or failed.

    ```java
    import javax.ws.rs.core.Response;

    @Test
    public void testHealthCheck() {

        // Execute health check policy
        Response response = get("http://localhost:8080/healthcheck");
        assertStatusCode(response, 200);

        // Get its correlation id.
        String correlationId = getCorrelationId(response);
        // check HTTP header
        assertContainsHeader(response, "Host");

        // check HTTP header and value
        assertContainsHeaderWithValue(response, "Host", "localhost:8080");
        // Check that Set Message and Reflect Filters pass.
        client.assertFilterPassed(correlationId, "Set Message", "Reflect");

        // Ensure fault handlers did not fire.
        client.assertFilterOfTypeDidNotExecute(correlationId, "GenericError");
        client.assertFilterOfTypeDidNotExecute(correlationId, "JSONError");
        client.assertFilterOfTypeDidNotExecute(correlationId, "SOAPFault");

        client.assertNFiltersPassed(correlationId, 2);
        client.assertNFiltersFailed(correlationId, 0);
    }
    ```

For more information on the client assertion methods, go to [API Gateway Javadoc](https://support.axway.com/htmldoc/1444954).

## Debug custom Java code with a Java debugger

You can debug custom Java code running in API Gateway (for example, code for a custom filter), by attaching a remote debugger to API Gateway. To attach a remote debugger, add a JVM argument to API Gateway and restart it.

To change the JVM settings of an API Gateway instance, follow these steps:

1. Create a file called `jvm.xml` in the following location:

    ```
    INSTALL_DIR/apigateway/groups/GROUP_ID/INSTANCE_ID/conf
    ```

2. Edit the `jvm.xml` file so that the contents are as follows:

    ```xml
    <ConfigurationFragment>
      <VMArg name="-Xrunjdwp:transport=dt_socket,server=y,address=9999" />
    </ConfigurationFragment>
    ```

3. Restart API Gateway.

When you restart the API Gateway instance with the above settings, it starts up and waits for a JVM debugger to connect to the process on port 9999. You can connect to port 9999 of the API Gateway instance using a Remote Java debug (for example, in the Eclipse IDE).

## Get diagnostics output from a custom filter

You can configure API Gateway to output detailed diagnostic information for a specific custom filter by setting the trace level to DEBUG or DATA.

To change the trace level in Policy Studio, select the **Server Settings** node, and click **General**. Select DEBUG or DATA from the **Tracing level** field, and click **Save**.

For more information on tracing and logging see [Configure API Gateway trace files](/docs/apim_administration/apigtw_admin/tracing/#configure-api-gateway-trace-files).

### Add custom trace output to custom code

{{< alert title="Note" color="primary" >}}Some code extracts in this section are from the `jabber` sample that is no longer included in the code samples supplied with API Gateway. {{< /alert >}}

To add custom trace information to custom code, you can add `Trace` statements within your code.

For example, the following code adds `Trace` statements to output the thread ID associated with the chat, which corresponds to the thread field of the SMACK XMPP message to a custom Jabber filter (see [Write a custom filter using the extension kit](/docs/apigtw_devguide/custom_filter_extension_kit/)).

```java
...
import com.vordel.trace.Trace;
...

public class JabberProcessor extends MessageProcessor {
    ...
    public boolean invoke(Circuit c, Message message)
      throws CircuitAbortException {
        ...
        try {
            Trace.debug("Chat Thread ID is " + chat.getThreadID());
            chat.sendMessage(messageStr.substitute(message));
        } catch (org.jivesoftware.smack.XMPPException ex) {
            Trace.error("Error Delivering block");
         }
        ...
    }
    ...
}
```

The Chat Thread ID is output in the API Gateway trace file as follows:

```
DEBUG   10/Jun/2013:11:18:21.365 [01f4] run filter [Jabber] {
DEBUG   10/Jun/2013:11:18:22.880 [01f4] Chat Thread ID is VSx1B0
DEBUG   10/Jun/2013:11:18:23.037 [01f4] } = 1, filter [Jabber]
DEBUG   10/Jun/2013:11:18:23.037 [01f4] Filter [Jabber] completes in 672 milliseconds.
```

The trace level DATA can be used to provide more detailed information. To use the DATA level in the preceding example, change the `Trace.debug` statements to `Trace.data` statements.

### Add custom log4j output to custom code

To output custom log4j information perform the following steps:

1. Update the `log4j2.xml` file, located in the `INSTALL_DIR/apigateway/system/conf` directory, to specify that the log4j appender sends output to the API Gateway trace file. For example:

    ```xml
    <Root level="debug">
    <AppenderRef ref="STDOUT" />
    <AppenderRef ref="VordelTrace" />
    </Root>
    ```

2. Add log4j statements to your code. Log4j is already on the API Gateway CLASSPATH. The following example shows the preceding code with log4j statements instead of Trace statements:

    ```java
    ...
    import org.apache.log4j.Logger;
    ...
    public class JabberProcessor extends MessageProcessor {
        private static final Logger log = LogManager.getLogger(JabberProcessor.class.getName());  
        ...
        public boolean invoke(Circuit c, Message message)
          throws CircuitAbortException {
            ...
            try {
                log.debug("Chat Thread ID is " + chat.getThreadID());
                chat.sendMessage(messageStr.substitute(message));
            } catch (org.jivesoftware.smack.XMPPException ex) {
                Trace.error("Error Delivering block");
             }
            ...
        }
        ...
    }
    ```

The following is output in the API Gateway trace file:

```
<Date> <Time> [Thread-xx] DEBUG com.vordel.jabber.filter.JabberProcessor - Chat Thread ID is xxxxxx
```
