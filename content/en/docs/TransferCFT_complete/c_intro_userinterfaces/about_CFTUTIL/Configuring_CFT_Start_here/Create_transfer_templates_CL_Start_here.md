{
    "title": "Ongoing  CFTSEND",
    "linkTitle": "Ongoing CFTSEND",
    "weight": "440"
}<span id="About_the_ongoing_CFTSEND_object"></span>

About the ongoing CFTSEND object
--------------------------------

Use the CFTSEND object to specify:

- The name and local
    physical characteristics of the file to send
- The network characteristics
    of the file to send to the partner
- The actions to
    perform locally during and after a transfer (translation, compression,
    call to a user EXIT, an end-of-transfer procedure...)
- An authorized time
    slot and default user associated with the transfers

There is no limit to the number of CFTSEND objects that you can create.

You can create a default CFTSEND object. The identifier must correspond
to the file identifier idf or,
if this parameter is not defined, the default parameter of the CFTPARM
object.

This topic describes the command used to create a transfer template.
The parameters are described in the CFTSEND and CFTRECV object topics.

****Related
topics****

- Command syntax
    [CFTSEND](../../../command_summary#CFTSEND)
