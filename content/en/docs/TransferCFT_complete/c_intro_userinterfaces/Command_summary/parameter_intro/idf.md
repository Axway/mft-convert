{
    "title": "idf",
    "linkTitle": "idf",
    "weight": "1530"
}<span id="idf"></span>

### idf

<span id="idf_END"></span><span id="idf_CFTCAT"></span>

#### END, START, KEEP, HALT, SUBMIT, DELETE, LISTCAT, DISPLAY, RESUME, KSTATE

****[IDF = identifier]****

File type identifier.

Several catalog entries may be associated with a given IDF. There is
no default value.

If a set of transfers is selected, transfers are processed in batches
of 20 every 5 seconds.

<span id="idf_CFTAUTH"></span>

#### CFTAUTH

****[IDF = {identifier &#124; mask, identifier &#124; mask
} ****]********

List of authorized or prohibited IDF.

Specify a list of up to 200 model file identifiers.

The value associated with each of these ****idfs****
can be:

- an explicit file
    identifier
- a mask

<span id="idf_CFTETB"></span>

#### CFTETB

****[IDF = identifier]****

Local identifier of the model file associated with this card format
(ID of CFTSEND/ CFTRECV).

If this parameter is not defined (default value = 8 blank characters),
the card is suitable for all model files.

*In requester mode*, the parameter
card consists of either:

- a
    CFTETB description, where the IDF equals the model file identifier of
    the transfer request, or
- the
    description CFTETB IDF = ‘ ’ if the above description does not exist

*In server mode*, the description CFTETB
IDF = ‘ ’ is always used to decode the parameter card received

The following descriptions are hence required for operation in server
mode:

CFTETB     DIRECT     =    
SEND,     ID     =    
ALL,     IDF     =    
‘ ’  
CFTETB     DIRECT     =    
RECV,     ID     =    
ALL,     IDF     =    
‘ ’

<span id="idf_CFTPART"></span>

#### CFTPART

****[IDF = identifier ]****

Default 32-character identifier of the file for the partner.

This parameter offers the possibility of designating a generic name
for each partner when the IDF in the object does not exist in the parameter
base. This name, which is valid for send transfers (SEND) and for receive
transfers (RECV), takes precedence over the DEFAULT parameter of the CFTPARM
object.

<span id="idf_CFTPROT"></span>

#### CFTPROT

**[IDF = *string64*]**

Use  to assign an IDF (file idendifier) to a file
on receiving an NIDF that is more than 8 characters long.

This parameter can be used in:

- server mode (sender
    or receiver)
- receiver requester
    following the activation of a RECV IDF=&lt;mask&gt; command

The value of this parameter may be:

- explicit: the authorized
    character string must then not be more than 8 characters long
- obtained from one
    or more "substrings" of the symbolic variable &NIDF (corresponding
    to the NIDF received), the character string must not be more than 64 characters
    long. See the definition of a "substring" of a [symbolic variable](../../symbolic_variables).

****Example:****

For a received NIDF containing 19 characters, IDF may take the value:
IDF=&2.3NIDF&12.5NIDF.

This string cannot be more than 64 characters long.

The string must not be more than 8 characters long after substituting
the variables.

In the above example, the length of the IDF value is:  
     - before substitution: 17 characters  
     - after substitution: 8 characters

#### SEND

****[ IDF = identifier ]****

File identifier.

If an identifier is not specified, the CFTPART IDF is used.

If the idf is specified, but does not exist, the idf is forced to IDFDEF, you can disable this functionality. See [cft.default_idf.enable](../../../../admin_intro/uconf/uconf_parameters).

<span id="IDF_send_recv"></span>

#### RECV

****[ IDF = { identifier &#124; mask } ]****

File identifier.

If an identifier is not specified, the CFTPART IDF is used.

If the idf is specified, but does not exist, the idf is forced to IDFDEF, you can disable this functionality. See [cft.default_idf.enable](../../../../admin_intro/uconf/uconf_parameters).

#### CFTNET

**[IDF = *string32*]**

[Return to Command index](../../)
