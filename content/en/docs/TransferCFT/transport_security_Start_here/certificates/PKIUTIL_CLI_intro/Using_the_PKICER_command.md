---
    "title": "Using  the PKICER command",
    "linkTitle": "Using PKICER",
    "weight": "230"
---
{{% TransferCFT/snippets/pkicer_intro%}}

Working with certificates
-------------------------

****Syntax****

```
PKICER
ID    
=     string,

[CHECK    
=     {YES &#124; NO},]
[COMMENT  
=     string,]
[IDATA     
=     string,]
[IFORM    
=     {PKCS12 &#124; DER &#124; PEM &#124; PKCS7,]
[IKDATA    
=     {base64/PEM},]
[IKFORM   
=     {DER &#124; PEM &#124; PKCS8},]
[IKNAME   
=     string,]
[IKPASSW  
=   string,]
[INAME    
=     string,]
[ITYPE    
=     {ALL &#124; USER &#124; ROOT &#124; INTER},]
[MODE    
=     {REPLACE &#124; CREATE &#124; DELETE},]
[STATE    
=     {ACT &#124; INACT},]
[ROOTCID  
=     string,]
[PKICER    = string,]

```
{{% TransferCFT/snippets/pkicer%}}
