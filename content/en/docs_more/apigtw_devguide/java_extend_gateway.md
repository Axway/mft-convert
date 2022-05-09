{
"title": "Java interfaces for extending API Gateway",
"linkTitle": "Java interfaces for extending API Gateway",
"weight":"50",
"date": "2019-11-27",
"description": "Learn about the Java interfaces that can be used to extend API Gateway."
}

This section describes the following Java interfaces that can be used to extend API Gateway:

* `LoadableModule` – Classes that implement this interface are used to instantiate long-lived objects in the API Gateway process. These objects can be loaded at startup or when a new configuration is deployed, and can be unloaded at shutdown.
* `MessageCreationListener` – Classes that implement this interface are used to track message creation.
* `MessageListener` – Classes that implement this interface are used to to track the changes in a message as it flows through API Gateway.

## Create a loadable module

This section describes the `LoadableModule` interface, and provides an example of a loadable module class that implements the interface. Another example of a class that implements `LoadableModule` can be found in the `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule` directory.

The `LoadableModule` interface provides methods that are invoked during startup or shutdown of the API Gateway, or when a new configuration is deployed.

A `LoadableModule` class can be loaded at startup, or when a new configuration is deployed, and can be unloaded at shutdown. Loadable modules are used to instantiate long-lived objects in the API Gateway server process (for example, a transport listener, a cache manager, an embedded broker, and so on).

The loadable module object itself is informed when it is loaded, reconfigured, and unloaded. The order in which all loadable modules are loaded and configured is specified explicitly with a `loadorder` field in the entity type description associated with that loadable module. If your loadable module depends on another loadable module then it must have a load order which is higher than the module on which it depends.

The base `LoadableModule` interface has three methods. These are used on startup of the API Gateway, on shutdown of the API Gateway, or when a new configuration is deployed to the API Gateway. The following example shows the methods.

```java
public interface LoadableModule {

    ...
    /**
     * Load a module into the process.
     * @param parent Loadable modules may nest - @parent provides access to
     * the containing LM, or is null for a top-level module.
     * @param entityType - The actual type of the entity that caused this
     * class to be constructed.
     */
    public void load(LoadableModule parent, String entityType)  
            throws FatalException;
    /**
     * Unload the module from the process. This is called once
     * when the module is no longer required.
     */
    public void unload();

    /**
     * Configure the loadable module. Called if the entity for the
     * object changes.
     * Note that currently, modules are unloaded and reloaded for each
     * refresh - this behaviour should not be relied upon.
     */
    public void configure(ConfigContext pack, Entity object)
            throws EntityStoreException,FatalException;
}
```

A loadable module is normally designed as a singleton object so that only one instance exists. The same instance is returned to all filters accessing the loadable module. (For example, the `GlobalProperties` class is global and there is only one instance that is accessible to all parts of the application.)

Loadable module classes can be subclassed to provide extra information. For example, the `TransportModule` class provides settings to indicate what traffic information should be recorded for a specific protocol.

