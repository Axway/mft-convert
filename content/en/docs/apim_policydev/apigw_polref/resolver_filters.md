{
"title": "Resolver filters",
  "linkTitle": "Resolver filters",
  "weight": 98,
  "date": "2019-10-17",
  "description": "Identify incoming XML messages based on SOAP action, SOAP operation name, or relative path."
}

All resolver filters use the regular expression syntax specified by `java.util.regex.Pattern`. For more details, see [Class Pattern](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).

## SOAP action resolver filter

The **SOAP Action Resolver**
filter enables you to identify an incoming XML message based on the `SOAPAction`
HTTP header in the message. The **SOAP Action Resolver**
filter applies to SOAP 1.1 and SOAP 1.2.

The following example illustrates how to locate the `SOAPAction`
header in an incoming message. Consider the following SOAP message:

```
POST /services/helloService
HTTP/1.1Host:localhost:8095
Content-Length:196
SOAPAction:HelloService
Accept-Language:en-US
UserAgent:API Gateway
Content-Type:text/XML; utf-8
```

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Header>
   </soap:Header>
   <soap:Body>
      <getHello xmlns="http://www.axway/>.com/"/>
   </soap:Body>
</soap:Envelope>
```

The SOAP Action for this message is `HelloService`.

To configure the **SOAP Action Resolver**
filter:

1. Enter an appropriate name for the filter to display in policy in the **Name**
    field.
2. Enter a regular expression to match the value of the `SOAPAction`
    HTTP header in the **SOAP Action**
    field.
3. For example, enter `^getQuote$`
    to exactly match a `SOAPAction`
    header with a value of `getQuote`. Incoming messages with a matching `SOAPAction`
    value are passed on to the next filter on the success path in the policy.

## SOAP operation name resolver filter

The **Operation Name**
filter enables you to identify an incoming XML message based on the SOAP operation in the message.

The following example shows how to find the SOAP operation of an incoming message. Consider the following SOAP message:

```
POST /services/timeservice
HTTP/1.0Host:localhost:8095
Content-Length:374
SOAPAction:TimeService
Accept-Language:en-US
UserAgent:API Gateway
Content-Type:text/XML; utf-8
```

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
      <ns1:getTime xmlns:ns1="Some-URI">
         <ns1:city>Dublin</ns1:city>
      </ns1:getTime>
   </soap:Body>
</soap:Envelope>
```

The SOAP operation for this message and its namespace are as follows:

* SOAP operation:`getTime`
* SOAP operation namespace:`urn:timeservice`

The SOAP operation is the first child element of the SOAP `<soap:Body>`
element.

To configure the **Operation Name**
filter:

1. Enter an appropriate name for the filter to display in a policy in the **Name**
    field.
2. Enter the name of the SOAP operation in the **Operation**
    field. Incoming messages with an operation name matching the value entered here are passed on to the next success filter in the policy.
3. Enter the namespace to which the SOAP operation belongs in the **Namespace**
    field.

## Relative path resolver

The **Relative Path**
filter enables you to identify an incoming XML message based on the relative path on which the message is received.

The following example shows how to find the relative path of an incoming message. Consider the following SOAP message:

```
POST /services/helloService HTTP/1.1
Host:localhost:8095
Content-Length:196
SOAPAction:HelloService
Accept-Language:en-US
UserAgent:API Gateway
Content-Type:text/XML; utf-8
```

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Header></soap:Header>
   <soap:Body>
       <getHello xmlns="http://www.axway"/>.com/"/>
   </soap:Body>
</soap:Envelope>
```

The relative path for this message is as follows:

```
/services/helloService
```

To configure the **Relative Path**
filter:

1. Enter an appropriate name for the filter to display in a policy in the **Name**
    field.
2. Enter a regular expression to match the value of the relative path on which messages are received in the **Relative Path**
    field.

    For example, enter `^/services/helloService$`
    to exactly match a path with a value of `/services/helloService`
    . Incoming messages received on a matching relative path value are passed on to the next filter on the success path in the policy.
