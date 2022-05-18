---
title: "Example catalog search request"
linkTitle: "Example catalog search request"
weight: 330
--- Use this request to search for information in the catalog, for example details about the status of a transfer request.

## XFER_CAT_SELECT request with IDTU

In this example the XFER_CAT_SELECT request uses the IDTU.

&lt;SOAP- ENV:Envelope xmlns:SOAP- ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP- ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:xsi="http://www.w3.org/2001/XMLSchema- instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;SOAP- ENV:Header>

> > &lt;m:ClientAuth xmlns:m="http://www.axway.com">
> >
> > &lt;m:user &gt;user1&lt;/m:user>encoding="base64"/>
> >
> > &lt;m:password>user1&lt;/m:password> encoding="base64"/>
> >
> > &lt;/m:ClientAuth>
>
> &lt;/SOAP- ENV:Header>

> &lt;SOAP- ENV:Body>
>
> > &lt;m:XFER_CAT_SELECT xmlns:m="http://www.axway.com">
> >
> > &lt;m:CAT_FILENAME>$CFTCATA&lt;/m:CAT_FILENAME>
>
> &lt;m:PERSISTENCE_LOCALIZATION>PERSISTENCE_ON_WORKSTATION&lt;/m:PERSISTENCE_LOCALIZATION>
>
> &lt;m:PERSISTENCE_ACCESSIBILITY>PERSISTENCE_USER_ALL&lt;/m:PERSISTENCE_ACCESSIBILITY>
>
> > &lt;m:DIRECT>BOTH&lt;/m:DIRECT>
> >
> > &lt;m:IDTU_ARRAY>
> >
> > > &lt;m:IDTU>A0000030&lt;/m:IDTU>
> >
> > &lt;/m:IDTU_ARRAY>
> >
> > &lt;m:CONTENT_SUBSET>
> >
> > > &lt;m:CAT_CONTENT>CAT_BRIEF&lt;/m:CAT_CONTENT>
> > >
> > > &lt;m:SELECT_FIELDS_ARRAY>
> > >
> > > &lt;m:SELECT_FIELD>String&lt;/m:SELECT_FIELD>
> >
> > > &lt;/m:SELECT_FIELDS_ARRAY>
> >
> > &lt;/m:CONTENT_SUBSET>
> >
> > &lt;/m:XFER_CAT_SELECT>
>
> &lt;/SOAP- ENV:Body>

&lt;/SOAP- ENV:Envelope>

## Successful response

### {{< TransferCFT/axwayvariablesComponentLongName  >}} is down

Executing the this request when Copilot is running but {{< TransferCFT/axwayvariablesComponentLongName  >}} is not running returns the same response.

&lt;?xml version="1.0" encoding="UTF- 8" standalone="yes"?>

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema- instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

&lt;soap:Body>

> &lt;XFER_CAT_SELECTResponse xmlns="http://www.axway.com">
>
> &lt;CAT_MAXRECORDS>10000&lt;/CAT_MAXRECORDS>
>
> &lt;CAT_CURRENTRECORDS>12&lt;/CAT_CURRENTRECORDS>
>
> &lt;LISTCAT_ARRAY>
>
> &lt;LISTCAT_SUBSET>
>
> &lt;CAT_FREC>14&lt;/CAT_FREC>
>
> &lt;CAT_DIAGI>0&lt;/CAT_DIAGI>
>
> &lt;CAT_NREC>14&lt;/CAT_NREC>
>
> &lt;CAT_TYPE>FILE&lt;/CAT_TYPE>
>
> &lt;CAT_IDA>30080&lt;/CAT_IDA>
>
> &lt;CAT_DIAGC/>
>
> &lt;CAT_IDT>E0314250&lt;/CAT_IDT>
>
> &lt;CAT_IDF>TEST&lt;/CAT_IDF>
>
> &lt;CAT_DIAGP>CP NONE&lt;/CAT_DIAGP>
>
> &lt;CAT_PART>LOOP&lt;/CAT_PART>
>
> &lt;CAT_STATE>CAT_STATE_CONSUMED&lt;/CAT_STATE>
>
> &lt;CAT_ACK>false&lt;/CAT_ACK>
>
> &lt;CAT_NACK>false&lt;/CAT_NACK>
>
> &lt;CAT_CFTSTATE>X&lt;/CAT_CFTSTATE>
>
> &lt;CAT_DIRECT>SEND&lt;/CAT_DIRECT>
>
> &lt;/LISTCAT_SUBSET>
>
> &lt;/LISTCAT_ARRAY>
>
> &lt;RETURN_CODE>3&lt;/RETURN_CODE>
>
> &lt;RETURN_MESSAGE/>
>
> &lt;/XFER_CAT_SELECTResponse>

> &lt;/soap:Body>

&lt;/soap:Envelope>

> **Note**
>
> To retrieve the Phase, Phasestep and  Appstate statuses you can set the CAT_CONTENT value to FULL. However, setting the catalog to FULL returns a large number of lines in the catalog.

> &lt;CAT_PHASE>X&lt;/CAT_PHASE>
>
> &lt;CAT_PHASESTEP>X&lt;/CAT_PHASESTEP>
>
> &lt;CAT_APPSTATE/>

## Response when there is no IDTU

In the following response the return code is 3, successful, but the LISCAT_ARRAY is empty.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema- instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;soap:Body>
>
> > &lt;XFER_CAT_SELECTResponse xmlns="http://www.axway.com">
> >
> > > &lt;CAT_MAXRECORDS>10000&lt;/CAT_MAXRECORDS>
> > >
> > > &lt;CAT_CURRENTRECORDS>12&lt;/CAT_CURRENTRECORDS>
> > >
> > > &lt;LISTCAT_ARRAY/>
> > >
> > > &lt;RETURN_CODE>3&lt;/RETURN_CODE>
> > >
> > > &lt;RETURN_MESSAGE/>
> >
> > &lt;/XFER_CAT_SELECTResponse>
>
> &lt;/soap:Body>

&lt;/soap:Envelope>

## Unsuccessful response

### Copilot is down

The exact message text may vary depending on the selected COM media type (File or TCP).

****Related topics****

[Get started with Web services](../get_started_web_services)
