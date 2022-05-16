---

    title: Example catalog search request
    linkTitle: Example catalog search request
    weight: 330

---
Use this request to search for information in the catalog, for example details about the status of a transfer request.

## XFER\_CAT\_SELECT request with IDTU

In this example the XFER\_CAT\_SELECT request uses the IDTU.

&lt;SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;SOAP-ENV:Header>

> > &lt;m:ClientAuth xmlns:m="http://www.axway.com">
> >
> > &lt;m:user &gt;user1&lt;/m:user>encoding="base64"/>
> >
> > &lt;m:password>user1&lt;/m:password> encoding="base64"/>
> >
> > &lt;/m:ClientAuth>
>
> &lt;/SOAP-ENV:Header>

> &lt;SOAP-ENV:Body>
>
> > &lt;m:XFER\_CAT\_SELECT xmlns:m="http://www.axway.com">
> >
> > &lt;m:CAT\_FILENAME>$CFTCATA&lt;/m:CAT\_FILENAME>
>
> &lt;m:PERSISTENCE\_LOCALIZATION>PERSISTENCE\_ON\_WORKSTATION&lt;/m:PERSISTENCE\_LOCALIZATION>
>
> &lt;m:PERSISTENCE\_ACCESSIBILITY>PERSISTENCE\_USER\_ALL&lt;/m:PERSISTENCE\_ACCESSIBILITY>
>
> > &lt;m:DIRECT>BOTH&lt;/m:DIRECT>
> >
> > &lt;m:IDTU\_ARRAY>
> >
> > > &lt;m:IDTU>A0000030&lt;/m:IDTU>
> >
> > &lt;/m:IDTU\_ARRAY>
> >
> > &lt;m:CONTENT\_SUBSET>
> >
> > > &lt;m:CAT\_CONTENT>CAT\_BRIEF&lt;/m:CAT\_CONTENT>
> > >
> > > &lt;m:SELECT\_FIELDS\_ARRAY>
> > >
> > > &lt;m:SELECT\_FIELD>String&lt;/m:SELECT\_FIELD>
> >
> > > &lt;/m:SELECT\_FIELDS\_ARRAY>
> >
> > &lt;/m:CONTENT\_SUBSET>
> >
> > &lt;/m:XFER\_CAT\_SELECT>
>
> &lt;/SOAP-ENV:Body>

&lt;/SOAP-ENV:Envelope>

## Successful response

### {{< TransferCFT/axwayvariablesComponentLongName  >}} is down

Executing the this request when Copilot is running but {{< TransferCFT/axwayvariablesComponentLongName  >}} is not running returns the same response.

&lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?>

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

&lt;soap:Body>

> &lt;XFER\_CAT\_SELECTResponse xmlns="http://www.axway.com">
>
> &lt;CAT\_MAXRECORDS>10000&lt;/CAT\_MAXRECORDS>
>
> &lt;CAT\_CURRENTRECORDS>12&lt;/CAT\_CURRENTRECORDS>
>
> &lt;LISTCAT\_ARRAY>
>
> &lt;LISTCAT\_SUBSET>
>
> &lt;CAT\_FREC>14&lt;/CAT\_FREC>
>
> &lt;CAT\_DIAGI>0&lt;/CAT\_DIAGI>
>
> &lt;CAT\_NREC>14&lt;/CAT\_NREC>
>
> &lt;CAT\_TYPE>FILE&lt;/CAT\_TYPE>
>
> &lt;CAT\_IDA>30080&lt;/CAT\_IDA>
>
> &lt;CAT\_DIAGC/>
>
> &lt;CAT\_IDT>E0314250&lt;/CAT\_IDT>
>
> &lt;CAT\_IDF>TEST&lt;/CAT\_IDF>
>
> &lt;CAT\_DIAGP>CP NONE&lt;/CAT\_DIAGP>
>
> &lt;CAT\_PART>LOOP&lt;/CAT\_PART>
>
> &lt;CAT\_STATE>CAT\_STATE\_CONSUMED&lt;/CAT\_STATE>
>
> &lt;CAT\_ACK>false&lt;/CAT\_ACK>
>
> &lt;CAT\_NACK>false&lt;/CAT\_NACK>
>
> &lt;CAT\_CFTSTATE>X&lt;/CAT\_CFTSTATE>
>
> &lt;CAT\_DIRECT>SEND&lt;/CAT\_DIRECT>
>
> &lt;/LISTCAT\_SUBSET>
>
> &lt;/LISTCAT\_ARRAY>
>
> &lt;RETURN\_CODE>3&lt;/RETURN\_CODE>
>
> &lt;RETURN\_MESSAGE/>
>
> &lt;/XFER\_CAT\_SELECTResponse>

> &lt;/soap:Body>

&lt;/soap:Envelope>

> **Note**
>
> To retrieve the Phase, Phasestep and  Appstate statuses you can set the CAT\_CONTENT value to FULL. However, setting the catalog to FULL returns a large number of lines in the catalog.

> &lt;CAT\_PHASE>X&lt;/CAT\_PHASE>
>
> &lt;CAT\_PHASESTEP>X&lt;/CAT\_PHASESTEP>
>
> &lt;CAT\_APPSTATE/>

## Response when there is no IDTU

In the following response the return code is 3, successful, but the LISCAT\_ARRAY is empty.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">

> &lt;soap:Body>
>
> > &lt;XFER\_CAT\_SELECTResponse xmlns="http://www.axway.com">
> >
> > > &lt;CAT\_MAXRECORDS>10000&lt;/CAT\_MAXRECORDS>
> > >
> > > &lt;CAT\_CURRENTRECORDS>12&lt;/CAT\_CURRENTRECORDS>
> > >
> > > &lt;LISTCAT\_ARRAY/>
> > >
> > > &lt;RETURN\_CODE>3&lt;/RETURN\_CODE>
> > >
> > > &lt;RETURN\_MESSAGE/>
> >
> > &lt;/XFER\_CAT\_SELECTResponse>
>
> &lt;/soap:Body>

&lt;/soap:Envelope>

## Unsuccessful response

### Copilot is down

The exact message text may vary depending on the selected COM media type (File or TCP).

****Related topics****

[Get started with Web services](../get_started_web_services)
