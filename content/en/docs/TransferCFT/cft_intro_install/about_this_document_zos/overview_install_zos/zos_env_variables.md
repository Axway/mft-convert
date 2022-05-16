---

    title: Define environment variables file
    linkTitle: Environment variables file
    weight: 190

---
You must define the environment variables in the <span class="code">`..UPARM (CNFENV)`</span> file with the DDname <span class="code">`STDENV`</span>.

> **Note**
>
> The ENVAR runtime option is: envar("\_CEE\_ENVFILE\_S=DD:STDENV".

This file must contain the following definition: <span class="code">`CFTUCONF=dd:UCONF `</span>(initialized when creating the instance - do not change this value).

You can modify this file to, for example, obtain traces, set the stack TCP/IP, etc.

When you start Transfer CFT, some parameters are printed in the transfer CFT LOG. For example:

```
CFTI18I+Environment variables (platform dependant) (DDname:STDENV)
CFTI18I+ CFTUCONF=dd:UCONF
CFTI18I+ _BPXK_SETIBMOPT_TRANSPORT=TCPIP
```
