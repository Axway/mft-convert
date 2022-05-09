{
    "title": "Synchronous  communication services",
    "linkTitle": "Synchronous communication services in COBOL",
    "weight": "350"
}{{% TransferCFT/snippets/synchcom_api%}}
<span id="Call Syntax"></span>

Call syntax
-----------

```
CALL     "CFTU"    
USING     <verb>       <param>    
<rc>
CALL     "CFTC"    
USING     <verb>       <param>    
<rc>
```

Where:

- CFTU indicates
    that syntax analysis is requested  
    CFTC indicates that syntax analysis is not requested
- &lt;verb&gt; is
    the command that you want to process
- &lt;param&gt; is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc&gt; is the
    return code

The available &lt;verbs&gt; are listed in the following table.


| **&lt;verb&gt;** | **Value** | **Service** |
| --- | --- | --- |
| F-COM | COM | Communication mode |


The available &lt;param&gt; are listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| F-COM | D-COM | The COM command parameter structure is as follows: &lt;medium type&gt; = &lt;Medium name&gt;<br/> The medium type consists in an uppercase letter:<br/> • 'F' for file<br/> • 'T' for the TCP/IP synchronous medium<br/> • 'C' for the configuration file (ConfigFileName)<br/> The medium name is the:<br/> • Filename, if the medium type is 'F'<br/> • Name of the communication channel, if the medium type is 'T'<br/> • Name of the configuration file containing the medium of communication characteristics, if the medium type is C. |


Return codes
------------


| Mnemonic | Description |
| --- | --- |
| CAPI-NOERR | No error |
| CAPI-FUNC-UNDEF | Command not valid |
| CAPI-COM-OPEN | Communication medium opening error |
| CAPI-COM-WRITE | Communication medium write error |
| CAPI-COM-CLOSE | Communication medium closing problem |
| CAPI-COM-ALLOC | Communication medium allocation problem |
| CAPI-COM-ERR | Communication medium not available on this system |


The available &lt;verbs&gt; are
listed in the following table.


| &lt;verb&gt; | Value | Service |
| --- | --- | --- |
| F-COM | COM | Communication mode |
| F-GETINXFO | GETINXFO | Recovering information about a transfer made from a synchronous request |


The available &lt;param&gt; are
listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| F-COM | D-COM | The COM command parameter structure is as follows: &lt;medium type&gt; = &lt;Medium name&gt;<br/> The medium type consists in an uppercase letter:<br/> • 'F' for file<br/> • 'T' for the TCP/IP synchronous medium<br/> • 'C' for the configuration file (ConfigFileName)<br/> The medium name is the:<br/> • Filename, if the medium type is 'F'<br/> • Name of the communication channel, if the medium type is 'T'<br/> • Name of the configuration file containing the medium of communication characteristics, if the medium type is C. |
| F-GETINXFO | Z-XINF | Information about a transfer in the format described in the ****OAPIINF**** file. |

