{
    "title": "Define environment variables file",
    "linkTitle": "Environment variables file",
    "weight": "190"
}You must define the environment variables in the `..UPARM (CNFENV)` file with the DDname `STDENV`.

> **Note**
>
> Note: The ENVAR runtime option is: envar("_CEE_ENVFILE_S=DD:STDENV".

This file must contain the following definition: `CFTUCONF=dd:UCONF `(initialized when creating the instance - do not change this value).

You can modify this file to, for example, obtain traces, set the stack TCP/IP, etc.

When you start Transfer CFT, some parameters are printed in the transfer CFT LOG. For example:

```
CFTI18I+Environment variables (platform dependant) (DDname:STDENV)
CFTI18I+ CFTUCONF=dd:UCONF
CFTI18I+ _BPXK_SETIBMOPT_TRANSPORT=TCPIP
```
