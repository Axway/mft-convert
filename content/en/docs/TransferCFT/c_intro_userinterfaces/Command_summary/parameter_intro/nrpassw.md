---
    "title": "nrpassw",
    "linkTitle": "nrpassw",
    "weight": "2320"
---
<span id="nrpassw"></span>

### nrpassw

#### CFTPART

**[NRPASSW = string]**

*string32*     SFTP

*string8*     ODETTE, PeSIT E

Partner sign-on password, authorizing a local site access right check.

The remote partner must submit this password to the local {{< TransferCFT/axwayvariablesComponentShortName  >}}, during the connection phase. On the remote partner end, you must declare this
password as the NSPASSW parameter of the CFTPART object
used in the transfer.

{{% TransferCFT/snippets/quotes_case_sensitive%}}
{{% TransferCFT/snippets/indirection_char%}}

 

[Return to Command index](../../)
