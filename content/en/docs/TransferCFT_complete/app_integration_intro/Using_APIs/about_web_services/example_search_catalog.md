{
    "title": "Example catalog search request",
    "linkTitle": "Example catalog search request",
    "weight": "330"
}Use this request to search for information in the catalog, for example details about the status of a transfer request.

XFER_CAT_SELECT request with IDTU
-----------------------------------

In this example the XFER_CAT_SELECT request uses the IDTU.

&lt;SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"&gt;

> &lt;SOAP-ENV:Header&gt;

> > &lt;m:ClientAuth xmlns:m="http://www.axway.com"&gt;
> >
> > &lt;m:user &gt;user1&lt;/m:user&gt;encoding="base64"/&gt;
> >
> > &lt;m:password&gt;user1&lt;/m:password&gt; encoding="base64"/&gt;
> >
> > &lt;/m:ClientAuth&gt;
>
> &lt;/SOAP-ENV:Header&gt;

> &lt;SOAP-ENV:Body&gt;
>
> > &lt;m:XFER_CAT_SELECT xmlns:m="http://www.axway.com"&gt;
> >
> > &lt;m:CAT_FILENAME&gt;$CFTCATA&lt;/m:CAT_FILENAME&gt;
>
> &lt;m:PERSISTENCE_LOCALIZATION&gt;PERSISTENCE_ON_WORKSTATION&lt;/m:PERSISTENCE_LOCALIZATION&gt;
>
> &lt;m:PERSISTENCE_ACCESSIBILITY&gt;PERSISTENCE_USER_ALL&lt;/m:PERSISTENCE_ACCESSIBILITY&gt;
>
> > &lt;m:DIRECT&gt;BOTH&lt;/m:DIRECT&gt;
> >
> > &lt;m:IDTU_ARRAY&gt;
> >
> > > &lt;m:IDTU&gt;A0000030&lt;/m:IDTU&gt;
> >
> > &lt;/m:IDTU_ARRAY&gt;
> >
> > &lt;m:CONTENT_SUBSET&gt;
> >
> > > &lt;m:CAT_CONTENT&gt;CAT_BRIEF&lt;/m:CAT_CONTENT&gt;
> > >
> > > &lt;m:SELECT_FIELDS_ARRAY&gt;
> > >
> > > &lt;m:SELECT_FIELD&gt;String&lt;/m:SELECT_FIELD&gt;
> >
> > > &lt;/m:SELECT_FIELDS_ARRAY&gt;
> >
> > &lt;/m:CONTENT_SUBSET&gt;
> >
> > &lt;/m:XFER_CAT_SELECT&gt;
>
> &lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

Successful response
-------------------

### {{< TransferCFT/axwayvariablesComponentLongName  >}} is down

Executing the this request when Copilot is running but {{< TransferCFT/axwayvariablesComponentLongName  >}} is not running returns the same response.

&lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?&gt;

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"&gt;

&lt;soap:Body&gt;

> &lt;XFER_CAT_SELECTResponse xmlns="http://www.axway.com"&gt;
>
> &lt;CAT_MAXRECORDS&gt;10000&lt;/CAT_MAXRECORDS&gt;
>
> &lt;CAT_CURRENTRECORDS&gt;12&lt;/CAT_CURRENTRECORDS&gt;
>
> &lt;LISTCAT_ARRAY&gt;
>
> &lt;LISTCAT_SUBSET&gt;
>
> &lt;CAT_FREC&gt;14&lt;/CAT_FREC&gt;
>
> &lt;CAT_DIAGI&gt;0&lt;/CAT_DIAGI&gt;
>
> &lt;CAT_NREC&gt;14&lt;/CAT_NREC&gt;
>
> &lt;CAT_TYPE&gt;FILE&lt;/CAT_TYPE&gt;
>
> &lt;CAT_IDA&gt;30080&lt;/CAT_IDA&gt;
>
> &lt;CAT_DIAGC/&gt;
>
> &lt;CAT_IDT&gt;E0314250&lt;/CAT_IDT&gt;
>
> &lt;CAT_IDF&gt;TEST&lt;/CAT_IDF&gt;
>
> &lt;CAT_DIAGP&gt;CP NONE&lt;/CAT_DIAGP&gt;
>
> &lt;CAT_PART&gt;LOOP&lt;/CAT_PART&gt;
>
> &lt;CAT_STATE&gt;CAT_STATE_CONSUMED&lt;/CAT_STATE&gt;
>
> &lt;CAT_ACK&gt;false&lt;/CAT_ACK&gt;
>
> &lt;CAT_NACK&gt;false&lt;/CAT_NACK&gt;
>
> &lt;CAT_CFTSTATE&gt;X&lt;/CAT_CFTSTATE&gt;
>
> &lt;CAT_DIRECT&gt;SEND&lt;/CAT_DIRECT&gt;
>
> &lt;/LISTCAT_SUBSET&gt;
>
> &lt;/LISTCAT_ARRAY&gt;
>
> &lt;RETURN_CODE&gt;3&lt;/RETURN_CODE&gt;
>
> &lt;RETURN_MESSAGE/&gt;
>
> &lt;/XFER_CAT_SELECTResponse&gt;

> &lt;/soap:Body&gt;

&lt;/soap:Envelope&gt;

> **Note**
>
> Note: To retrieve the Phase, Phasestep and  Appstate statuses you can set the CAT_CONTENT value to FULL. However, setting the catalog to FULL returns a large number of lines in the catalog.

> &lt;CAT_PHASE&gt;X&lt;/CAT_PHASE&gt;
>
> &lt;CAT_PHASESTEP&gt;X&lt;/CAT_PHASESTEP&gt;
>
> &lt;CAT_APPSTATE/&gt;

Response when there is no IDTU
------------------------------

In the following response the return code is 3, successful, but the LISCAT_ARRAY is empty.

&lt;soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"&gt;

> &lt;soap:Body&gt;
>
> > &lt;XFER_CAT_SELECTResponse xmlns="http://www.axway.com"&gt;
> >
> > > &lt;CAT_MAXRECORDS&gt;10000&lt;/CAT_MAXRECORDS&gt;
> > >
> > > &lt;CAT_CURRENTRECORDS&gt;12&lt;/CAT_CURRENTRECORDS&gt;
> > >
> > > &lt;LISTCAT_ARRAY/&gt;
> > >
> > > &lt;RETURN_CODE&gt;3&lt;/RETURN_CODE&gt;
> > >
> > > &lt;RETURN_MESSAGE/&gt;
> >
> > &lt;/XFER_CAT_SELECTResponse&gt;
>
> &lt;/soap:Body&gt;

&lt;/soap:Envelope&gt;

Unsuccessful response
---------------------

### Copilot is down

The exact message text may vary depending on the selected COM media type (File or TCP).

****Related topics****

[Get started with Web services](../get_started_web_services)
