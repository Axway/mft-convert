{
    "title": "verify",
    "linkTitle": "verify",
    "weight": "3740"
}<span id="verify"></span>

### {{< TransferCFT/SystemTitle  >}}

#### LISTCOM

****[VERIFY = { YES &#124; NO
}]****

Request to verify the validity of each record in the file at the time
it is listed or displayed.

#### CFTTCP

****[VERIFY = {0
&#124; n } ]****

Option to verify the partner number (DIALNO) on an incoming connection
request (the first "n" digits of the caller number are checked).

If VERIFY = 0 no verification is performed.

#### CFTSSL DIRECT=SERVER

****[VERIFY = { NONE &#124; REQUIRED
&#124; OPTIONAL } ]****

- NONE: Only the server must be authenticated. 
- REQUIRED: The server and the client must be authenticated.
- OPTIONAL: Requests mutual authentication, but even if the client does not send a valid certificate the handshake is successful.

#### CFTSSL DIRECT=CLIENT

The DIRECT=CLIENT VERIFY options are available as of {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.3.2 SP2.

****[VERIFY = { <span class="underline">NONE</span> &#124; REQUIRED
&#124; OPTIONAL &#124; ENFORCED } ]****

- ENFORCED: Ensures client authentication with the server. During the handshake, if the server does not ask for the client certificate, then the transfer fails.
- OPTIONAL and REQUIRED: The same as NONE (for backward compatibility), but should not be used.
- NONE: Only the server must be authenticated.

**** ****

****Example 1****

This example demonstrates the use of the client `NONE` value.

`CFTSSL type=client, verify=none and CFTSSL type=server, verify=none`

` `

****Example 2****

This example demonstrates the use of the client `ENFORCED `value.

`CFTSSL type=client, verify=ENFORCED and CFTSSL type=server, verify=required`

![](/Images/TransferCFT/verify1.png)

 

****Example 3****

This example demonstrates a different use of the `ENFORCED `value. When acting as a client, `ENFORCED `enables Transfer CFT to cancel a transfer if the server does not require the client authentication. Here, the transfer fails with diagi 260 due to the fact that the client requires authentication:

`CFTSSL type=client, verify=ENFORCED and CFTSSL type=server, verify=NONE`

[Return to Command index](../../)
