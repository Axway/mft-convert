---
title: "About Web services"
linkTitle: "Using Web services"
weight: 290
---This documentation describes the Transfer CFT Web services interface option, and provides instructions for getting started with Web services, executing a SEND file transfer request, and retrieving the request details from the catalog.

Web services provide a way for applications to use software services over networks such as the Internet. Client applications use the Web Services Description Language (WSDL) to do this and exchange data using XML.
Since you can use URLs, HTTP, and XML to access Web services, applications running on a variety of platforms and using various languages can access XML Web services.

You can use Web services to access all of the main functions for
managing Transfer CFT such as:

* Configure Transfer
    CFT: create, modify, and view configuration objects
* Monitor Transfer
    CFT: consult the log, the catalog, and transfer statistics
* Create transfers:
    create a new file transfer, a message or transfer reply

## License key

You require a license key that includes the Web services option for the Transfer
CFT product. To request a key,
contact your Axway sales representative.

## About the WSDL file

To use Web services with Transfer CFT, you need a [WSDL Web Services Description Language]() file. Your installed Transfer CFT product comes with a WSDL file that describes the operations, operation attributes, requests and response structure. You can access the WSDL file from your local installation at:

* On UNIX/Windows: &lt;cft_installation_directory>/distrib/copilot/wsdl/copilotcft.wsdl
* On z/OS: &lt;copilot. http.httprootdir>/wsdl/copilotcft.wsdl
* On IBM i: &lt;install_directory>/distrib/copilot/wsdl/copilotcft.wsdl
* On OpenVMS: CFTINSTALLDIR :[distrib.copilot.wsdl]copilotcft.wsdl

Using the WSDL file, you can automatically generate a Web services client. You can use an open source Web service Java toolkit, for example Axis from The Apache Software Foundation, to create this Web services client.

The Web services requests sent by the client are then processed on the Transfer CFT Copilot server listening port.

### Additional resources

For more information on the following subjects, go to:

* WSDL: [www.w3schools.com/wsdl/](http://www.w3schools.com/wsdl/) 
* SOAP: [www.w3schools.com/soap/](http://www.w3schools.com/soap/)
* Axis: [axis.apache.org/axis/](https://axis.apache.org/axis/)

You can find SOAP samples and documentation at:

* Transfer CFT Web services documentation: &lt;cft_installation_directory>/distrib/copilot/wsdl/doc/index.html
* SOAP request samples:&lt;cft_installation_directory>/distrib/copilot/wsdl/sample

> **Note**
>
> For UNIX/Windows systems, for other platforms refer to the platform specific paths.

## General restrictions

The Web services process can receive any SOAP request that conforms
to the W3C specifications and WS-I Recommendations.

The following restrictions apply to Transfer CFT Web services:

* The time zone offset
    is not supported for XSD types Time
    and DateTime.
* Element attributes
    are not supported, nor parsed. This means that if there are incorrect
    spaces in names for any operation attribute or message, they are skipped and
    no error is returned. Exceptions are made to support WS-I recommendations.

## WS-I recommendations

Verify that the correct option is set if your client requests have to
be checked to conform to WS-I recommendations. To do this, set the UCONF [copilot.webservices.wsicomplience] identifier
to yes.

The following WS-I constraints are checked by the UI server for the
XML representation of SOAP messages. These constraints include:

* SOAP Header constraints:
    *   R1009: A MESSAGE
        MUST NOT contain processing instructions
    *   R1012: A MESSAGE
        MUST be serialized as either UTF-8 or UTF-16
* SOAP Body constraints:
    *   R1006: A MESSAGE
        MUST NOT contain soap:encoding
* Style attributes on any element that is a child of soap:Body
    *   R1007: A MESSAGE
        described in an rpc-literal binding MUST NOT contain soap:encodingStyle
        attribute on any elements that are grandchildren of soap:Body
    *   R1014: The
        children of the soap:Body element in a MESSAGE MUST be namespace qualified

For more information on WS-I recommendations, visit [http://www.ws-i.org](http://www.ws-i.org/)

## Limit the number of failed login attempts

Transfer CFT provides brute force protection for logging on the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI, REST API, or Web Services when using either the *system* mode or *xfbadm* mode (UNIX and HP NonStop only) authentication. That is, it limits the number of login failure attempts, where both the user and the password are checked to avoid brute force attacks.

For other authentication methods, such as PassPort and LDAP, no check is made. You must manage that in the Password Policy of those external tools.

You can use the following UCONF parameters to manage this option:

* `copilot.general.login_failures_fname`: A file that stores data shared between Transfer CFT and Copilot.
* `copilot.general.max_login_failures`: An integer that sets the maximum number of login failures for a user (default is 3, and 0 disables this option).

> **Note**
>
> In a multi-host environment, an attacker may have up to the copilot.general.max_login_failures \* &lt;number of host> tries before the user is locked if the file is not in a directory shared by all hosts.

When the maximum number of login failures is reached, the user account is locked for 30 seconds.

****Platform specifics****

* On IBM i systems, there is no action if the password is incorrect as the system offers methods that you can rely on to avoid brute force attacks (the system value is [QMAXSIGN](https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_74/rzarl/rzarlmaxsgn.htm)).
* On z/OS systems, only the inherent system protection is available (refer to the RACF suboperand [REVOKE](https://www.ibm.com/support/knowledgecenter/SSLTBW_2.3.0/com.ibm.zos.v2r3.icha700/setrpw.htm) for the PASSWORD option).
* On OpenVMS systems, only existing system protection is available.

****Related topics****

[Get started with Web services](get_started_web_services)
