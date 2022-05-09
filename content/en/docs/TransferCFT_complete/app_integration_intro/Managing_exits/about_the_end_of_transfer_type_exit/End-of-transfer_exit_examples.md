{
    "title": "End-of-transfer  exit example",
    "linkTitle": "End-of-transfer exit example",
    "weight": "380"
}This page introduces the following end-of-transfer elements, Parameter
settings, Structure
in C language, User
program in C.

****Parameter settings****

```
/\* CFTPARM Card \*/
cftparm id = idparm0          ,
       
exiteot = exe1          ,
.....
.....
```

```
/\* CFTEXIT Card \*/
cftexit id = exe1          ,
     language = c          ,
     type = exec          ,
     prog = cftexie         ,
     parm = exe1          ,
     mode = replace
```

****Structure in C****

Refer to the ****exeus.h****
delivered with the {{< TransferCFT/axwayvariablesComponentShortName  >}} product samples.

****User program in C****

Refer to the ****exexmp1.c****
delivered with the {{< TransferCFT/axwayvariablesComponentShortName  >}} product samples.
