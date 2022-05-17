---
    title: " Transfer CFT  catalog query services"
    linkTitle: "Catalog query services in COBOL"
    weight: 340
---This service provides access to the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog entries, for
querying and modification, and enables you to sort the selected catalog
entries. Additionally, you can sort the current selection in memory.

> **Note**
>
> The communication structure
> in Transfer CFT enables you to recuperate catalog fields, such
> as an identifier, that exceed 8 to 32 characters.


| Function | Use |
| --- | --- |
| OPEN | Open catalog file<br/> This function:<br/> • Allocates the catalog file<br/> • Opens the file<br/> • Reserves an internal control block<br/> • Initializes the internal block parameter |
| SELECT | Specify the selection criteria<br/> This function:<br/> • Checks the syntax used<br/> • Stores the selection criteria in the internal control block |
| SELECT240 | Specify the selection criteria<br/> This function:<br/> • Is available in CFT v2.4 and higher<br/> • Retrieves identifiers that are longer than 8 to 32 characters<br/> • Checks the syntax used<br/> • Stores the selection criteria in the internal control block |
| NEXT | Read next entry in the catalog<br/> This function:<br/> • Reads the next entry<br/> • Sets the "catalog entry data" area<br/> The first call to this function must be preceded by a SELECT. |
| NEXT240 | Read next entry in the catalog<br/> This function:<br/> • Is available in CFT v2.4 and higher<br/> • Retrieves identifiers that are longer than 8 to 32 characters<br/> • Reads the next entry<br/> • Sets the "catalog entry data" area |
| MODIFY | Modify the state of the current catalog entry or delete this entry from the catalog<br/> This function:<br/> • Retrieves the last entry read from the internal control block<br/> • Checks the state of this entry<br/> • Sends the modification request to {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| SORT | Sort the selected catalog entries<br/> This function:<br/> • Close the catalog file<br/> • De-allocates the file<br/> • Frees the internal control block<br/> • Resets the internal control block parameter |
| DO | Execute the current selection and the requested sort in memory |
| CLOSE | Close catalog file |


<span id="Call Syntax"></span>

## Call syntax

```
CALL     "CFTI"    
USING     <verb>       <blk>    
  <param>    
<rc>
```

Where:

- &lt;verb> is
    the command that you want to process
- &lt;blk> is
    a **{{< TransferCFT/axwayvariablesComponentShortName >}} specific field that
    must not be changed by the application**
- &lt;param> is
    a character string of variable length that contains the command parameters.
    The end of the field is defined by a character initially set to low-value
- &lt;rc> is the
    return code

The available &lt;verbs> are listed in the following table.


| &lt;verb&gt; | Value | Service |
| --- | --- | --- |
| F-OPEN | OPEN | Open catalog |
| F-SELECT | SELECT | Define selection criteria |
| F-NEXT | NEXT | Read next entry |
| F-MODIFY | MODIFY | Modify catalog entry state |
| F-CLOSE | CLOSE | Close catalog |


The available &lt;param> are listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| F-OPEN | D-CAT | Path name or logical name of the catalog file. If the name is blank, Transfer CFTI uses a default name. |
| F-SELECT | Z-SEL | Selection criteria according to the format described in the "S**election data description"** in the ****cft.apicop**** file. If a field is blank or equal to binary zeros, it is considered not selective.<br/> This field can contain:<br/> • the size of the selection criteria field (SLENTGTH) and the size of the field supporting the catalog entry (CLENGTH) in order to avoid recompiling the application program if these two fields are extended.<br/> • the transfer start and end date (BDATE and EDATE) to select transfers performed between these two dates.<br/> • a mask with the special characters "?" and *". The "?" character replaces any character. The "*" character replaces a character string of any length.<br /> <br /> Examples:<br /> A*D replaces ABCD, ABCED or AID<br /> A??D replaces ABCD, AXYD or AQZD<br /> *CD replaces ABECD, YXZCDor TYUICD<br /> ?CD replaces ACD, XCD or ZCD<br /> ?B* replaces ABCDEF, XBZWEO or *KBWXCV<br /> ???? replaces ABCD, XYZW or HGFD |
| F-NEXT | Z-CAT | Next catalog entry according to the format described in the "S**election data description**" in the ****cft.apicop**** file.<br/> The length of this field is defined by the SELECT service. See the CLENGTH field in the Selection data description. |
| F-MODIFY | M-STATE | New state of a transfer to be placed in the catalog entry previously read:<br/> • ‘D’ at Disposal: only valid if the former state is H or K<br/> • ‘H’ Hold: only valid if the former state is D, C or K<br/> • ‘K’ Keep: only valid if the former state is D, C or H<br/> • ‘X’ eXecuted: only valid if the former state is T<br/> • ‘P’ Purge: deletes the catalog entry |
| F-CLOSE |   | No &lt;param&gt; needed |


## Return codes

The return codes listed below apply to all services.


| Mnemonic | Description |
| --- | --- |
| CAPI-NOERR | No error |
| CAPI-FUNC-UNDEF | Command not valid |
| CAPI-INT-BLK | &lt;blk&gt; parameter invalid |
| ****OPEN code only**** |   |
| CAPI-MEM-GET | Memory allocation error |
| CAPI-CAT-ALLOC | Catalog file allocation error |
| CAPI-CAT-OPEN | Catalog file opening problem |
| ****SELECT code only**** |   |
| CAPI-SEL-DIRECT | DIRECTION criterion incorrect |
| CAPI-SEL-TYPE | TYPE criterion incorrect |
| CAPI-SEL-STATE | STATE criterion incorrect |
| CAPI-CAT-EMPTY | Catalog empty |
| CAPI-SEL-DATE | EDATE value &lt; BDATE value |
| CAPI-SEL-FDATE | FDATE criterion incorrect |
| CAPI-SEL-CDATE | CDATE criterion incorrect |
| CAPI-SEL-BDATE | BDATE criterion incorrect |
| CAPI-SEL-DATE | ****EDATE criterion incorrect**** |
| ****NEXT code only**** |   |
| CAPI-CAT-EOF | End of catalog file |
| CAPI-CAT-READ | Catalog file read error |
| ****MODIFY code only**** |   |
| CAPI-MOD-OSTATE | State invalid |
| CAPI-MOD-NSTATE | Requested new state incorrect |
| CAPI-INT-ERR2 | Internal error |
| CAPIO-COM-OPEN | Communication medium opening error |
| CAPI-COM-WRITE | Communication medium write error |
| CAPI-COM-CLOSE | Communication medium closing problem |
| ****CLOSE code only**** |   |
| CAPI-MEM-FREE | Memory de-allocation error |
| CAPI-CAT-FREE | Catalog file de-allocation error |
| CAPI-CAT-CLOSE | Catalog file closing error |

