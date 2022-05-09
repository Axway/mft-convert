{
"title": "Enable API Gateway with JMX",
"linkTitle": "Enable API Gateway with JMX",
"weight":"80",
"date": "2019-11-27",
"description": "Set up Java Management Extensions to allow remote clients to connect to a JVM and manage or monitor running applications."
}

Java Management Extensions (JMX) allows remote clients to connect to a JVM and manage or monitor running applications in that JVM. MBeans are the controllable endpoints of your application where remote clients can observe application activity as well as control their inner workings.

For more information on implementing an MBean interface, see the `FilterInterceptor` example. The sample code can be found in the `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule` directory. The `FilterInterceptor` class implements the `FilterInterceptorMBean` interface.

After you have set up your MBean, you must tell the JMX infrastructure about the MBean so that it can be published to clients. This involves creating a unique name for the MBean and registering it with the MBeanServer. For example:

```java
...
try {
    MBeanServer mbs = ManagementFactory.getPlatformMBeanServer();
    // Construct the ObjectName for the MBean we will register.
    ObjectName name = new ObjectName(
      "example:type=FilterInterceptorMBean");
    mbs.registerMBean(this, name);
} catch (Throwable t) {
    Trace.error(t);
}
...
```

After you have set up your MBeans and registered them with the MBeanServer you can view them in the management console that your JMX container supports (for example, JConsole). To use JConsole, add the following JVM argument to API Gateway and restart it.

Follow these steps:

1. Create a file called `jvm.xml` in the following location:

    ```
    INSTALL_DIR/apigateway/groups/GROUP_ID/INSTANCE_ID/conf
    ```

2. Edit the `jvm.xml` file so that the contents are as follows:

    ```
    <ConfigurationFragment>
        <VMArg name="-Dcom.sun.management.jmxremote"/>
    </ConfigurationFragment>
    ```

3. Restart API Gateway.

When you restart the API Gateway instance with the above settings you can connect using Java Monitoring and Management Console (`JConsole`) and view your MBeans. Launch `jconsole` (an executable in the `bin` directory of your Java JDK installation) and select the **MBeans** tab to see the **FilterInterceptorMBean** listed on the left. You can see the message total, success, failure, and abort counts.

{{< alert title="Note" color="primary" >}}In this case, the attributes are read-only, but in other cases they might be modifiable and you can change their settings.{{< /alert >}}
