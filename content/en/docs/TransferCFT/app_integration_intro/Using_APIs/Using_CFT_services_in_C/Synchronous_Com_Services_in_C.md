{
    "title": "Synchronous  communication services",
    "linkTitle": "Synchronous communication services in C",
    "weight": "350"
}{{% TransferCFT/snippets/synchcom_api%}}
<span id="Call Syntax"></span>

Call syntax
-----------

`rc =      cftau (verb,param)`

`rc =      cftac (verb,param)`

Where:

- cftau indicates
    that syntax analysis is requested  
    cftac indicates that syntax analysis is not requested
- &lt;verb&gt; is
    the command that you want to process
- &lt;param&gt; is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc&gt; is the
    return code

The available &lt;verbs&gt; are listed in the following table.


| &lt;verb&gt; | Service |
| --- | --- |
| COM | Communication mode |
| GETXINFO | Retrieving information about a transfer made from a synchronous request |


The available &lt;param&gt; are listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Description |
| --- | --- | --- |
| com<br/>  | param<br/>  | The COM command parameter structure is as follows: &lt;medium type&gt; = &lt;Medium name&gt;<br/> The medium type consists in an uppercase letter:<br/> • 'F' for file<br/> • 'T' for the TCP/IP synchronous medium<br/> • 'C' for the configuration file (ConfigFileName)<br/> The medium name is the:<br/> • Filename, if the medium type is 'F'<br/> • Name of the communication channel, if the medium type is 'T'<br/> • Name of the configuration file containing the medium of communication characteristics, if the medium type is C. |
| getxinfo | xinf | Information about a transfer in the format described in the cftapi.h file. |


Step procedure
--------------

Use the COM command to define the synchronous medium.

1. Open the synchronous communication.
1. Write the command. This is not specific to synchronous mediums.
1. Retrieve information using the GETXINFO service.

To view the synchronous communication template containing details and an example, see tcftsyn.

Return codes
------------


| Mnemonic | Description |
| --- | --- |
| CAPI_NOERR | No error. |
| CAPI_FUNC_UNDEF | Command not valid. |
| CAPI_COM_OPEN | Communication medium opening error. |
| CAPI_COM_WRITE | Communication medium write error. |
| CAPI_COM_CLOSE | Communication medium closing problem. |
| CAPI_COM_ALLOC | Communication medium allocation problem. |
| CAPI_COM_ERR | Communication medium not available on this system. |