You can load a type definition for a `LoadableModule` class using the ES Explorer tool (see [Load a type definition](/docs/apigtw_devguide/entity_store#load-a-type-definition)). You can also view the `LoadableModule` classes in the ES Explorer (see [Use the ES Explorer](/docs/apigtw_devguide/entity_store#use-the-es-explorer)).

## `TimerLoadableModule` example

This section describes how to create a loadable module using a simple example. The `TimerLoadableModule` class creates a timer and traces a message to the trace output at a set interval.

### Create the TypeDoc definition for the loadable module

A TypeDoc is an XML file that contains entity type definitions. Entity type definitions describe the format of data associated with a configurable item.

All TypeDocs for `LoadableModule` classes must:

* Extend the `LoadableModule` type or one of its subtypes (such as `NamedLoadableModule`)
* Define a constant `LoadableModule` class
* Define the `loadorder` that indicates in what order the loadable module is loaded and configured
* List the configuration fields for the entity

The following definition lists the various fields that form the configuration data for the `TimerLoadableModule` class.

```xml
<entityType name="TimerLoadableModule" extends="NamedLoadableModule">
    <constant name="_version" type="integer" value="0"/>
    <constant name="class" type="string"
      value="com.vordel.example.TimerLoadableModule "/>
    <constant name="loadorder" type="integer" value="20"/>
    <field name="delaySecs" type="integer" cardinality="1" default="30"/>
    <field name="periodSec" type="integer" cardinality="1" default="10"/>
    <field name="textMessage" type="string" cardinality="1" default="Hello world"/>
</entityType>
```

In this definition:

* `delaySecs` – Delay in milliseconds before task is to be executed
* `periodSec` – Time in milliseconds between successive task executions
* `textMessage` – Message to be output to the trace file

### Create the loadable module implementation class

The API Gateway server-side implementation class is responsible for creating a timer and scheduling a task for repeated fixed-rate executions, beginning after a specified delay. Subsequent executions take place at regular intervals, separated by a specified period.

The following code shows the members and methods of the `TimerLoadableModule` class:

```java
public class TimerLoadableModule implements LoadableModule {

  Timer timer = null;
  int initialDelay = 30 * 1000;
  int period = 10 * 1000;
  String message = "Hello world";

  @Override
  public void configure(ConfigContext solutionPack, Entity entity)
    throws EntityStoreException {

      if (timer != null)
        timer.cancel();

      // load the configuration settings
      initialDelay = entity.getIntegerValue("delaySecs") * 1000;
      period = entity.getIntegerValue("periodSec") * 1000;
      message = entity.getStringValue("textMessage");
      TimerTask task = new TimerTask() {
          public void run() {
              Trace.error(message);
          }
      };
      timer.scheduleAtFixedRate(task, initialDelay, period);
  }

  @Override
  public void load(LoadableModule loadableModule, String arg1) {
      timer = new Timer();
  }

  @Override
  public void unload() {
      // clean up
      if (timer != null)
          timer.cancel();
  }
}
```

The `load` method creates a `Timer` instance. The `unload` method terminates the timer, discarding any currently scheduled tasks. It does not interfere with a currently executing task (if it exists). When the timer is terminated, its execution thread terminates gracefully, and no more tasks can be scheduled on it.

The `configure` method loads the configuration data and creates a new `TimerTask` that traces a message to the trace output and schedules this task to be executed at a repeated fixed rate, beginning after a delay, with subsequent executions to take place at regular intervals, separated by a specified period.

{{< alert title="Note" color="primary" >}}Currently, each loadable module is unloaded and recreated at reconfiguration time, so that the `configure` method is called only once for each loadable module. This behavior should not be relied upon.{{< /alert >}}

## Create a message creation listener

This section describes the `MessageCreationListener` interface, and provides an example of a message creation listener class that implements the interface. The sample code can be found in the `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule` directory.

### `MessageCreationListener` interface

The `MessageCreationListener` interface provides a method that is invoked when a message is created.

A `MessageCreationListener` class is used to track message creation. It is called when a message is created but before the originator of the message has populated any properties.

An example of its usage can be seen in the following `FilterInterceptor` class:

```java
public class FilterInterceptor implements LoadableModule,
  MessageCreationListener, MessageListener, FilterInterceptorMBean
{
    ...

    @Override
    public void load(LoadableModule parent, String typeName) {
        Message.addCreationListener(this);
        …
    }

    @Override
    public void unload() {
        Message.removeCreationListener(this);
        …
    }

    @Override
    public void messageCreated(Message msg, Object context) {
        msg.addMessageListener(this);
    }

    ...
}
```

The message creation listener is added when the loadable module is loaded, and removed when it is unloaded. In this example, it adds a message listener when a message is created.

## Create a message listener

This section describes the `MessageListener` interface, and provides an example of a message listener class that implements the interface. The sample code can be found in the `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule` directory.

### `MessageListener` interface

The `MessageListener` interface provides a set of callbacks that are invoked during the processing of a message as it passes through the processing engine of the API Gateway. The `MessageListener` interface provides callbacks which are invoked at certain points in the processing, for example just before a policy (circuit) is run, or before and after a message is processed by a filter. A message listener can be used to track the changes in a message as it flows through API Gateway, or to monitor the status of policies or filters as messages pass through them. Commonly it is used to gather statistics on message processing, which can then be used to give an indication of the status of API Gateway.

The `MessageListener` interface defines several methods that are invoked in conjunction with the methods or lifecycle events of the message. These include:

* Policy processing
    * `preCircuitProcessing` – Called when the message originator has completed initializing the message, and API Gateway is about to start processing in the policy-space.
    * `postCircuitProcessing` – Called when all processing in the policy-space is completed.
* Policy invocation
    *`preCircuitInvocation` – Called before the first filter in a given policy is invoked.
    *`postCircuitInvocation` – Called after a chain of filters in the policy has been invoked.
* Filter invocation
    * `preFilterInvocation` – This method is called immediately before a filter's `MessageProcessor` is invoked.
    * `postFilterInvocation` – This method is called when a filter's `MessageProcessor` has finished execution.
* `abortedCircuitInvocation` – Called if the policy exits because of a fault with one of the filters within it.
* `preFaultHandlerInvocation` – Called before attempting to handle a previous `CircuitAbortException` with specific fault-handling.
* `onMessageCompletion` – Called when a message has fully exited the system.

## `FilterInterceptor` example

This section describes how to create a message listener using a simple example. The `FilterInterceptor` class counts the number of messages which pass, fail, or abort during processing of a request in the API Gateway.

### Create the TypeDoc definition for the message listener

A TypeDoc is an XML file that contains entity type definitions. Entity type definitions describe the format of data associated with a configurable item.

In this example, the `FilterInterceptorLoadableModule` extends `NamedLoadableModule`.

The following definition lists the various fields that form the configuration data for the `FilterInterceptorLoadableModule` class and declares an instance of the type.

```xml
<entityStoreData>
  <entityType name="FilterInterceptorLoadableModule" extends="NamedLoadableModule">
    <constant name="class" type="string"
      value="com.vordel.interceptor.FilterInterceptor"/>
    <constant name="loadorder" type="integer" value="1000000"/>
  </entityType>
</entityStoreData>
```

```xml
<entityStoreData>
  <entity type="FilterInterceptorLoadableModule">
    <fval name="name">
      <value>Filter Invocation Callback Listener</value>
    </fval>
  </entity>
</entityStoreData>
```

```xml
<typeSet>
  <typedoc file="FilterInterceptorLoadableModule.xml"/>
  <typedoc file="instance.xml"/>
</typeSet>
```

To add the `FilterInterceptorLoadableModule` type to the primary entity store, you can use the `publish.py` script. For example:

```
cd INSTALL_DIR/apigateway/samples/scripts
./run.sh publish/publish.py -i DEVELOPER_SAMPLES/FilterInterceptorLoadableModule/conf/typedoc/typeSet.xml -t FilterInterceptorLoadableModule -g "QuickStart Group" -n "QuickStart Server"
```

Alternatively, you can use the ES Explorer to add the type.

### Create the message listener implementation class

The API Gateway server-side implementation class is responsible for monitoring message creation and lifecycle events. On each lifecycle event it writes messages to the trace output.

The following is an extract of the `FilterInterceptor` class that can be found in the `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule/src` directory.

```java
public class FilterInterceptor implements LoadableModule,
  MessageCreationListener, MessageListener, FilterInterceptorMBean
{
    …
    @Override
    public void load(LoadableModule parent, String typeName) {
        Message.addCreationListener(this);
        …
    }

    @Override
    public void unload() {
        Message.removeCreationListener(this);
        …
    }

    @Override
    public void messageCreated(Message msg, Object context) {
        msg.addMessageListener(this);
    }
    …

    @Override
    public void preCircuitInvocation(Circuit circuit, Message message,
      Object context)
    {
        Trace.info("Circuit ["+circuit.getName()+"] about to invoke message: "
          +message.correlationId+", caller context is: "+context);
    }

    @Override
    public void postCircuitInvocation(Circuit circuit, Message message,
      boolean result, Object obj)
    {
        Trace.info("Circuit ["+circuit.getName()+"] has finished with message: "
          +message.correlationId+", result is :"+(result?"PASSED": "FAILED"));
    }
    …

    @Override
    public void preFilterInvocation(Circuit circuit, MessageProcessor processor,
      Message message, MessageProcessor caller, Object obj)
    {
        Filter f = processor.getFilter();
        String type = f.getEntity().getType().getName();
        Trace.info("["+f.getName()+"("+type+")] msg:"+message.correlationId);
    }

    @Override
    public void postFilterInvocation(Circuit circuit, MessageProcessor processor,
      Message message, int resultType, MessageProcessor caller, Object obj)
    {
        Trace.info("["+processor.getFilter().getName()+"] msg:"
          +message.correlationId+" Result: "+toString(resultType));
    }

    @Override
    public void onMessageCompletion(Message message) {
        Trace.info("Message ["+message.correlationId+"] completed.");
    }
    …
}
```

A `MessageListener` is registered with a message instance by first listening for a message creation event via the `addCreationListener(MessageCreationListener)` method, which is called during the loading of the `FilterInterceptor`, and then calling the `addMessageListener(MessageListener)` method on the message parameter. The message creation listener is unregistered during the unloading of the `FilterInterceptor`.

The `onMessageCompletion` method monitors the completion of a message in a policy so that resources can be cleaned up when the message is no longer useful. The preprocessing and postprocessing interceptor methods output trace information.

The following is an example of the trace output from the `FilterInterceptor` during the execution of a filter:

```
INFO 26/Feb/2013:11:26:12.064 [1698] Circuit [Send Instant Message] about to invoke
  message: 8a8ef28f512c9bce01980000, caller context
  is: com.vordel.dwe.http.HTTPPlugin@2b3fab
INFO 26/Feb/2013:11:26:14.017 [1698] [Jabber(JabberFilter)]
  msg:8a8ef28f512c9bce01980000
INFO 26/Feb/2013:11:26:16.658 [1698] [Jabber]
  msg:8a8ef28f512c9bce01980000 Result: SUCCESS
INFO 26/Feb/2013:11:26:20.799 [1698] Circuit [Send Instant Message] has finished
  with message: 8a8ef28f512c9bce01980000, result is :PASSED
```

{{< alert title="Note" color="primary" >}}To see the above output, you must build the FilterInterceptorLoadableModule sample and add the resulting `interceptor.jar` to the API Gateway CLASSPATH. See the `README.TXT` for more information on building the sample.{{< /alert >}}

The following is a sample style sheet that can be used with the `removeType` script in the API Gateway to remove the `FilterInterceptorLoadableModule` and its instances from the primary entity store.

```xml
<?xml version="1.0" ?>
<stylesheet xmlns="http://www.w3.org/1999/XSL/Transform"
  version="1.0"
  xmlns:es="http://www.vordel.com/2005/06/24/entityStore">
    <template match="comment()|processing-instruction()">
      <copy />
    </template>
    <template match="@*|node()">
      <copy>
      <apply-templates select="@*|node()" />
      </copy>
    </template>
    <!-- Removing type and instances -->
    <template match=
      "/es:entityStoreData/es:entityType[@name='FilterInterceptorLoadableModule']"/>
    <template match=
      "/es:entityStoreData/es:entity[@type='FilterInterceptorLoadableModule']"/>
</stylesheet>
```

You can remove the type from the primary store by running the following command:

```
cd INSTALL_DIR/apigateway/samples/scripts
./run.sh unpublish/unpublish.py -i DEVELOPER_SAMPLES/FilterInterceptorLoadableModule/conf/remove.xslt -t FilterInterceptorLoadableModule -g "QuickStart Group" -n "QuickStart Server"
```

You can use the ES Explorer tool to view new types that were added, or to verify that types were removed. For more information, see [Use the ES Explorer](/docs/apigtw_devguide/entity_store#use-the-es-explorer).
