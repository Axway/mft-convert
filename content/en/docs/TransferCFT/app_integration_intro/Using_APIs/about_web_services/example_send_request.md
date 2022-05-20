---
title: "Example send file transfer request"
linkTitle: "Example send file request"
weight: 320
---This section describes how to execute a SEND transfer request using Web services. After submitting a request you can retrieve transfer processing information in the {{< TransferCFT/axwayvariablesComponentLongName  >}} catalog using a [Web services catalog search request](../example_search_catalog). Responses to SEND requests may differ depending on the type of COM media file that you are using (File or TCP).

## XFER_CMD_SEND_FILE request

This example request uses only the minimal number of options needed to submit the SEND file SOAP request. You can modify as needed.

&lt;soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:axw="http://www.axway.com">

&lt;soapenv:Header>

> &lt;axw:ClientAuth>

> > &lt;axw:user encoding="base64">user1&lt;/axw:user>

> > &lt;axw:password encoding="base64">user1password&lt;/axw:password>

> &lt;/axw:ClientAuth>

&lt;/soapenv:Header>

&lt;soapenv:Body>

> > &lt;axw:XFER_CMD_SEND_FILE &gt;
>
> > &lt;axw:IDF>TEST&lt;/axw:IDF>
> >
> > &lt;axw:FNAME>FiletoTransfer.txt&lt;/axw:FNAME>
>
> > &lt;axw:PARTID>PARIS&lt;/axw:PARTID>
>
> > &lt;/axw:XFER_CMD_SEND_FILE>

&lt;/soapenv:Body>

&lt;/soapenv:Envelope>

## Successful response

In the following successful response, you can see that the IDTU value CAT_IDTU is returned. While the IDTU value CAT_IDTU indicates that the request is correctly delivered to {{< TransferCFT/axwayvariablesComponentLongName  >}}, you do not know the transfer status. To obtain the transfer status, see the section describing how to use the [XFER_CAT_SELECT](../example_search_catalog) function to view the catalog.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

&lt;soap:Body>

> &lt;XFER_CMD_SEND_FILEResponse &gt;
>
> > &lt;CAT_IDTU>A000002M&lt;/CAT_IDTU>
> >
> > &lt;RETURN_CODE>3&lt;/RETURN_CODE>
> >
> > &lt;RETURN_MESSAGE/>
>
> &lt;/XFER_CMD_SEND_FILEResponse>

&lt;/soap:Body>

&lt;/soap:Envelope>

> **Note**
>
> The values displayed are example values and yours results may differ.

## Unsuccessful response

The request response differs depending on if Transfer CFT is running, and if the Transfer CFT Copilot server is started, as described in the following sections.

### {{< TransferCFT/axwayvariablesComponentLongName  >}} is down

An error message (unsuccessful response) is displayed if Copilot is running, but {{< TransferCFT/axwayvariablesComponentLongName  >}} is down. The following two examples demonstrate the different responses depending on the COM media type being used.

#### Using the TCP media type

If Transfer CFT is not started and you are using TCP, an error message is displayed.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;soap:Body>
>
> &lt;XFER_CMD_SEND_FILEResponse xmlns="http://www.axway.com">
>
> > &lt;RETURN_CODE>7&lt;/RETURN_CODE>
> >
> > &lt;RETURN_MESSAGE>ERROR : Open channel failed  (-6006/0).
> >
> > CSCcom()&lt;/RETURN_MESSAGE>
>
> &lt;/XFER_CMD_SEND_FILEResponse>
>
> &lt;/soap:Body>

&lt;/soap:Envelope>

> **Note**
>
> The return code here is 7, while a successful request would return the value 3.

#### Using the File media type

In this scenario no IDTU value is displayed because the request is not immediately processed by Transfer CFT. Instead the request is temporarily stored in the COM file until Transfer CFT is started.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;soap:Body>
>
> &lt;XFER_CMD_SEND_FILEResponse xmlns="http://www.axway.com">
>
> > &lt;RETURN_CODE>3&lt;/RETURN_CODE>
> >
> > &lt;RETURN_MESSAGE/>
>
> &lt;/XFER_CMD_SEND_FILEResponse>
>
> &lt;/soap:Body>

&lt;/soap:Envelope>

### Copilot is down

If Copilot is down you get a "connection refused" type of error, regardless of whether Transfer CFT is running or not.

****Related topics****

[Get started with Web services](../get_started_web_services)
