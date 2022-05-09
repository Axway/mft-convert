{
    "title": "Event messages",
    "linkTitle": "Event messages",
    "weight": "230"
}{{< TransferCFT/axwayvariablesComponentLongName  >}} can issue Transfer CFT log messages and account records as Event Messages. A management application can then get these event messages by opening an Event Management System (EMS) distributor process and requesting the messages.

In this chapter, event-message tokens and their values are represented in DDL. For a quick explanation of DDL as it applies to SPI, refer to the *SPI Programming Manual* &gt; *[Summary of DDL for SPI](http://h20565.www2.hpe.com/hpsc/doc/public/display?sp4ts.oid=4201303&docId=emr_na-c02131958&docLocale=en_US)* appendix.

For general information on how an application obtains event messages from a subsystem, refer to the EMS Manual.

> **Note**
>
> Note: For any HP documentation referenced in this guide, you should check for the most recent version on the HP Support Center.

Event messages format
---------------------

{{< TransferCFT/axwayvariablesComponentLongName  >}} Guardian events were also available in the previous 2.3 version, with the main difference being that each {{< TransferCFT/axwayvariablesComponentLongName  >}} process was defined as a sub system. In contrast, {{< TransferCFT/axwayvariablesComponentLongName  >}} version {{< TransferCFT/axwayvariablesReleaseNumber  >}} only has one defined sub-system.

All messages have the following tokens:


| Token  | Description  |
| --- | --- |
| ZSPI-TKN-SSID  | The Transfer CFT subsystem ID, whose value is XCF2_VAL_EXTERNAL_SSID. This token is described in the SPI Programming Manual. |
| ZEMS-TKN-EVENTNUMBER  | The event number, as described in the EMS Manual. Its value is one of the values described in the table below. |
| XCF2_TKN_SUBJ  | The message subject for which the values are described in the Event messages table below. |
| XCF2_TKN_MSG  | The message text.<br/> For details about Transfer CFT LOG messages, see the {{< TransferCFT/axwayvariablesComponentLongName  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}} [Messages and error codes](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Troubleshooting/Messages_and_Codes/Messages_and_error_codes_Start_here_1.htm) documentation.<br/> The accounting messages are binary coded data, and are described in the exacct.h header. |
| ZEMS_TKN_EMPHASIS  | If the value is ZSPI-VAL-TRUE, the event being reported is considered critical. This is the case for ERROR and FATAL log events as well as process errors when using the NonStop mode.  |


Event messages

The following table shows the relationship between the event, the subject, and the message type.


| Event number  | EMS subject  | Event type  |
| --- | --- | --- |
| 4  | CFT INFO LOG  | Log information message  |
| 5  | CFT WARN LOG  | Log warning message  |
| 6  | CFT ERR LOG  | Log error message  |
| 7  | CFT FAIL LOG  | Log failure message  |
| 8  | CFT ACCOUNT  | Account message  |


Activate event log messages
---------------------------

The CFTLOG object defines the Transfer CFT log file declarations. The name of the destination is provided in the fname parameter, which can be:

- A file: The files are created using a CFTUTIL command, as described in Transfer CFT Environment.
- Collector: Specifies the name of the collector to which log messages are written.

You can use the NOTIFY parameter of the CFTLOG object to combine the two destinations so that they refer to a Collector.

- NOTIFY: Name of the collector.
- OPERMSG: Allows you to filter the type of messages to be sent.
    -   This number is the sum of the values that correspond to the types of messages you want to filter
    -   For example, Operating error messages=16, System error messages=32, Operating fatal error messages=64, System fatal error messages=128 giving a total of 240

> **Note**
>
> Note: Refer to the Transfer CFT 3.10 Users Guide, available on the documentation portal, for a description of the CFTLOG object parameters.

**Example**

In the Transfer CFT configuration:

```
cftlog id = log0,
…
fname= ' log/cftlog',
afname= ' log/cftalog',
notify= '$COL',
opermsg= 240,
….
 
```

Activate event accounting messages
----------------------------------

The CFTACCNT object defines the destinations for the statistical data concerning terminated transfers (accounting messages). The possible destinations provided in the fname parameter are:

- A file: The files are created using a CFTUTIL command as described in Transfer CFT Environment.
- Collector: Specifies the name of the collector to which account messages are written.

Once defined, you can activate and link the object to the CFTPARM as shown in the following example.

**Example**

```
CFTACCNT ID=ACCNT1, TYPE=FILE, FNAME=$COL,...,MODE=REPLACE
CFTPARM ID=IDPARM0,...,ACCNT=ACCNT1,MODE=REPLACE
```

Transfer CFT EMS
----------------

The CFTPLATE file contains the Transfer CFT templates to be concatenated with the system template for an EMS collector.

The XCFTDDL and XCFTEMS files describe the Transfer CFT information and CFT EMS messages in DDL format.
