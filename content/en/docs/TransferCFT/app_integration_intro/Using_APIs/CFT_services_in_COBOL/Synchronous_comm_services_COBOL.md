---

    title: Synchronous  communication services
    linkTitle: Synchronous communication services in COBOL
    weight: 350

---
This topic describes {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication services.

## Description of functions


| Function | Use |
| --- | --- |
| COM | Set the communication medium |
| GETXINFO | Retrieve information concerning the last transfer made from a synchronous request after a request of the following types: SEND, RECV, HALT, KEEP, START, RESUME, DELETE, END, SUBMIT, SWITCH, PURGE.<br/> The information is stored in a cftApiInf-type structure:<br/> • Transfer state<br/> • Diagnostic<br/> • Diagnostic protocol<br/> • Value of the PART field of CFTPARM<br/> • Transfer identifier (IDT)<br/> • Local transfer identifier (IDTU)<br/> • Transfer type (single, cyclical, diffusion list, collection, file group)<br/> • Public reference of the transfer (only for a single transfer in Send)<br/> The GETXINFO action returns an error if the communication medium is not synchronous.<br/> <blockquote> **Note**<br/> The public reference of the transfer is a character string of variable length. In the PESIT protocol, it contains 'pi13.pi3.pi4.pi11.pi12.pi61.pi62'.<br/> </blockquote>  |


<span id="Call Syntax"></span>

## Call syntax

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
- &lt;verb> is
    the command that you want to process
- &lt;param> is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc> is the
    return code

The available &lt;verbs> are listed in the following table.


| **&lt;verb&gt;** | **Value** | **Service** |
| --- | --- | --- |
| F-COM | COM | Communication mode |


The available &lt;param> are listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| F-COM | D-COM | The COM command parameter structure is as follows: &lt;medium type&gt; = &lt;Medium name&gt;<br/> The medium type consists in an uppercase letter:<br/> • 'F' for file<br/> • 'T' for the TCP/IP synchronous medium<br/> • 'C' for the configuration file (ConfigFileName)<br/> The medium name is the:<br/> • Filename, if the medium type is 'F'<br/> • Name of the communication channel, if the medium type is 'T'<br/> • Name of the configuration file containing the medium of communication characteristics, if the medium type is C. |


## Return codes


| Mnemonic | Description |
| --- | --- |
| CAPI-NOERR | No error |
| CAPI-FUNC-UNDEF | Command not valid |
| CAPI-COM-OPEN | Communication medium opening error |
| CAPI-COM-WRITE | Communication medium write error |
| CAPI-COM-CLOSE | Communication medium closing problem |
| CAPI-COM-ALLOC | Communication medium allocation problem |
| CAPI-COM-ERR | Communication medium not available on this system |


The available &lt;verbs> are
listed in the following table.


| &lt;verb&gt; | Value | Service |
| --- | --- | --- |
| F-COM | COM | Communication mode |
| F-GETINXFO | GETINXFO | Recovering information about a transfer made from a synchronous request |


The available &lt;param> are
listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| F-COM | D-COM | The COM command parameter structure is as follows: &lt;medium type&gt; = &lt;Medium name&gt;<br/> The medium type consists in an uppercase letter:<br/> • 'F' for file<br/> • 'T' for the TCP/IP synchronous medium<br/> • 'C' for the configuration file (ConfigFileName)<br/> The medium name is the:<br/> • Filename, if the medium type is 'F'<br/> • Name of the communication channel, if the medium type is 'T'<br/> • Name of the configuration file containing the medium of communication characteristics, if the medium type is C. |
| F-GETINXFO | Z-XINF | Information about a transfer in the format described in the <span >****OAPIINF****</span> file. |

