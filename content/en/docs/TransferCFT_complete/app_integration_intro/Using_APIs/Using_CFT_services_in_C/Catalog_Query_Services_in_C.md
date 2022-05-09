{
    "title": " Transfer CFT  catalog query services",
    "linkTitle": "Catalog query services in C",
    "weight": "340"
}{{% TransferCFT/snippets/catquery_api%}}
<span id="Call_Syntax"></span>

Call syntax
-----------

These services enable you to query the catalog either with or without
specific criteria.

`rc = cftai (verb,&ptr,param)`

`rc = cftaix (verb,&ptr,param)`

Where:

- rc is the return
    code (int)
- verb is the command
    that you want to process (char \*)
- ptr contains the
    address of an internal control block (char \*) completed on return of an
    OPEN service call. It must be provided and defined to call other functions
- param points to
    the parameters specific to each function

The available verbs are listed in the following table.


| &lt;verb&gt; | Service |
| --- | --- |
| OPEN | Open catalog |
| SELECT | Define selection criteria |
| NEXT | Read next entry |
| MODIFY | Modify catalog entry state |
| SORT | ****cftaix only****<br/> ****Sort the selected catalog entries**** |
| DO | ****cftaix only****<br/> Do the current selection and the requested sort in memory |
| CLOSE | Close catalog |


The available &lt;param&gt; are listed in the following table.


| &lt;verb&gt; | &lt;param&gt; | Explanation |
| --- | --- | --- |
| OPEN | cat | Path name or logical name of the catalog file. If the name is blank, Transfer CFT uses a default name. |
| SELECT<br/> and SELECT240 | &amp;cftsel | Selection criteria according to the format described in the &quot;**Selection data description**&quot; in the ****cftapi.h**** file.<br/> • cftsel230T for SELECT service<br/> • cftsel240T for SELECT240 service<br/> All the fields must be defined by left-aligned character strings. If a field is equal to binary zeros, it is considered not selective.<br/> This structure can contain:<br/> • The size of the selection criteria field (slength) and the size of the field supporting the catalog entry (clength) in order to avoid recompiling the application program if these two fields are extended.<br/> • The transfer start and end date (BDATE and EDATE) to select transfers performed between these two dates.<br/> • A field can be composed of a mask with the special characters &quot;?&quot; and *&quot;. The &quot;?&quot; character replaces any character. The &quot;*&quot; character replaces a character string of any length.<br /> <br /> Examples:<br /> A*D replaces ABCD, ABCED or AID<br /> A??D replaces ABCD, AXYD or AQZD<br /> *CD replaces ABECD, YXZCDor TYUICD<br /> ?CD replaces ACD, XCD or ZCD<br /> ?B* replaces ABCDEF, XBZWEO or *KBWXCV<br /> ???? replaces ABCD, XYZW or HGFD<br/> You should initialize the following:<br/> • The param field to binary zero before defining it<br/> • The slength and clength by &quot;itoa()&quot;<br/> ****cftaix only****<br/> The selection is only taken into account at the time the DO service is called. |
| NEXT<br/> and<br/> NEXT240 | &amp;cftcat | Next catalog entry according to the format described in the &quot;**Selection data description**&quot; in the ****cftapi.h**** file.<br/> • cftcatT for NEXT service<br/> • cftcat240T for NEXT240 service |
| MODIFY | &amp;nstate | New state of a transfer to be placed in the catalog entry previously read:<br/> • ‘D’ at Disposal: only valid if the former state is H or K<br/> • ‘H’ Hold: only valid if the former state is D, C or K<br/> • ‘K’ Keep: only valid if the former state is D, C or H<br/> • ‘X’ eXecuted: only valid if the former state is T<br/> • ‘P’ Purge: deletes the catalog entry*. It is only valid if the current state is D, H, K, T or X |
| SORT | param | ****cftaix only****<br/> Sort options as described in the &quot;**Sort structure of the selected catalog entries**&quot; in the ****cftapi.h**** file.<br/> The function is only taken into account at the time the DO service is called. |
| DO | &quot; &quot; |   |
| CLOSE | &quot; &quot; |   |


Return codes
------------

The following return codes apply to all services.


| Mnemonic | Description |
| --- | --- |
| CAPI_NOERR | No error |
| CAPI_FUNC_UNDEF | Command not valid or service refused by the operating security system |
| CAPI_INT_PTR | &lt;ptr&gt; parameter invalid |
| ****OPEN code only**** |   |
| CAPI_MEM_GET | Memory allocation error |
| CAPI_CAT_ALLOC | ****cftai only****<br/> Catalog file allocation error |
| CAPI_CAT_OPEN | Catalog file opening problem |
| ****SELECT code only**** |   |
| CAPI_SEL_DIRECT | DIRECTION criterion incorrect |
| CAPI_SEL_TYPE | TYPE criterion incorrect |
| CAPI_SEL_STATE | STATE criterion incorrect |
| CAPI_CAT_EMPTY | Catalog empty |
| CAPI_CAT_SELECT | ****cftaix only****<br/> Selection incorrect |
| CAPI_SEL_DATE | EDATE value &lt; BDATE value |
| CAPI_SEL_FDATE | FDATE criterion incorrect |
| CAPI_SEL_CDATE | CDATE criterion incorrect |
| CAPI_SEL_BDATE | BDATE criterion incorrect |
| CAPI_SEL_DATE | ****EDATE criterion incorrect**** |
| ****NEXT code only**** |   |
| CAPI_CAT_EOF | End of catalog file |
| CAPI_CAT_READ | Catalog file read error |
| ****MODIFY code only**** |   |
| CAPI_CAT_MODIFY | ****cftaix only****<br/> Modification error |
| CAPI_MOD_OSTATE | State invalid |
| CAPI_MOD_NSTATE | Requested new state incorrect |
| CAPI_INT_ERR2 | Internal error |
| CAPIO_COM_OPEN | Communication medium opening error |
| CAPI_COM_WRITE | Communication medium write error |
| CAPI_COM_CLOSE | Communication medium closing problem |
| ****SORT code only<br /> (cftaix only)**** |   |
| CAPI_CATSORT | sort incorrect |
| ****DO code only<br /> (cftaix only)**** |   |
| CAPI_CAT_CLOSE | Error on closing the catalog |
| CAPI_CAT_OPEN | Error on opening the catalog |
| CAPI_CAT_EMPTY | Catalog empty or no record selected |
| CAPI_ERREXEC | Execution error |
| ****CLOSE code only**** |   |
| CAPI_MEM_FREE | Memory de-allocation error |
| CAPI_CAT_FREE | Catalog file de-allocation error |
| CAPI_CAT_CLOSE | Catalog file closing error |

